---
layout: post
title: Getting started with PostgresCompare
excerpt: From first launch to a deployment script in six steps — try the demo, connect your databases, compare them, and generate the SQL to bring them in sync.
last_modified_at: 2026-09-15
---

This guide takes you from first launch to a deployment script. The steps are the same on Windows, macOS and Linux. For more detail on any step, see the [documentation](/docs/).

## 1. Try the demo

The welcome screen offers **Try the demo**. It opens a real comparison built from sample schemas, so you can explore the results and a deployment script before connecting anything of your own. The sample project is read-only — when you've seen enough, carry on with your own databases.

## 2. Add your connections

A connection holds the details PostgresCompare needs to reach a PostgreSQL server. You need one for each database you want to compare — typically a development or staging database, and production.

1. Click **Connections** in the sidebar, then **New connection**. From the welcome screen, **Connect my databases** takes you to the same place.
2. Give the connection a name, and enter the host, port, username, password and a default database.
3. Click **Test** to check PostgresCompare can connect, then **Save**.

If PostgresCompare finds PostgreSQL connections already set up on your machine, **Import from environment** adds them in one step.

A user with read access is enough to compare. You only need write access to the target when you deploy.

## 3. Create a project

A project pairs a **Source** with a **Target** and remembers your comparison options, so you can run the same comparison again whenever you need it.

1. Click **Schema projects** in the sidebar, then **New project**.
2. Name the project.
3. Under **Source** and **Target**, choose the connection, database and schema for each side. The source is the database with the changes you want to ship; the target is where they're going.
4. Save the project.

Projects are part of Pro, and included in your 14-day trial. On the free tier — or whenever you only need a one-off answer — use **Quick compare** in the sidebar instead. It compares two databases without creating a project, and lets you generate and deploy the script, but the result isn't saved.

## 4. Run the comparison

Open the project and click **Compare**. PostgresCompare reads both schemas and shows its progress as it goes.

When it finishes, the results list every object it checked, grouped by type and marked with a status:

- **New** — exists only in the source
- **Different** — exists on both sides, but has changed
- **Dropped** — exists only in the target
- **Identical** — the same on both sides

Select an object to see its source and target SQL side by side, with the changed lines highlighted. Filter by name, type or status to focus on what matters.

## 5. Generate a deployment script

Click **Generate deployment script…**. PostgresCompare writes the SQL that makes the target match the source, ordered so that dependencies are created before the objects that need them, and wrapped in a transaction.

- Every change is listed with a checkbox. Untick anything you don't want to deploy — selecting a change also selects the statements it depends on.
- Destructive statements, such as dropping a table or a column, are flagged in red before anything else.
- Copy or save the script to run it yourself, or deploy it from PostgresCompare.

## 6. Deploy

Click **Run script…** to deploy to the target. A confirmation shows the target connection and database, and highlights any destructive statements, before anything runs. Each statement reports its progress as it executes.

Prefer your own tooling? Save the script and run it with psql, your migration tool or your CI pipeline instead.

Whichever route you take, read the script first, and try it against a non-production copy before it goes near production.

## Next steps

- [Understanding comparison results](/docs/getting-started/first-comparison/) — the results view in detail
- [Deployment scripts](/docs/guides/deployment-scripts/) — script structure, pre- and post-deploy scripts, and statement selection
- [Safe deployments](/docs/guides/safe-deployments/) — checklists, backups and rollback planning
- Stuck, or something doesn't match what you see? [Get in touch](/contact).
