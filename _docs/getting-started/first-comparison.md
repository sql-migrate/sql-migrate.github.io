---
title: First Comparison
description: Understanding comparison results and reviewing differences
category: getting-started
order: 3
permalink: /docs/getting-started/first-comparison/
---

After running a comparison, PostgresCompare presents a detailed view of all differences between your X and Y databases. This guide explains how to interpret and work with these results.

## The Results View

The comparison results are displayed in a tree structure, organized by object type. Each node shows:

- **Object name** - The database object's name
- **Status** - Visual indicator of the difference type
- **Object type** - Table, view, function, etc.

### Status Indicators

| Status | Meaning | Action |
|--------|---------|--------|
| New | Exists only in X environment | Will be created in Y |
| Dropped | Exists only in Y environment | Will be dropped from Y |
| Different | Different between X and Y | Will be altered to match X |
| Identical | Same in both databases | No action needed |

## The Overview Tab

The **Overview** tab gives you a high-level summary of a comparison at a glance. It shows:

- **Stat cards** — Five clickable cards (Total, Identical, Different, New, Dropped) each showing a count. Clicking a card switches to the Objects tab with that status filter applied.
- **By Status chart** — A doughnut chart breaking down objects by their comparison result. Click a segment to jump to the Objects tab filtered by that status.
- **By Object Type chart** — A stacked bar chart showing the breakdown of change types per object type, sorted by total count. Click a bar segment to filter by both status and object type simultaneously.

Use the Overview tab to quickly understand the shape of a comparison before diving into individual differences.

## Viewing Differences

Click on any object to see its details in the right panel. PostgresCompare uses a Monaco SQL editor to display DDL, providing syntax highlighting and a familiar editing experience.

### For new objects
- Shows the complete DDL that will create the object
- Includes all columns, constraints, indexes, etc.

### For dropped objects
- Shows the DROP statement that will remove the object
- Lists dependent objects that may be affected

### For different objects
- Shows a side-by-side comparison of X vs. Y definitions
- Highlights specific differences (columns added/removed, type changes, etc.)
- Shows the ALTER statements needed to make the changes

### SQL line highlighting

When you click a child row in the differences list — such as a column, constraint, or property — the diff editor automatically scrolls to and highlights the exact line of SQL for that sub-object. This makes it easy to pinpoint a specific change without manually searching through the SQL.

### Related object navigation

When a difference is selected, clickable chips appear in the diff viewer header showing the selected object's dependencies and reverse references:

- **Depends on** — objects this one references (e.g. a trigger's parent table and function, a view's referenced tables)
- **Referenced by** — objects that reference this one (e.g. indexes and foreign keys on a table)

Each chip is colour-coded to match the object's comparison status:

| Colour | Status |
|--------|--------|
| Amber | Different |
| Blue | New |
| Red | Dropped |
| Green | Identical |

Click a chip to select that object in the differences list and scroll to it. Objects that don't appear in the comparison are shown as dimmed, non-clickable chips.

This makes it straightforward to trace the impact of a change — for example, clicking a table chip from a trigger to review what other objects depend on it.

## Object History

PostgresCompare tracks the history of each object across comparisons. Use the object history viewer to see how an object has changed over time, providing a development history for your database schema.

## Filtering Results

Use the filter toolbar to focus on specific changes:

- **Show only differences** - Hide identical objects
- **Filter by type** - Show only tables, views, functions, etc.
- **Search** - Find objects by name

## Selecting Changes to Deploy

Not every difference needs to be deployed. You can selectively include or exclude changes:

1. **Checkbox** each object to include or exclude it
2. **Select All/None** buttons for bulk selection

### Common Scenarios

**Deploying only new features:**
- Include objects that exist only in X
- Exclude objects that would be dropped

**Syncing a development database:**
- Include all differences
- Review carefully for data loss

**Targeted fix:**
- Include only the specific objects that need updating

## Understanding Dependencies

PostgresCompare automatically orders changes to respect dependencies:

- Tables are created before views that reference them
- Functions are created before triggers that call them
- Indexes are created after their tables

The generated script handles this ordering automatically.

## Reviewing Generated SQL

Before generating a deployment script, you can preview the SQL for any object:

1. Select the object in the tree
2. View the SQL in the detail panel (displayed in the Monaco editor)
3. Click **Copy SQL** to copy individual statements

<div class="tip">
<strong>Tip:</strong> Use the SQL preview to understand exactly what changes will be made before generating the full deployment script.
</div>

## Handling Conflicts

Sometimes changes may conflict or require manual intervention:

### Column type changes
- Changing column types may require data migration
- PostgresCompare shows warnings for potentially lossy conversions

### Dropping objects with dependencies
- Objects with dependents show a warning
- Consider the impact on your application

### Rename detection
- Renamed objects may appear as drop + create
- Review these carefully and adjust comparison options if needed

## Next Steps

- [Generating deployment scripts](/docs/guides/deployment-scripts/) - Create and customize deployment SQL
- [Safe deployments](/docs/guides/safe-deployments/) - Best practices for deploying changes
- [Comparison options](/docs/configuration/options/) - Customize what gets compared
