---
layout: post
title: "PostgresCompare vs Redgate pgCompare"
excerpt: "Both tools compare PostgreSQL schemas. One deploys directly with safety checks and runs 100% locally. See the full feature and pricing comparison."
---

Redgate built SQL Compare into the de facto standard for SQL Server schema comparison. It's a trusted tool across a huge number of enterprise teams. In 2024 they launched pgCompare — a PostgreSQL equivalent, available in a free Community edition and a paid Standard edition.

It's a credible new entry in the space, and for teams already in the Redgate ecosystem it will be a familiar proposition. But it's a v1.0 product, and the gap between what it does today and what a dedicated PostgreSQL comparison tool offers is significant.

This article looks at both tools across the workflows that matter.

## Redgate pgCompare: What It Does Well

pgCompare covers the comparison fundamentals: connect to two PostgreSQL databases, compare their schemas, view a side-by-side SQL diff, and generate a synchronisation script. The dependency-aware script generation is solid — objects are ordered correctly so that a generated script can be run without manual reordering.

The Community edition is free, with no revenue limit for students, educators, and open-source projects (and a generous threshold for small businesses). For teams that need a no-cost comparison tool and are comfortable running SQL scripts manually, that's a reasonable starting point.

It runs on Windows, macOS, and Linux.

<!-- SCREENSHOT: Redgate pgCompare main diff view — two databases selected, list of differences shown, SQL diff panel on the right -->

## The Comparison, Feature by Feature

### Comparison sources

pgCompare requires two live PostgreSQL database connections. There is no way to compare against a schema file or snapshot — both sides must be live at the time of comparison. Redgate's roadmap lists Snapshots support (using a Redgate Snapshot file as a source or target) as a planned Standard edition feature, but it is not yet available.

PostgresCompare supports live database connections and **pg_dump SQL files** as sources. Either side of a comparison — X or Y — can be a live database or a pg_dump file imported from disk. This means you can compare a live database against a schema dump taken at a point in time, compare two dump files directly, or use a dump as a baseline when you don't have access to a second live environment. The project settings include a toggle between "live database" and "SQL file" per environment.

### Schema diff basics

Both tools compare PostgreSQL schemas and produce a list of differences. pgCompare covers the most common object types — tables, views, functions, sequences, indexes, triggers, constraints, and procedures. PostgresCompare supports 38 object types, including less common ones like event triggers, foreign data wrappers, publications, subscriptions, operator families, and text search configuration objects.

For most comparisons this won't matter. For teams managing complex schemas the broader coverage is useful.

### Reading the diff

pgCompare shows a side-by-side SQL diff for each object — the before and after DDL displayed in a split view.

PostgresCompare gives you the same SQL diff, with line-level highlighting for child object changes within a table (individual columns, constraints, indexes) so you can see exactly what changed rather than scanning the full DDL. The editor includes a navigator for jumping between changed sections and a word wrap toggle for long function bodies.

The Changes tab, added in 1.1.107, provides a human-readable summary alongside the SQL view — structured descriptions of what changed rather than raw DDL. For pre-deployment review this is considerably faster than reading SQL.

<!-- SCREENSHOT: PostgresCompare Changes tab alongside the SQL diff editor, showing a human-readable breakdown of a table's modifications -->

### Projects and organisation

pgCompare comparisons are session-based. There is no concept of a saved project, named comparison, or persistent state between sessions.

PostgresCompare organises work into projects, each containing named comparisons. If you run the same dev-to-staging and dev-to-production comparisons regularly, you create them once and re-run them as needed — and multiple comparisons can run simultaneously, so there's no waiting for one to finish before starting the next. Comparisons you return to often can be starred for quick access. A global search bar finds projects, comparisons, and individual objects from anywhere in the app.

### Comparison history

pgCompare has no history. Each session starts fresh.

PostgresCompare records every comparison automatically. You can select any two historical runs and view a swimlane showing how objects moved between states — which differences were fixed, which are new, which have changed since the last run. Individual objects have their own history tab, redesigned in 1.2 with a timeline view and a Changes tab showing a plain-English summary of how that object evolved across comparisons. This makes it straightforward to track schema drift over time and to verify that a deployment had exactly the intended effect.

### Dependency tracking

pgCompare has no dependency tracking in the diff view.

PostgresCompare surfaces object dependencies directly in the comparison list. Each object shows colour-coded chips for the objects it depends on and the objects that reference it, with each chip showing the related object's status. Clicking a chip jumps to that object. This makes it easy to understand the knock-on effects of a change before generating a script.

### Filtering and search

pgCompare's filtering options are limited. You can focus on specific object types but there's no fine-grained control over how objects are compared.

PostgresCompare has 10 ignore toggles — whitespace, column order, privileges, owner, tablespace, and others — that let you tune the comparison to your workflow. All 38 object types can be enabled or disabled in project settings. The 1.1.107 release adds per-object-type name filters, so you can search within a specific type (for example, showing only tables whose names contain "order") without affecting the rest of the results.

### Deployment

pgCompare generates SQL scripts. Deployment is a manual step — you take the generated script and run it yourself against the target database. There are no in-tool deployment controls, confirmation prompts, or progress tracking.

PostgresCompare has a built-in deployment workflow. Every statement in the generated script is classified by risk: **Destructive** statements (DROP TABLE, DROP COLUMN) are highlighted red; **Warning** statements (DROP FUNCTION, DROP VIEW) are highlighted amber. A summary banner shows the total count before you do anything.

Clicking **Run script…** shows a confirmation dialog with the target connection name, database, and a restatement of the risk counts. The Run button turns red when destructive changes are present. Pre-deploy and post-deploy scripts can be attached to each project. The 1.1.107 release improves deployment progress logging, giving real-time per-statement feedback as the script executes.

<!-- SCREENSHOT: PostgresCompare deploy confirmation dialog — target database name, destructive statement count, Run button in red -->

### Export and reporting

pgCompare's export and reporting capabilities are listed as a roadmap feature for the Standard edition and are not fully available in the current release.

PostgresCompare can export comparison results in six formats today: Excel (with a summary sheet and doughnut chart), HTML (self-contained with filterable status pills), PDF, JSON (with a companion schema file for CI/CD validation), Markdown, and CSV.

## Summary

| Feature | Redgate pgCompare | PostgresCompare |
|---------|-------------------|-----------------|
| Basic diff | Yes | Yes |
| Object types | Common types | 38 |
| pg_dump / file as source | No (Snapshots on roadmap) | Yes |
| Readable diff summary | No | Yes (Changes tab, 1.1.107) |
| Free edition | Yes (Community) | 30-day trial |
| Projects and organisation | No | Yes |
| Simultaneous comparisons | No | Yes |
| Starred comparisons | No | Yes |
| Global search | No | Yes |
| Comparison history | No | Yes |
| Dependency tracking | No | Yes |
| Name filters | No | Yes (1.1.107) |
| Direct deployment | No | Yes |
| Deployment safety checks | No | Yes |
| Destructive warnings | No | Yes |
| Progress notifications | No | Yes (1.1.107) |
| Export reports | Roadmap | Yes (6 formats) |

## When Redgate pgCompare Is Worth Considering

- You need a free tool and the Community edition fits your situation
- Your workflow is purely script-based — you generate SQL and run it through your own deployment process
- You're a Redgate shop already and want a consistent toolset
- Your schema uses only the most common object types

## When PostgresCompare Is Worth It

- You deploy schema changes regularly and need safety checks built into the process
- You manage multiple databases or environments and need persistent, organised comparisons
- You want to compare against a pg_dump file rather than a live database — for offline comparison, CI pipelines, or point-in-time baselines
- You want to track schema changes over time and catch regressions
- You need to share comparison reports with the team or produce audit trails
- You want a tool that covers the full range of PostgreSQL object types

---

[Download PostgresCompare](/downloads/) to try it free for 30 days — no credit card required.
