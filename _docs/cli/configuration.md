---
title: Configuration
description: Configuring the PostgresCompare CLI with pgc.yaml
category: cli
order: 10
permalink: /docs/cli/configuration/
---

The PostgresCompare CLI can be configured using a `pgc.yaml` file to provide project-level defaults, define environments, and manage settings, reducing the need for long command-line arguments.

## Getting Started

Generate a starter configuration file with:

```bash
pgc config init
```

This creates a `pgc.yaml` file in the current directory with examples of all available settings.

## Configuration Discovery

The CLI searches for a configuration file in the following order:

1. The path specified by `-c` / `--config` on the command line.
2. The current directory and each parent directory, looking for any of these filenames:
   - `pgc.yaml`
   - `pgc.yml`
   - `.pgc.yaml`
   - `.pgc.yml`

The first file found is loaded. Use `pgc config show` to see what configuration is currently in effect.

## Configuration Structure

The configuration file uses **underscore_case** for property names. Here is a complete example:

```yaml
# pgc.yaml - PostgresCompare CLI Configuration
# Place this file in your project root.

# Default connection (used when no source/target is specified)
connection:
  default: postgres://localhost/mydb

# Named environments (referenced with @name or just name)
environments:
  prod:
    uri: postgres://${PROD_USER}:${PROD_PASS}@db.example.com/production
    readonly: true
    description: Production database
  staging:
    uri: postgres://${STAGING_USER}:${STAGING_PASS}@staging.example.com/staging
    description: Staging environment
  local:
    uri: postgres://postgres:postgres@localhost:5432/dev_db
    description: Local development

# Default comparison options (applied to diff, script, report, etc.)
comparison:
  # Properties to ignore in all comparisons
  ignore:
    - owner
    - tablespace
    - privileges
    - comments

  # Schemas to exclude from all comparisons
  exclude_schemas:
    - pg_catalog
    - information_schema

  # Object name patterns to exclude
  exclude_patterns:
    - "*_backup"
    - "tmp_*"

  # Object name patterns to include (if set, only matching objects are compared)
  # include_patterns:
  #   - "users_*"
  #   - "orders_*"

  # Restrict comparisons to specific object types
  # only:
  #   - tables
  #   - views
  #   - functions

# Output settings
output:
  format: human  # human, json, sql, markdown, github, junit, quiet
  color: auto    # auto, always, never

# Script generation settings
script:
  wrap_transaction: true
  dry_run_checks: false
  # pre_script: path/to/pre-deploy.sql
  # post_script: path/to/post-deploy.sql
```

## Merging and Precedence

Configuration settings are resolved with the following precedence, from highest to lowest:

1.  **Command-line flags** — Any option specified directly on the command line (e.g., `--no-transaction`, `--ignore statistics`) always wins.
2.  **`pgc.yaml` file** — Settings loaded from the discovered or specified configuration file.
3.  **Application defaults** — The built-in default values.

### Environment Precedence

Environments can be defined in two places:

- **User-level:** `~/.pgc/environments.json` (managed with `pgc env add/remove`)
- **Project-level:** `pgc.yaml` `environments` section

If the same environment name is defined in both, the **user-level environment takes precedence**. This allows individual developers to override project-level connection strings with their own credentials without modifying the shared `pgc.yaml`.

## `pgc config` Commands

| Command | Description |
|---|---|
| `pgc config init` | Create an example `pgc.yaml`. Use `--force` to overwrite. |
| `pgc config show` | Display the currently resolved configuration in a table. |
