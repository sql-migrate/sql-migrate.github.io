---
title: Comparing Databases
description: Advanced techniques for comparing PostgreSQL databases
category: guides
order: 4
permalink: /docs/guides/comparing-databases/
---

This guide covers advanced techniques for comparing PostgreSQL databases with PostgresCompare.

## Database to Database Comparison

PostgresCompare compares two live PostgreSQL databases. Common scenarios include:
- Comparing development to production
- Comparing feature branches
- Auditing database drift

## Schema Filtering

By default, PostgresCompare compares all schemas. To focus on specific schemas, use the schema pairing options:

| Option | Description |
|--------|-------------|
| X Schema to Compare | The schema to compare from the X environment |
| Y Schema to Compare | The schema to compare from the Y environment |

This also allows cross-schema comparison — comparing a schema named `dev` in X against `prod` in Y.

## Object Type Filtering

Filter which object types are compared in the project settings. PostgresCompare supports 38 object types:

**Default ON:** Tables, Views, Materialized Views, Triggers, Functions, Procedures, Indexes, Schemas, Composite Types, Enums, Domains, Extensions, Access Methods, Casts, Conversions, Event Triggers, Foreign Data Wrappers, Operator Families, Foreign Servers, Text Search Parsers, Text Search Templates, Text Search Dictionaries, Text Search Configurations, User Mappings, Publications, Subscriptions, Statistics, Policies, Collations, Sequences, Operators, Foreign Tables, Aggregates, Ranges, Settings

**Default OFF:** Roles, Databases, Tablespaces

## Comparison Options

Fine-tune how objects are compared using the 10 ignore toggles:

| Option | Default | Description |
|--------|---------|-------------|
| Ignore Whitespace | Off | Ignore whitespace in function definitions |
| Ignore Table Partitions | Off | Ignore partition definitions |
| Ignore Column Order | Off | Only compare column existence, not position |
| Ignore Code Comments | On | Ignore comments in code |
| Ignore Case | Off | Case-insensitive comparison |
| Ignore Owner | Off | Don't compare ownership |
| Ignore Tablespace | On | Don't compare tablespace assignments |
| Ignore Privileges | On | Don't compare GRANT/REVOKE |
| Ignore Defaults | Off | Ignore column default differences |
| Ignore Statistics | Off | Ignore statistics targets |

## Filtering and Selecting Changes

The results view lets you filter differences before generating a script:

- Use the filter dropdowns to narrow results by object type, change type, or schema
- The **select all** checkbox in the header toggles only the rows currently visible after filtering — rows hidden by a filter are not affected
- The header checkbox reflects the actual selection state of visible rows and updates as filters change

This makes it straightforward to select a specific subset of changes — for example, selecting only new tables while leaving function changes unselected.

## Data Comparison

In addition to schema comparison, PostgresCompare supports data comparison between databases. This allows you to identify row-level differences in table data between your X and Y environments.

## Comparing Large Databases

For databases with thousands of objects:

### Performance Tips

1. **Use schema filters** - Compare only relevant schemas
2. **Limit object types** - Disable types you don't need

## Tracking Changes Over Time

PostgresCompare provides an object history viewer that tracks how database objects change across comparisons. This gives you a development history for your schema, independent of version control.

## Next Steps

- [Generating deployment scripts](/docs/guides/deployment-scripts/) - Create SQL scripts from comparisons
- [Safe deployments](/docs/guides/safe-deployments/) - Best practices for deploying changes
