---
title: Quick Start
description: Create your first project and run a database comparison
category: getting-started
order: 2
permalink: /docs/getting-started/quick-start/
---

This guide walks you through creating your first PostgresCompare project and running a database comparison.

## Creating a New Project

1. Launch PostgresCompare
2. Click **New Project** on the welcome screen
3. Name your project

Projects store your environment settings, comparison options, and results, making it easy to repeat comparisons.

## Connecting to Databases

PostgresCompare compares two databases referred to as the **X environment** and **Y environment**.

### Adding an Environment

1. In the project window, go to the **Environments** tab
2. Enter your connection details:
   - **Host**: Database server address (e.g., `localhost` or `db.example.com`)
   - **Port**: PostgreSQL port (default: `5432`)
   - **Database**: Name of the database
   - **Username**: Database user
   - **Password**: User password
3. Click **Test Connection** to verify
4. Click **Save**

### Connection Tips

- Use a database user with read permissions on all schemas you want to compare
- For remote databases, ensure your firewall allows connections on the PostgreSQL port
- Consider using SSH tunneling for secure remote access

## Running Your First Comparison

With both X and Y environments configured:

1. Click **Compare** in the toolbar
2. Wait for the comparison to complete

The comparison reads the schema from both databases and identifies:
- Objects that exist only in the X environment (new)
- Objects that exist only in the Y environment (dropped)
- Objects that differ between X and Y environments (different)

## Understanding the Results

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
- **New** - Exists only in the X environment
- **Dropped** - Exists only in the Y environment
- **Different** - Different between X and Y environments
- **Identical** - Same in both databases (no action needed)

## Generating a Deployment Script

To create a SQL script that synchronizes the differences:

1. Review the differences and select which changes to include using the checkboxes
2. Click **Generate Script** in the toolbar
4. Review the generated SQL before executing

<div class="warning">
<strong>Important:</strong> Always review the generated script before running it against your database. Consider testing on a non-production database first.
</div>

## Next Steps

- [Understanding comparison results](/docs/getting-started/first-comparison/) - Detailed guide to interpreting results
- [Comparing databases](/docs/guides/comparing-databases/) - Advanced comparison techniques
- [Safe deployments](/docs/guides/safe-deployments/) - Best practices for deploying changes
