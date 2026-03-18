---
layout: post
title: "How to Compare Two PostgreSQL Databases (Step-by-Step Guide)"
excerpt: "Learn three ways to compare PostgreSQL databases: using a dedicated comparison tool, pgAdmin Schema Diff, or pg_dump. Find schema differences in minutes, not hours."
---

Comparing two PostgreSQL databases is one of the most common tasks in database development — checking whether a migration applied correctly, finding out how dev has drifted from production, or verifying that staging matches what you intended to deploy.

There are three practical approaches: a dedicated schema comparison tool, pgAdmin's built-in Schema Diff, or exporting schemas with `pg_dump` and diffing the output. Each fits a different situation.

## Method 1: PostgresCompare (Recommended for Regular Use)

PostgresCompare connects directly to both databases, compares their schemas, and shows you a structured list of every difference — objects that exist only on one side, and objects that differ between the two. From there you can generate a deployment script to bring them in sync.

### Step 1: Add your connections

Open PostgresCompare and add a connection for each database. Each connection needs a host, port, database name, username, and password. Connections are stored in the app and reused across comparisons.

### Step 2: Create a project and comparison

Create a project (a container for related comparisons — for example, all comparisons for a given application) and add a new comparison. Select the X environment (the source — typically dev or staging) and the Y environment (the target — typically production).

### Step 3: Run the comparison

Click **Compare**. PostgresCompare connects to both databases, reads their schema metadata, and produces a results list grouped by object type. Each object is marked as:

- **Identical** — the same in both databases
- **Different** — exists in both but the definitions differ
- **New** — exists only in X (will need to be created in Y)
- **Dropped** — exists only in Y (will need to be removed from Y)

### Step 4: Review the differences

Click any object in the list to see a side-by-side SQL diff showing exactly what changed. The **Changes tab** gives a plain-English summary — which columns were added or removed, which constraints changed — without having to read raw DDL. Dependency chips show related objects and their status, so you can understand the knock-on effects of a change before acting on it.

### Step 5: Generate a deployment script

Select the changes you want to deploy using the checkboxes. Click **Generate Script** to produce a dependency-ordered SQL script. The script includes `RAISE NOTICE` progress statements so you can monitor execution. Destructive statements (`DROP TABLE`, `DROP COLUMN`) are flagged in red so nothing gets missed.

PostgresCompare runs entirely on your machine. Neither database connection nor schema data is sent anywhere.

---

## Method 2: pgAdmin Schema Diff (Quick, No Extra Tools)

pgAdmin 4 includes a built-in Schema Diff tool. It's free, requires no additional installation if you already use pgAdmin, and covers the most common object types.

### Step 1: Open Schema Diff

In pgAdmin, go to **Tools → Schema Diff**. The Schema Diff panel opens as a new tab.

### Step 2: Select the two databases

Use the dropdowns to select the source and target databases. You can compare across different servers — the servers just need to be registered in pgAdmin.

### Step 3: Click Compare

pgAdmin connects to both databases and produces a list of differences. Each row shows the object name, type, and status.

### Step 4: Review the SQL diff

Click an object to see the SQL diff in the panel below. This shows the DDL for the object on each side.

### Step 5: Generate a script

Click **Generate Script** to produce SQL that synchronises the target to match the source. Copy it to a query tool and run it manually.

**Limitations to be aware of:** pgAdmin Schema Diff covers the most common object types but not all of them. There's no saved comparison state — each session starts fresh. The generated script needs to be run manually outside of pgAdmin.

---

## Method 3: pg_dump and diff (No GUI Required)

If you prefer the command line or need to compare schemas in a script, you can dump both schemas to files and diff them. This requires no additional tools beyond `pg_dump` and a diff utility.

### Step 1: Dump both schemas

```bash
pg_dump \
  --schema-only \
  --no-owner \
  --no-acl \
  --no-comments \
  -h db1.example.com \
  -U postgres \
  mydb > schema_a.sql

pg_dump \
  --schema-only \
  --no-owner \
  --no-acl \
  --no-comments \
  -h db2.example.com \
  -U postgres \
  mydb > schema_b.sql
```

The `--no-owner`, `--no-acl`, and `--no-comments` flags reduce noise in the diff output. Without them, irrelevant differences in ownership and comments can obscure the real changes.

### Step 2: Sort the output

`pg_dump` does not guarantee a consistent object ordering across runs or databases. Before diffing, sort the output to align equivalent objects:

```bash
sort schema_a.sql > schema_a_sorted.sql
sort schema_b.sql > schema_b_sorted.sql
```

This is a rough sort — it works well enough for a quick check but may produce false positives where objects are genuinely identical but formatted differently.

### Step 3: Diff the files

```bash
diff schema_a_sorted.sql schema_b_sorted.sql
```

Or for a more readable output:

```bash
diff --unified schema_a_sorted.sql schema_b_sorted.sql | less
```

**Limitations to be aware of:** The `pg_dump` approach works for a quick check but has significant practical limitations. The output ordering is not stable, so sorting introduces noise. Whitespace differences in function bodies produce spurious diffs. There's no way to select a subset of changes to deploy — the output is a raw text diff, not structured schema data. For anything beyond a quick sanity check, this approach becomes difficult to work with.

---

## Which Method to Use

| Method | Best for | Limitations |
|--------|----------|-------------|
| PostgresCompare | Regular comparisons, deployments, team workflows | Paid (30-day free trial) |
| pgAdmin Schema Diff | Quick checks, already using pgAdmin | No history, manual deployment, limited object types |
| pg_dump + diff | One-off checks, scripting, no GUI | Noisy output, no deployment path, ordering issues |

The `pg_dump` approach is fine for a quick sanity check when you have no other tools available. pgAdmin Schema Diff is a reasonable step up if you're already in pgAdmin and just need to see SQL differences. For anything where you need to act on the results — generate a deployment script, track changes over time, or manage multiple environments — a dedicated tool is worth it.

---

[Download PostgresCompare](/downloads/) and run your first comparison in minutes — free for 30 days, no credit card required.
