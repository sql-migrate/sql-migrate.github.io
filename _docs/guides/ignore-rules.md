---
title: Using Ignore Rules
description: Exclude specific objects and differences from comparison
category: guides
order: 7
permalink: /docs/guides/ignore-rules/
---

Ignore rules let you exclude specific database objects or types of differences from PostgresCompare results. This is useful for handling environment-specific configurations, intentional differences, and noise reduction.

## Why Use Ignore Rules?

Common scenarios for ignore rules:

- **Environment differences** - Different connection pooler settings in dev vs. prod
- **Generated objects** - Tables created by frameworks or ORMs
- **Permissions** - Different role grants per environment
- **Temporary objects** - Staging tables, import tables
- **Third-party schemas** - Extension-created objects

## Creating Ignore Rules

### From the UI

1. Right-click an object in the comparison results
2. Select **Ignore** from the context menu
3. Choose the ignore scope:
   - **This object** - Ignores only this specific object
   - **Objects matching pattern** - Uses wildcards
   - **All objects of this type** - Ignores all tables, all functions, etc.

### From Project Settings

1. Open **Project Settings**
2. Navigate to **Ignore Rules**
3. Click **Add Rule**
4. Configure the rule (see options below)

## Rule Types

### Object Name Rules

Match objects by name using exact match or wildcards:

```yaml
# Exact match
- type: table
  name: temp_import_data

# Wildcard match
- type: table
  pattern: "temp_*"

# Schema-qualified
- type: table
  schema: staging
  pattern: "*"
```

### Object Type Rules

Ignore entire categories of objects:

```yaml
# Ignore all sequences
- type: sequence
  all: true

# Ignore all objects in a schema
- schema: pg_temp
  all: true
```

### Property Rules

Ignore specific properties rather than entire objects:

```yaml
# Ignore tablespace differences
- property: tablespace
  all: true

# Ignore storage parameters
- property: storage_parameters
  all: true

# Ignore owner differences
- property: owner
  all: true

# Ignore all permissions
- property: privileges
  all: true
```

## Pattern Matching

PostgresCompare supports these wildcards:

| Pattern | Matches |
|---------|---------|
| `*` | Any characters (zero or more) |
| `?` | Any single character |
| `[abc]` | Any character in the set |
| `[a-z]` | Any character in the range |

### Examples

```yaml
# Tables starting with "tmp_"
pattern: "tmp_*"

# Tables ending with "_backup"
pattern: "*_backup"

# Tables with date suffix
pattern: "orders_202?"

# Audit tables in any schema
pattern: "*audit*"
```

## Common Ignore Rules

### Development vs. Production

```yaml
ignore_rules:
  # Ignore test data tables
  - type: table
    pattern: "test_*"

  # Ignore development-only functions
  - type: function
    pattern: "debug_*"

  # Ignore different connection pool settings
  - property: storage_parameters
    all: true
```

### Multi-Tenant Databases

```yaml
ignore_rules:
  # Ignore tenant-specific schemas
  - schema: tenant_*
    all: true

  # But keep shared schema
  # (no rule needed, it's compared by default)
```

### Framework-Generated Objects

```yaml
ignore_rules:
  # Django migrations table
  - type: table
    name: django_migrations

  # Rails schema info
  - type: table
    name: schema_migrations
  - type: table
    name: ar_internal_metadata

  # Prisma
  - type: table
    name: _prisma_migrations
```

### Replication and Extensions

```yaml
ignore_rules:
  # Logical replication slots
  - schema: pg_catalog
    type: replication_slot

  # PostGIS system tables
  - type: table
    name: spatial_ref_sys

  # pg_stat_statements
  - schema: public
    type: view
    name: pg_stat_statements
```

## Managing Ignore Rules

### Viewing Active Rules

1. Open **Project Settings > Ignore Rules**
2. See list of all configured rules
3. Toggle rules on/off without deleting

### Editing Rules

1. Select a rule in the list
2. Click **Edit**
3. Modify the rule parameters
4. Click **Save**

### Importing/Exporting Rules

Share rules between projects:

```bash
# Export rules
pgcompare export-rules project.pgc > rules.yaml

# Import rules
pgcompare import-rules project.pgc < rules.yaml
```

## Rule Priority

When multiple rules might apply:

1. More specific rules take precedence
2. Object-level rules override property-level rules
3. Later rules in the list override earlier ones

### Example

```yaml
ignore_rules:
  # Ignore all temp tables
  - type: table
    pattern: "temp_*"
    all: true

  # But DON'T ignore temp_important
  - type: table
    name: temp_important
    ignore: false  # This overrides the pattern rule
```

## Temporary Ignores

For one-time comparisons, use quick ignores:

1. Run comparison as normal
2. Right-click unwanted differences
3. Select **Ignore for this comparison**

These ignores don't persist to project settings.

## Troubleshooting Rules

### Rule Not Working?

Check:
1. **Exact spelling** - Names are case-sensitive
2. **Schema prefix** - May need `schema.object` format
3. **Rule order** - Later rules override earlier ones
4. **Rule enabled** - Toggle may be off

### Too Much Ignored?

If rules are hiding things you want to see:
1. Review all rules in Project Settings
2. Disable rules temporarily to test
3. Make patterns more specific

## Best Practices

1. **Document why** - Add comments explaining each rule
2. **Be specific** - Prefer exact matches over broad patterns
3. **Review regularly** - Remove obsolete rules
4. **Share with team** - Export rules for consistency
5. **Test carefully** - Verify rules work as expected

## Next Steps

- [Comparing databases](/docs/guides/comparing-databases/) - Advanced comparison techniques
- [Configuration options](/docs/configuration/options/) - All comparison settings
- [CLI usage](/docs/cli/commands/) - Apply rules via command line
