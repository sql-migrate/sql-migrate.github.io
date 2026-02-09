---
title: Ignore Rules Configuration
description: Configure ignore rules to exclude objects from comparison
category: configuration
order: 12
permalink: /docs/configuration/ignore-rules/
---

Ignore rules allow you to exclude specific database objects or types of differences from comparison results. This reference covers all available ignore rule options.

## Rule Structure

Ignore rules are defined in your project settings or exported as YAML:

```yaml
ignore_rules:
  - type: table
    name: temp_data
    comment: Temporary import table

  - type: function
    pattern: "debug_*"
    comment: Debug functions only in dev
```

## Rule Properties

### type

The type of database object to match:

| Value | Description |
|-------|-------------|
| `table` | Tables |
| `view` | Views |
| `materialized_view` | Materialized views |
| `function` | Functions and procedures |
| `trigger` | Triggers |
| `index` | Indexes |
| `sequence` | Sequences |
| `type` | Custom types |
| `domain` | Domains |
| `extension` | Extensions |
| `schema` | Schemas |
| `constraint` | Constraints (all types) |
| `foreign_key` | Foreign key constraints |
| `check` | Check constraints |
| `unique` | Unique constraints |
| `primary_key` | Primary key constraints |

### name

Exact object name to match (case-sensitive):

```yaml
- type: table
  name: schema_migrations
```

### pattern

Wildcard pattern to match object names:

| Pattern | Matches |
|---------|---------|
| `*` | Any characters |
| `?` | Single character |
| `[abc]` | Character set |
| `[a-z]` | Character range |

```yaml
- type: table
  pattern: "temp_*"

- type: table
  pattern: "*_backup"

- type: table
  pattern: "log_202?"
```

### schema

Limit rule to a specific schema:

```yaml
- type: table
  schema: staging
  pattern: "*"
  comment: Ignore all staging tables

- type: function
  schema: internal
  name: debug_log
```

### all

Match all objects of the specified type:

```yaml
- type: sequence
  all: true
  comment: Ignore all sequences
```

### property

Ignore specific object properties instead of entire objects:

```yaml
- property: owner
  all: true
  comment: Ignore ownership differences

- property: tablespace
  all: true

- property: storage_parameters
  all: true

- property: privileges
  all: true

- property: comments
  all: true
```

### enabled

Toggle rule on/off without deleting:

```yaml
- type: table
  name: temp_data
  enabled: false
  comment: Temporarily disabled
```

### comment

Document the purpose of the rule:

```yaml
- type: table
  pattern: "django_*"
  comment: Django framework tables, managed by migrations
```

## Complete Example

```yaml
ignore_rules:
  # Framework tables
  - type: table
    name: django_migrations
    comment: Django migration tracking

  - type: table
    name: django_content_type
    comment: Django content types

  - type: table
    name: django_session
    comment: Django sessions

  # Temporary and staging
  - type: table
    pattern: "temp_*"
    comment: Temporary import tables

  - type: table
    schema: staging
    all: true
    comment: All staging schema objects

  # Development only
  - type: function
    pattern: "debug_*"
    comment: Debug functions

  - type: table
    pattern: "test_*"
    comment: Test data tables

  # Environment differences
  - property: owner
    all: true
    comment: Different owners per environment

  - property: tablespace
    all: true
    comment: Tablespace varies by environment

  # Extensions managed separately
  - type: extension
    name: pg_stat_statements
    comment: Monitoring extension

  # Replication objects
  - type: table
    pattern: "*_repl"
    comment: Replication shadow tables
```

## Rule Files

### Exporting Rules

Export rules to a file:

```bash
pgcompare project export-rules myproject.pgc > rules.yaml
```

### Importing Rules

Import rules from a file:

```bash
pgcompare project import-rules myproject.pgc < rules.yaml
```

### Sharing Rules

Create a team-wide rules file:

```yaml
# team-ignore-rules.yaml
# Standard ignore rules for all projects

ignore_rules:
  # Framework tables
  - type: table
    name: schema_migrations

  # Environment-specific
  - property: owner
    all: true

  - property: tablespace
    all: true
```

## Next Steps

- [Using Ignore Rules](/docs/guides/ignore-rules/) - Practical usage guide
- [Options](/docs/configuration/options/) - Other configuration options
