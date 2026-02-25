---
layout: post
title: "How to Safely Rename a Column in PostgreSQL"
excerpt: "Renaming a column sounds simple, but on a live system it can break your application instantly. Here's how to do it safely without downtime."
---

Renaming a column in PostgreSQL takes a single command. But on a production system with a live application, that single command can break everything in an instant.

This article walks through why it's risky and how to do it safely.

## The Simple Way (and Why It's Dangerous)

The straightforward approach is:

```sql
ALTER TABLE users RENAME COLUMN fname TO first_name;
```

This works perfectly in isolation. The problem is that your application is almost certainly referencing the old column name. The moment you run this command, every query that mentions `fname` will fail with:

```
ERROR: column "fname" does not exist
```

There's no grace period. No deprecation window. The old name is gone immediately.

`ALTER TABLE` also acquires an **ACCESS EXCLUSIVE** lock, which blocks all reads and writes on the table for the duration of the operation. On a busy table this can cause a queue of waiting queries to build up very quickly.

## When the Simple Way Is Fine

Before reaching for a more complex approach, consider whether you actually need one.

The simple rename is acceptable when:

- You're in a maintenance window with traffic stopped
- The table is only accessed by a single, deployable application (so you can rename the column and deploy the code change atomically)
- The table is small and internal, with no external consumers

If any of those conditions apply, `ALTER TABLE ... RENAME COLUMN` is the right tool. The multi-step approach below is for situations where you cannot afford downtime or cannot coordinate a simultaneous code and schema deploy.

## The Safe Approach: Expand, Migrate, Contract

The expand-contract pattern lets you rename a column without any downtime. The idea is to run both column names in parallel until the old one has no consumers left.

### Step 1: Add the New Column

Add the new column alongside the old one. If the old column has a `NOT NULL` constraint, the new column should be nullable for now to avoid a table rewrite.

```sql
ALTER TABLE users ADD COLUMN first_name VARCHAR(100);
```

### Step 2: Backfill Existing Data

Copy the data from the old column to the new one. For large tables, do this in batches to avoid locking rows for too long.

```sql
-- Small tables: single update
UPDATE users SET first_name = fname;

-- Large tables: batch update to reduce lock duration
DO $$
DECLARE
  batch_size INT := 10000;
  last_id    BIGINT := 0;
BEGIN
  LOOP
    UPDATE users
    SET    first_name = fname
    WHERE  id > last_id
      AND  id <= last_id + batch_size
      AND  first_name IS NULL;

    EXIT WHEN NOT FOUND;
    last_id := last_id + batch_size;
    PERFORM pg_sleep(0.01); -- brief pause between batches
  END LOOP;
END $$;
```

### Step 3: Keep Them in Sync with a Trigger

While your application is being updated, writes will still go to the old column. A trigger keeps both columns in sync during the transition.

```sql
CREATE OR REPLACE FUNCTION sync_first_name()
RETURNS TRIGGER AS $$
BEGIN
  IF NEW.fname IS DISTINCT FROM OLD.fname THEN
    NEW.first_name := NEW.fname;
  END IF;
  IF NEW.first_name IS DISTINCT FROM OLD.first_name THEN
    NEW.fname := NEW.first_name;
  END IF;
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_sync_first_name
BEFORE INSERT OR UPDATE ON users
FOR EACH ROW EXECUTE FUNCTION sync_first_name();
```

### Step 4: Deploy the Application Update

Update your application to read from and write to `first_name` instead of `fname`. Deploy this while the trigger is keeping both columns in sync.

At this point:
- Old application instances still use `fname` — the trigger keeps `first_name` up to date
- New application instances use `first_name` — the trigger keeps `fname` up to date

Once the deployment is complete and you've confirmed no references to `fname` remain, move to the final step.

### Step 5: Clean Up

Drop the trigger and the old column.

```sql
DROP TRIGGER trg_sync_first_name ON users;
DROP FUNCTION sync_first_name();

ALTER TABLE users DROP COLUMN fname;
```

If you need the `NOT NULL` constraint on the new column, add it now:

```sql
ALTER TABLE users ALTER COLUMN first_name SET NOT NULL;
```

## Summary

| Approach | When to use |
|----------|-------------|
| `ALTER TABLE RENAME COLUMN` | Maintenance window, or app and schema can be deployed atomically |
| Expand-contract pattern | Zero-downtime rename on a live, high-traffic system |

The expand-contract pattern takes more steps, but it gives you full control over the transition. You can validate data at each stage and roll back at any point before the cleanup step.

## Reviewing Rename Operations with PostgresCompare

When working across multiple environments — dev, staging, production — column renames are a common source of schema drift. A rename applied to dev but not yet to production shows up as a column present on one side and missing on the other.

[PostgresCompare](/) surfaces these differences clearly in its side-by-side comparison view, letting you see exactly what changed and generate the appropriate migration script before anything reaches production.

[Download PostgresCompare](/downloads/) to keep your schemas in sync across environments.
