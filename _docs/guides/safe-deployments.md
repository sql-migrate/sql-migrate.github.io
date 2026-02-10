---
title: Safe Deployments
description: Best practices for deploying database changes safely
category: guides
order: 6
permalink: /docs/guides/safe-deployments/
---

Deploying database schema changes requires careful planning. This guide covers best practices for safe, reliable deployments using PostgresCompare.

## Pre-Deployment Checklist

Before running any deployment script:

- [ ] Review all changes in PostgresCompare
- [ ] Understand the impact of each change
- [ ] Test on a non-production database
- [ ] Create a backup of the target database
- [ ] Schedule deployment during low-traffic periods
- [ ] Notify affected teams
- [ ] Have a rollback plan ready

## Understanding Destructive Operations

Some operations can cause data loss. PostgresCompare marks these clearly:

### High-Risk Operations

| Operation | Risk | Mitigation |
|-----------|------|------------|
| DROP TABLE | Data loss | Backup first, consider RENAME |
| DROP COLUMN | Data loss | Export data first |
| ALTER TYPE (narrowing) | Data truncation | Verify data fits new type |
| DROP INDEX | Performance impact | Monitor after deployment |
| DROP FUNCTION | Application errors | Verify no callers |

### Medium-Risk Operations

| Operation | Risk | Mitigation |
|-----------|------|------------|
| ADD NOT NULL | Insert failures | Provide default value |
| ADD UNIQUE | Constraint violations | Check for duplicates |
| ADD FOREIGN KEY | Constraint violations | Verify referential integrity |

<div class="warning">
<strong>Warning:</strong> Always review the generated script carefully. PostgresCompare shows warnings for destructive operations, but understanding your specific data is essential.
</div>

## Testing Strategy

### Development Testing

1. Generate script from PostgresCompare
2. Apply to a development database
3. Run application tests
4. Verify data integrity

### Staging Testing

1. Restore production backup to staging
2. Apply deployment script
3. Run integration tests
4. Measure performance impact
5. Practice rollback procedure

### Production Dry Run

Some changes support dry run mode:

```sql
-- Start a transaction
BEGIN;

-- Apply changes
ALTER TABLE public.users ADD COLUMN preferences JSONB;
CREATE INDEX idx_users_preferences ON public.users USING gin(preferences);

-- Verify changes
\d public.users

-- Rollback instead of commit
ROLLBACK;
```

## Backup Strategies

### Before Deployment

```bash
# Full database backup
pg_dump -Fc production_db > backup_$(date +%Y%m%d_%H%M%S).dump

# Schema-only backup (faster)
pg_dump -Fc --schema-only production_db > schema_backup.dump

# Specific tables
pg_dump -Fc -t users -t orders production_db > tables_backup.dump
```

### Point-in-Time Recovery

For critical databases, enable WAL archiving:

```sql
-- postgresql.conf
archive_mode = on
archive_command = 'cp %p /archive/%f'
```

This allows recovery to any point in time.

## Deployment Patterns

### Blue-Green Deployment

1. Create a copy of the production database
2. Apply changes to the copy
3. Verify everything works
4. Switch application to new database
5. Keep old database as rollback option

### Rolling Deployment

For large tables, apply changes incrementally:

```sql
-- Add column as nullable first
ALTER TABLE public.large_table ADD COLUMN new_field VARCHAR(100);

-- Backfill in batches
DO $$
DECLARE
    batch_size INT := 10000;
    affected INT := 1;
BEGIN
    WHILE affected > 0 LOOP
        WITH batch AS (
            SELECT id FROM public.large_table
            WHERE new_field IS NULL
            LIMIT batch_size
            FOR UPDATE SKIP LOCKED
        )
        UPDATE public.large_table t
        SET new_field = 'default'
        FROM batch
        WHERE t.id = batch.id;

        GET DIAGNOSTICS affected = ROW_COUNT;
        COMMIT;
    END LOOP;
END $$;

-- Add NOT NULL constraint
ALTER TABLE public.large_table ALTER COLUMN new_field SET NOT NULL;
```

### Expand-Contract Pattern

For breaking changes:

1. **Expand**: Add new structures alongside old
2. **Migrate**: Update application to use new structures
3. **Contract**: Remove old structures

```sql
-- Step 1: Expand
ALTER TABLE public.users ADD COLUMN full_name VARCHAR(200);
UPDATE public.users SET full_name = first_name || ' ' || last_name;

-- Step 2: Deploy application using full_name

-- Step 3: Contract (after all apps updated)
ALTER TABLE public.users DROP COLUMN first_name;
ALTER TABLE public.users DROP COLUMN last_name;
```

## Rollback Planning

### Transaction-Based Rollback

Wrap scripts in transactions:

```sql
BEGIN;

-- Changes here

-- If something goes wrong:
ROLLBACK;

-- If everything is good:
COMMIT;
```

### Script-Based Rollback

Generate reverse scripts:

```sql
-- rollback_2024-01-15.sql

-- Reverse: ALTER TABLE public.users ADD COLUMN preferences JSONB
ALTER TABLE public.users DROP COLUMN preferences;

-- Reverse: CREATE INDEX idx_users_preferences
DROP INDEX IF EXISTS idx_users_preferences;
```

### Backup-Based Rollback

```bash
# Restore from backup
pg_restore -d production_db backup_20240115.dump
```

## Monitoring Deployments

### During Deployment

Monitor for:
- Lock wait events
- Long-running queries
- Error messages
- Connection count

```sql
-- Check for blocking locks
SELECT * FROM pg_stat_activity WHERE wait_event_type = 'Lock';

-- Check for long queries
SELECT * FROM pg_stat_activity WHERE state = 'active'
    AND query_start < NOW() - INTERVAL '1 minute';
```

### After Deployment

Verify:
- Application functionality
- Query performance
- Error rates
- Database metrics

## Emergency Procedures

### If Deployment Fails

1. **Don't panic** - Assess the situation
2. **Check error messages** - Understand what failed
3. **Rollback if needed** - Use prepared rollback plan
4. **Document the issue** - For post-mortem

### If Performance Degrades

1. **Identify slow queries** - Use `pg_stat_statements`
2. **Check new indexes** - May need REINDEX
3. **Analyze tables** - Run ANALYZE on affected tables
4. **Consider rollback** - If impact is severe

## Using PostgresCompare for Safe Deployments

PostgresCompare provides several features that support safe deployment workflows:

### Direct Deployment with Progress Tracking

Use the **Deploy** button to apply changes directly to the target database. PostgresCompare provides real-time progress tracking so you can monitor each statement as it executes.

### Deployment History

PostgresCompare tracks all deployments within the project, creating an audit trail of what was changed and when.

### Post-Deployment Recompare

After deploying, run a new comparison to verify the deployment was successful. This confirms the target database matches the expected state and catches any issues immediately.

## Next Steps

- [Deployment scripts](/docs/guides/deployment-scripts/) - Customize script generation
- [Troubleshooting](/docs/troubleshooting/common-issues/) - Common issues during deployment
