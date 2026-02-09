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
3. Choose a location and name for your project file (`.pgc`)
4. Click **Save**

Projects store your connection settings, comparison options, and ignore rules, making it easy to repeat comparisons.

## Connecting to Databases

PostgresCompare compares two databases: a **source** (what you have) and a **target** (what you want to update).

### Adding a Connection

1. In the project window, click **Add Connection** for either source or target
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

With both source and target connections configured:

1. Click **Compare** in the toolbar
2. Wait for the comparison to complete

The comparison reads the schema from both databases and identifies:
- Objects that exist only in the source (will be created)
- Objects that exist only in the target (will be dropped)
- Objects that differ between source and target (will be altered)

## Understanding the Results

The comparison results show a tree view of all database objects organized by type:

- **Tables** - Structure, columns, constraints
- **Views** - View definitions
- **Functions** - Stored procedures and functions
- **Indexes** - Table indexes
- **Sequences** - Auto-increment sequences
- **Types** - Custom data types
- **Extensions** - PostgreSQL extensions

Each object shows an icon indicating its status:
- **Green plus** - Exists only in source (will be created)
- **Red minus** - Exists only in target (will be dropped)
- **Yellow delta** - Different between source and target (will be altered)
- **Gray check** - Identical in both databases (no action)

## Generating a Deployment Script

To create a SQL script that updates the target to match the source:

1. Review the differences and select which changes to include
2. Click **Generate Script** in the toolbar
3. Choose a save location for the `.sql` file
4. Review the generated SQL before executing

<div class="warning">
<strong>Important:</strong> Always review the generated script before running it against your database. Consider testing on a non-production database first.
</div>

## Next Steps

- [Understanding comparison results](/docs/getting-started/first-comparison/) - Detailed guide to interpreting results
- [Comparing databases](/docs/guides/comparing-databases/) - Advanced comparison techniques
- [Safe deployments](/docs/guides/safe-deployments/) - Best practices for deploying changes
