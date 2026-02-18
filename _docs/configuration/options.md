---
title: Options
description: Configure comparison and deployment options
category: configuration
order: 13
permalink: /docs/configuration/options/
---

PostgresCompare offers options to customize how comparisons are performed and how deployment scripts are generated. All options are configured per project in the project settings.

## Comparison Options

### Ignore Toggles

Control which types of differences are detected:

| Option | Description | Default |
|--------|-------------|---------|
| Ignore Whitespace | Ignore whitespace differences when comparing functions | Off |
| Ignore Table Partitions | Ignore partition definitions on tables | Off |
| Ignore Column Order | Only compare column existence, not position | Off |
| Ignore Code Comments | Ignore comments within code definitions | On |
| Ignore Case | Treat identifiers as case-insensitive | Off |
| Ignore Owner | Don't compare object ownership | Off |
| Ignore Tablespace | Don't compare tablespace assignments | On |
| Ignore Privileges | Don't compare GRANT/REVOKE statements | On |
| Ignore Defaults | Ignore column default value differences | Off |
| Ignore Statistics | Ignore statistics target differences | Off |

### Object Type Toggles

Toggle which object types are included in the comparison. Each type can be independently enabled or disabled.

**Default ON:**

| Object Type | Description |
|-------------|-------------|
| Tables | Table structure, columns, constraints |
| Views | View definitions |
| Materialized Views | Materialized view definitions |
| Triggers | Table and event triggers |
| Functions | User-defined functions |
| Procedures | Stored procedures |
| Indexes | Table indexes |
| Schemas | Schema definitions |
| Composite Types | User-defined composite types |
| Enums | Enumeration types |
| Domains | Domain types |
| Extensions | PostgreSQL extensions |
| Access Methods | Custom access methods |
| Casts | Type casts |
| Conversions | Encoding conversions |
| Event Triggers | Event-level triggers |
| Foreign Data Wrappers | FDW definitions |
| Operator Families | Operator family definitions |
| Foreign Servers | Foreign server definitions |
| Text Search Parsers | Full-text search parsers |
| Text Search Templates | Full-text search templates |
| Text Search Dictionaries | Full-text search dictionaries |
| Text Search Configurations | Full-text search configurations |
| User Mappings | Foreign server user mappings |
| Publications | Logical replication publications |
| Subscriptions | Logical replication subscriptions |
| Statistics | Extended statistics objects |
| Policies | Row-level security policies |
| Collations | Collation definitions |
| Sequences | Sequence definitions |
| Operators | User-defined operators |
| Foreign Tables | Foreign table definitions |
| Aggregates | Aggregate functions |
| Ranges | Range type definitions |
| Settings | Configuration parameter settings |

**Default OFF:**

| Object Type | Description |
|-------------|-------------|
| Roles | Database roles and permissions |
| Databases | Database-level settings |
| Tablespaces | Tablespace definitions |

### Schema Filtering

| Option | Description |
|--------|-------------|
| X Schema to Compare | Schema to compare from the X environment |
| Y Schema to Compare | Schema to compare from the Y environment |

## Script Options

### Statement Selection

After running a comparison, use the checkboxes next to each difference to select which changes to include in the generated script. This allows fine-grained control over what gets deployed.

### Transaction Wrapping

Deployment scripts can be wrapped in a transaction block (BEGIN/COMMIT) so that all changes are applied atomically — either all succeed or all are rolled back.

## Next Steps

- [Environments](/docs/configuration/connections/) - Environment configuration
- [Comparing Databases](/docs/guides/comparing-databases/) - Comparison techniques
