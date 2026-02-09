---
title: Options
description: Configure comparison and script generation options
category: configuration
order: 13
permalink: /docs/configuration/options/
---

PostgresCompare offers many options to customize how comparisons are performed and how deployment scripts are generated.

## Comparison Options

### Schema Options

| Option | Description | Default |
|--------|-------------|---------|
| Include schemas | List of schemas to compare | All |
| Exclude schemas | List of schemas to skip | None |
| Compare system schemas | Include pg_catalog, information_schema | Off |

### Object Type Options

Toggle which object types are compared:

| Option | Default |
|--------|---------|
| Tables | On |
| Views | On |
| Materialized Views | On |
| Functions | On |
| Procedures | On |
| Triggers | On |
| Indexes | On |
| Sequences | On |
| Types | On |
| Domains | On |
| Extensions | On |
| Schemas | On |
| Comments | On |
| Privileges | Off |

### Difference Detection

| Option | Description | Default |
|--------|-------------|---------|
| Ignore case in names | Treat "MyTable" and "mytable" as equal | Off |
| Ignore whitespace in definitions | Ignore formatting differences | On |
| Ignore column order | Only compare column existence, not position | Off |
| Ignore constraint names | Compare constraint definitions, not names | Off |
| Ignore index names | Compare index definitions, not names | Off |

### Property Options

Ignore specific object properties:

| Option | Description | Default |
|--------|-------------|---------|
| Ignore owner | Don't compare object ownership | Off |
| Ignore tablespace | Don't compare tablespace assignments | Off |
| Ignore storage parameters | Don't compare FILLFACTOR, etc. | Off |
| Ignore privileges | Don't compare GRANT/REVOKE | On |
| Ignore comments | Don't compare object comments | Off |

## Script Generation Options

### Transaction Options

| Option | Description | Default |
|--------|-------------|---------|
| Wrap in transaction | BEGIN/COMMIT around script | On |
| Use savepoints | Add SAVEPOINTs for partial rollback | Off |
| Transaction isolation | Set isolation level | Default |

### Statement Options

| Option | Description | Default |
|--------|-------------|---------|
| IF EXISTS | Add IF EXISTS to DROP statements | On |
| IF NOT EXISTS | Add IF NOT EXISTS to CREATE | Off |
| Quote identifiers | Always quote names | Off |
| Lowercase keywords | Use lowercase SQL keywords | Off |
| Statement separator | Character between statements | `;` |

### Drop Options

| Option | Description | Default |
|--------|-------------|---------|
| Include drops | Generate DROP statements | On |
| Cascade drops | Add CASCADE to DROP | Off |
| Comment out drops | Wrap DROP in comments | Off |

### Ordering Options

| Option | Description | Default |
|--------|-------------|---------|
| Dependency order | Order by object dependencies | On |
| Alphabetical | Sort alphabetically within type | Off |
| Group by type | Group all tables, then views, etc. | On |

### Output Options

| Option | Description | Default |
|--------|-------------|---------|
| Include header | Add script header with metadata | On |
| Include timestamps | Add generation timestamp | On |
| Separate files | One file per object type | Off |
| Line ending | LF, CRLF, or platform default | Platform |
| Encoding | Output file encoding | UTF-8 |

## Project Settings

### Default Connections

Set default source and target connections that are used when the project opens.

### Comparison Presets

Save common option combinations as presets:

1. Configure options
2. Click **Save as Preset**
3. Name the preset
4. Apply presets from the dropdown

### Auto-Save

| Option | Description | Default |
|--------|-------------|---------|
| Auto-save project | Save project after comparison | Off |
| Auto-save interval | Minutes between auto-saves | 5 |
| Save comparison results | Persist results in project | On |

## Application Settings

### General

| Option | Description | Default |
|--------|-------------|---------|
| Theme | Light, Dark, or System | System |
| Font size | UI font size | Medium |
| Show line numbers | In SQL preview | On |
| Syntax highlighting | Color SQL syntax | On |

### Performance

| Option | Description | Default |
|--------|-------------|---------|
| Max parallel queries | Concurrent schema queries | 4 |
| Query timeout | Seconds before timeout | 60 |
| Cache snapshots | Keep snapshots in memory | On |

### Updates

| Option | Description | Default |
|--------|-------------|---------|
| Check for updates | On startup | On |
| Auto-download updates | Download when available | Off |

## CLI Configuration

Many options can be set via command line:

```bash
pgcompare compare \
  --source production \
  --target staging \
  --ignore-owner \
  --ignore-tablespace \
  --ignore-privileges \
  --include-if-exists \
  --wrap-transaction \
  --output deploy.sql
```

Or via environment variables:

```bash
export PGCOMPARE_IGNORE_OWNER=true
export PGCOMPARE_IGNORE_TABLESPACE=true
export PGCOMPARE_WRAP_TRANSACTION=true
```

## Configuration File

Create a `.pgcompare.yaml` in your project directory:

```yaml
comparison:
  ignore_owner: true
  ignore_tablespace: true
  ignore_privileges: true
  exclude_schemas:
    - pg_temp
    - staging

script:
  wrap_transaction: true
  include_if_exists: true
  quote_identifiers: false

output:
  include_header: true
  encoding: utf-8
```

## Next Steps

- [Connections](/docs/configuration/connections/) - Connection configuration
- [Ignore Rules](/docs/configuration/ignore-rules/) - Object filtering
- [CLI Commands](/docs/cli/commands/) - Command-line options
