---
title: Comparing Databases
description: Advanced techniques for comparing PostgreSQL databases
category: guides
order: 4
permalink: /docs/guides/comparing-databases/
---

This guide covers advanced techniques for comparing PostgreSQL databases with PostgresCompare.

## Comparison Modes

PostgresCompare supports several comparison scenarios:

### Database to Database
Compare two live PostgreSQL databases. This is the most common scenario for:
- Comparing development to production
- Comparing feature branches
- Auditing database drift

### Database to Snapshot
Compare a live database against a saved snapshot. Useful for:
- Tracking changes over time
- Comparing against a known baseline
- Working offline

### Snapshot to Snapshot
Compare two saved snapshots. Ideal for:
- Historical comparison
- Comparing databases without live access
- Documentation and auditing

## Working with Snapshots

### Creating a Snapshot

1. Connect to a database
2. Click **Create Snapshot** in the toolbar
3. Choose a location and name
4. Wait for the snapshot to complete

Snapshots capture the complete schema definition and can be shared with team members.

### Using Snapshots

1. In the source or target connection dialog, select **Snapshot**
2. Browse to the `.pgs` snapshot file
3. Continue with comparison as normal

## Schema Filtering

By default, PostgresCompare compares all schemas. To focus on specific schemas:

1. Open **Project Settings**
2. Navigate to **Schema Filters**
3. Add schemas to include or exclude

### Include vs. Exclude

- **Include list**: Only compare schemas in this list
- **Exclude list**: Compare all schemas except those in this list

Common schemas to exclude:
- `pg_catalog` - PostgreSQL system catalog
- `information_schema` - SQL standard metadata
- `pg_toast` - TOAST storage

## Object Type Filtering

Filter which object types are compared:

1. Open **Project Settings**
2. Navigate to **Object Types**
3. Toggle object types on or off

Available object types:
- Tables
- Views
- Materialized Views
- Functions
- Procedures
- Triggers
- Indexes
- Sequences
- Types
- Domains
- Extensions
- Schemas

## Comparison Options

Fine-tune how objects are compared:

### Ignore Options

- **Ignore tablespace** - Treat objects as equal even if tablespace differs
- **Ignore storage parameters** - Ignore FILLFACTOR, autovacuum settings, etc.
- **Ignore owner** - Don't compare object ownership
- **Ignore permissions** - Don't compare GRANT/REVOKE statements

### Script Options

- **Include IF EXISTS** - Add IF EXISTS to DROP statements
- **Include IF NOT EXISTS** - Add IF NOT EXISTS to CREATE statements
- **Quote identifiers** - Always quote table and column names

## Comparing Large Databases

For databases with thousands of objects:

### Performance Tips

1. **Use schema filters** - Compare only relevant schemas
2. **Exclude large tables** - If you only care about structure
3. **Use snapshots** - Avoid re-reading the same database

### Memory Management

PostgresCompare loads schema metadata into memory. For very large databases:

1. Close other applications to free memory
2. Consider comparing schemas separately
3. Use command-line for automated comparison

## Multi-Database Comparison

To compare the same source against multiple targets:

1. Create a project for each target database
2. Use the same source connection in each project
3. Run comparisons separately

Or use the CLI for automated batch comparison:

```bash
pgcompare --source production.pgs --target staging --output staging-diff.sql
pgcompare --source production.pgs --target development --output dev-diff.sql
```

## Tracking Changes Over Time

Use snapshots to track schema changes:

1. Create a baseline snapshot of your production database
2. Schedule regular snapshots (weekly, after releases, etc.)
3. Compare snapshots to see what changed

This creates an audit trail of schema changes independent of your version control.

## Next Steps

- [Generating deployment scripts](/docs/guides/deployment-scripts/) - Create SQL scripts from comparisons
- [Using ignore rules](/docs/guides/ignore-rules/) - Fine-tune what gets compared
- [CI/CD integration](/docs/guides/ci-cd-integration/) - Automate comparisons in your pipeline
