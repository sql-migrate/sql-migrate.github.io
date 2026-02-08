---
layout: post
title: "Destructive PostgreSQL Operations: A Reference Guide"
excerpt: "A comprehensive reference of PostgreSQL operations that can cause data loss or service outages, and safer alternatives."
---

When working with PostgreSQL schema migrations, understanding which operations are destructive is critical. This reference guide documents operations that can cause **data loss** or **service outages**, helping you review deployment scripts with confidence.

## What Makes an Operation "Destructive"?

Destructive operations fall into two categories:

1. **Data Loss** - Operations that permanently delete or corrupt data with no built-in recovery
2. **Outage Risk** - Operations that acquire locks blocking other queries, potentially causing service degradation or downtime

Both require careful review before execution, especially in production environments.

## Data Loss Operations

These operations can permanently destroy data. Always ensure you have verified backups before executing them.

| Operation | Risk Level | Description |
|-----------|------------|-------------|
| `DROP TABLE` | Critical | Permanently deletes table and all data |
| `DROP DATABASE` | Critical | Destroys entire database |
| `DROP SCHEMA CASCADE` | Critical | Drops schema and all contained objects |
| `TRUNCATE TABLE` | Critical | Removes all rows instantly, cannot be rolled back outside a transaction |
| `DELETE` (no WHERE) | Critical | Deletes all rows |
| `UPDATE` (no WHERE) | High | Overwrites all rows with new values |
| `ALTER TABLE DROP COLUMN` | High | Removes column and its data permanently |
| `ALTER TABLE ALTER TYPE` | Medium | Type conversion can fail or truncate data |

### Notes on Data Loss Operations

**DROP vs DELETE**: `DROP TABLE` removes the table structure entirely, while `DELETE` only removes rows. Both are destructive, but `DROP` also eliminates indexes, constraints, and permissions.

**TRUNCATE vs DELETE**: `TRUNCATE` is faster because it doesn't scan rows, but it cannot be rolled back once committed and doesn't fire row-level triggers.

**Type Changes**: `ALTER TABLE ALTER TYPE` can silently truncate data. For example, changing `VARCHAR(100)` to `VARCHAR(50)` will truncate longer values without warning.

## Outage-Causing Operations

These operations acquire locks that block other queries. On busy tables, they can cause cascading lock waits and service degradation.

| Operation | Lock Type | Impact |
|-----------|-----------|--------|
| `ALTER TABLE` | ACCESS EXCLUSIVE | Blocks all reads and writes |
| `CREATE INDEX` | SHARE | Blocks writes, allows reads |
| `DROP INDEX` | ACCESS EXCLUSIVE | Brief but blocks everything |
| `VACUUM FULL` | ACCESS EXCLUSIVE | Blocks all access during operation |
| `REINDEX` | ACCESS EXCLUSIVE | Blocks all access |
| `CLUSTER` | ACCESS EXCLUSIVE | Rewrites entire table, blocks all access |
| `LOCK TABLE` | Varies | Explicit blocking based on lock mode |

### Understanding Lock Impact

**ACCESS EXCLUSIVE** is the most restrictive lock. Any operation requiring this lock will block all other operations on the table, including simple `SELECT` queries.

**SHARE locks** allow concurrent reads but block writes. This means your application can still read data but inserts and updates will queue up.

The danger isn't just the lock itself - it's the cascading effect. A single `ALTER TABLE` waiting for a long-running query can cause all subsequent queries to queue behind it.

## Safer Alternatives

PostgreSQL provides concurrent variants for several operations that minimise locking impact.

| Instead of... | Use... | Notes |
|---------------|--------|-------|
| `CREATE INDEX` | `CREATE INDEX CONCURRENTLY` | Allows writes during index creation |
| `DROP INDEX` | `DROP INDEX CONCURRENTLY` | Reduces lock duration |
| `REINDEX` | `REINDEX CONCURRENTLY` | PostgreSQL 12+ only |
| `VACUUM FULL` | Regular `VACUUM` + `pg_repack` | Reclaims space without exclusive locks |
| `ALTER TABLE ADD COLUMN NOT NULL` | Add nullable column, backfill, add constraint | Avoids full table rewrite |

### Adding NOT NULL Columns Safely

Instead of:
```sql
ALTER TABLE users ADD COLUMN status VARCHAR(20) NOT NULL DEFAULT 'active';
```

Use this multi-step approach:
```sql
-- Step 1: Add nullable column with default (fast, minimal locking)
ALTER TABLE users ADD COLUMN status VARCHAR(20) DEFAULT 'active';

-- Step 2: Backfill any NULL values (can be done in batches)
UPDATE users SET status = 'active' WHERE status IS NULL;

-- Step 3: Add NOT NULL constraint
ALTER TABLE users ALTER COLUMN status SET NOT NULL;
```

In PostgreSQL 11+, adding a column with a non-volatile default is optimised and doesn't rewrite the table, but the pattern above remains useful for complex migrations.

## Using PostgresCompare for Safe Deployments

[PostgresCompare](/) helps you identify destructive operations before they reach production:

- **Visual highlighting** of DROP, TRUNCATE, and other destructive statements
- **Side-by-side comparison** showing exactly what will change
- **Script review workflow** to catch issues before deployment

When reviewing generated migration scripts, pay particular attention to any operation listed in this guide. A few seconds of review can prevent hours of recovery work.

[Download PostgresCompare](/downloads/) to add this safety net to your deployment workflow.
