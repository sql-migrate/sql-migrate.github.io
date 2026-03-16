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
- Use the **name filter** on any object type column to search within that type — for example, showing only tables whose names contain a specific term, without affecting other object types in the list
- The **select all** checkbox in the header toggles only the rows currently visible after filtering — rows hidden by a filter are not affected
- The header checkbox reflects the actual selection state of visible rows and updates as filters change

This makes it straightforward to select a specific subset of changes — for example, selecting only new tables while leaving function changes unselected.

## Starring Comparisons

Mark comparisons as favourites by clicking the **star icon** on a comparison tile. The star appears on hover and turns amber when active. To show only starred comparisons, click the **Starred** filter toggle in the comparisons toolbar.

This is useful for pinning the comparisons you return to regularly — for example, your main dev-to-production check — so they are easy to find in a busy project.

## Global Search

Use the global search bar to find projects, environments, and comparison objects from anywhere in the app. Results are grouped by type and update as you type.

## Keyboard Navigation

In the difference list, use the **up/down arrow keys** to move between rows without the mouse. This makes it faster to review a long list of differences when scripting or deploying changes.

## Exporting Comparison Results

Export comparison results from the comparison details panel. Click the **Export** button and choose a format:

| Format | Description |
|--------|-------------|
| **Excel** | Spreadsheet with a Summary sheet (including a doughnut chart) and a per-object data sheet |
| **HTML** | Self-contained file with clickable status filter pills and sortable column headers |
| **PDF** | Print-ready document generated from the HTML report |
| **JSON** | Machine-readable export; a companion `.schema.json` file is written alongside for CI/CD validation |
| **Markdown** | Plain-text table format for pasting into wikis or pull requests |
| **CSV** | Simple comma-separated values for spreadsheet import |

The export options modal lets you choose which status categories to include. **Identical** objects are unchecked by default to keep exports focused on differences. Filenames default to the source and target database names plus the date (e.g. `devDb_vs_prodDb_2026-03-08.xlsx`).

## Data Comparison

In addition to schema comparison, PostgresCompare supports data comparison between databases. This allows you to identify row-level differences in table data between your X and Y environments.

## Comparing Large Databases

For databases with thousands of objects:

### Performance Tips

1. **Use schema filters** - Compare only relevant schemas
2. **Limit object types** - Disable types you don't need

## Re-running a Comparison

The **Re-run comparison** button in the comparison toolbar starts a fresh comparison without leaving the current view. This is useful when you've made changes to a database and want to see whether a difference has been resolved.

- Results stream in live as the new comparison runs
- The toolbar button is disabled while a comparison is in progress
- The previous comparison is preserved in the history list — navigating away and back will not lose it
- If you navigate to a different comparison while a re-run is in progress, polling is cancelled automatically

## Tracking Schema Changes Over Time

PostgresCompare records every comparison, letting you see how your schema has evolved across runs. To view schema changes between two snapshots:

1. From the comparisons list, select two comparisons using the checkboxes
2. Click the **Schema Changes** button that appears

### The schema changes swimlane view

The schema changes view groups database objects by how they changed between the two selected comparisons:

| Category | Meaning |
|----------|---------|
| **Fixed** | Was different or missing; now identical |
| **Regressed** | Was identical; now different or missing |
| **New** | Appeared for the first time |
| **Removed** | No longer present |
| **Changed** | Different in both snapshots, but the difference changed |
| **Unchanged** | No change between the two snapshots |

Each category is a collapsible section with a count badge and a colour-coded left border. A date range header shows the time span between the two comparisons. Use the name filter to search within the view.

This makes it easy to spot regressions — for example, catching a table that was identical in an earlier comparison but has drifted since a recent deployment.

### Object history viewer

Click any object in the differences list and open the history tab to see that specific object's changes across all comparisons. This gives you a per-object development timeline independent of version control.

## Next Steps

- [Generating deployment scripts](/docs/guides/deployment-scripts/) - Create SQL scripts from comparisons
- [Safe deployments](/docs/guides/safe-deployments/) - Best practices for deploying changes
