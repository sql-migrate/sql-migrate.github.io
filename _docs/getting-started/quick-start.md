---
title: Quick Start
description: Connect to your databases and run your first schema comparison
category: getting-started
order: 2
permalink: /docs/getting-started/quick-start/
---

This guide walks you through connecting to your databases, creating a project and running your first schema comparison.

Want to look around first? Click **Try the demo** on the welcome screen to open a sample comparison — no database needed.

## Connecting to Databases

PostgresCompare compares two databases: the **Source** and the **Target**. Each side uses a saved connection.

### Adding a Connection

1. Click **Connections** in the sidebar
2. Click **New connection**
3. Enter your connection details:
   - **Name**: A name to recognise this connection by
   - **Host**: Database server address (e.g., `localhost` or `db.example.com`)
   - **Port**: PostgreSQL port (default: `5432`)
   - **Username**: Database user
   - **Password**: User password
   - **Default database**: The database to connect to
4. Click **Test** to verify the connection
5. Click **Save**

If PostgresCompare finds PostgreSQL connections already set up on your machine, click **Import from environment** to add them in one step.

### Connection Tips

- Use a database user with read permissions on all schemas you want to compare
- For remote databases, ensure your firewall allows connections on the PostgreSQL port
- Consider using SSH tunneling for secure remote access

## Creating a New Project

<div class="note">
<strong>Pro feature.</strong> Projects require a Pro subscription and are included in your 14-day trial. On the free tier, use <strong>Quick compare</strong> in the sidebar to compare two databases without a project. <a href="/purchase">See pricing</a>.
</div>

1. Click **Schema projects** in the sidebar
2. Click **New project**
3. Name your project
4. Under **Source** and **Target**, choose the connection, database and schema for each side
5. Save the project

Projects store your connections, comparison options, and results, making it easy to repeat comparisons.

## Running Your First Comparison

With the project's source and target set:

1. Open the project and click **Compare**
2. Wait for the comparison to complete

<figure class="doc-shot">
  <img src="/images/screenshot2.png" alt="A PostgresCompare project showing the latest comparison as a donut chart, with a status of Different (3) and a Compare again button." loading="lazy" width="1920" height="1086">
  <figcaption>The project view after a comparison, with the result summarised and a button to run it again.</figcaption>
</figure>

The comparison reads the schema from both databases and identifies:
- Objects that exist only in the source (new)
- Objects that exist only in the target (dropped)
- Objects that differ between source and target (different)

<div class="note">
<strong>On the free tier</strong>, use <strong>Quick compare</strong> in the sidebar instead of a project. It runs the same comparison and lets you generate and deploy the script, but the result isn't saved. Saved projects and their history are part of Pro. <a href="/faq">Read the FAQ</a>.
</div>

## Understanding the Results

<figure class="doc-shot">
  <img src="/images/postgrescomparescreenshot.png" alt="Comparison results in PostgresCompare: a filterable list of database objects with New, Different and Identical statuses, above a side-by-side Difference SQL view with changed lines highlighted." loading="lazy" width="1920" height="1084">
  <figcaption>Objects on top, filterable by name, type and status; the difference SQL for the selected object below.</figcaption>
</figure>

The comparison results show a tree view of all database objects organized by type:

- **Tables** - Structure, columns, constraints
- **Views** - View definitions
- **Materialized Views** - Materialized view definitions
- **Functions** - Functions
- **Procedures** - Stored procedures
- **Triggers** - Table triggers
- **Indexes** - Table indexes
- **Sequences** - Auto-increment sequences
- **Schemas** - Database schemas
- **Composite Types** - User-defined composite types
- **Enums** - Enumeration types
- **Domains** - Domain types
- **Extensions** - PostgreSQL extensions
- **Roles** - Database roles
- **Databases** - Database settings
- **Tablespaces** - Tablespace definitions
- **Casts** - Type casts
- **Conversions** - Encoding conversions
- **Event Triggers** - Event triggers
- **Foreign Data Wrappers** - FDW definitions
- **Foreign Servers** - Foreign server definitions
- **Foreign Tables** - Foreign tables
- **Operator Families** - Operator families
- **Operators** - User-defined operators
- **Access Methods** - Access methods
- **Text Search Parsers** - Full-text search parsers
- **Text Search Templates** - Full-text search templates
- **Text Search Dictionaries** - Full-text search dictionaries
- **Text Search Configurations** - Full-text search configurations
- **User Mappings** - Foreign server user mappings
- **Publications** - Logical replication publications
- **Subscriptions** - Logical replication subscriptions
- **Statistics** - Extended statistics
- **Policies** - Row-level security policies
- **Collations** - Collation definitions
- **Aggregates** - Aggregate functions
- **Ranges** - Range types
- **Settings** - Configuration settings

Each object shows a status indicating its comparison result:
- **New** - Exists only in the source
- **Dropped** - Exists only in the target
- **Different** - Different between source and target
- **Identical** - Same in both databases (no action needed)

## Generating a Deployment Script

To create a SQL script that synchronizes the differences:

1. Review the differences and select which changes to include using the checkboxes
2. Click **Generate deployment script…**
3. Review the generated SQL before running it

<div class="warning">
<strong>Important:</strong> Always review the generated script before running it against your database. Consider testing on a non-production database first.
</div>

## Next Steps

- [Understanding comparison results](/docs/getting-started/first-comparison/) - Detailed guide to interpreting results
- [Comparing databases](/docs/guides/comparing-databases/) - Advanced comparison techniques
- [Safe deployments](/docs/guides/safe-deployments/) - Best practices for deploying changes
