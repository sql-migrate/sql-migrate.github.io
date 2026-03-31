---
title: CLI Commands
description: Command-line interface reference for PostgresCompare CLI v1.2.1
category: cli
order: 8
permalink: /docs/cli/commands/
---

The PostgresCompare command-line interface (CLI), `pgc`, provides a powerful set of tools for schema comparison, script generation, and database management from your terminal. It's designed for both interactive use and integration into CI/CD pipelines.

## Global Options

These options can be used with any command:

- `--help`, `-h`: Show help information for any command.
- `--version`: Show the application version.
- `--no-color`: Disable colorized output. Also respects the `NO_COLOR` environment variable.

## Data Sources

Most commands operate on one or two data sources. These can be specified as:

| Format | Example |
|---|---|
| Live Database | `postgres://user:pass@host:5432/dbname` |
| Environment | `@production` or `production` |
| JSON Snapshot | `path/to/snapshot.json` |
| SQL File | `path/to/schema.sql` |
| SQL Folder | `path/to/sql_folder/` |
| Git Reference | `git:main:schema.sql` |

See the [Data Sources](/docs/cli/data-sources/) page for full details.

## Exit Codes

All comparison commands use the same exit codes:

| Code | Meaning |
|---|---|
| `0` | Success — schemas are identical (or command completed successfully) |
| `1` | Differences found |
| `2` | Error (connection failure, invalid arguments, etc.) |

## Default Ignore Rules

By default, `pgc diff` and `pgc interactive` ignore the following properties: **owner**, **tablespace**, **privileges**, **column-order**, **whitespace**, and **comments**. This avoids noisy diffs in most environments. Use `--no-ignore-defaults` to compare everything.

---

## `pgc diff`

Compares two data sources and shows the differences. This is the core command for understanding schema drift.

### Syntax

```
pgc diff <source> <target> [options]
```

### Options

| Option | Description |
|---|---|
| `-f, --format <FORMAT>` | Output format: `human` (default), `json`, `sql`, `markdown`, `github`, `junit`, `quiet` |
| `-o, --output <FILE>` | Write output to a file instead of stdout |
| `-s, --schema <SCHEMA>` | Compare only specific schema(s), comma-separated |
| `--exclude-schema <PATTERN>` | Exclude schemas matching pattern (supports wildcards) |
| `--only <TYPES>` | Compare only specific object types (e.g. `tables,views,functions`) |
| `--exclude <TYPES>` | Exclude specific object types from comparison |
| `--include <PATTERN>` | Include only objects matching name pattern (e.g. `'users_*,orders_*'`) |
| `--exclude-pattern <PATTERN>` | Exclude objects matching name pattern |
| `--ignore <OPTIONS>` | Comma-separated list of properties to ignore (see below) |
| `--no-ignore-defaults` | Don't ignore owner, tablespace, privileges, column-order, whitespace, or comments by default |
| `--sql` | Shorthand for `--format sql` |
| `-q, --quiet` | Minimal output, rely on exit code |
| `--no-progress` | Disable progress indicators |
| `--wrap-transaction` | Wrap SQL output in `BEGIN`/`COMMIT` |
| `--pre-script <SCRIPT_OR_FILE>` | SQL to run before migration (inline string or path to `.sql` file) |
| `--post-script <SCRIPT_OR_FILE>` | SQL to run after migration (inline string or path to `.sql` file) |
| `-c, --config <FILE>` | Path to config file (default: `pgc.yaml`) |

#### `--ignore` values

`--ignore owner,tablespace,privileges,comments,whitespace,column-order,case,defaults,statistics,partitions`

Multiple values can be combined with commas.

#### `--only` / `--exclude` object types

Supports all PostgreSQL object types, including: `tables`, `views`, `functions`, `indexes`, `sequences`, `triggers`, `enums`, `domains`, `schemas`, `extensions`, `policies`, `procedures`, `roles`, `aggregates`, `collations`, `conversions`, `foreigndatawrappers`, `foreignservers`, `foreigntables`, `operators`, `operatorfamilies`, `publications`, `subscriptions`, `statistics`, `ranges`, `usermappings`, and all text search types. Both singular and plural forms are accepted.

### Examples

```bash
# Compare two live databases
pgc diff postgres://localhost/db1 postgres://localhost/db2

# Compare environments, output as JUnit XML for CI
pgc diff @staging @production --format junit > results.xml

# Compare a database against a schema file in git
pgc diff postgres://localhost/dev_db git:main:schema.sql

# Pipe clean SQL output to a file
pgc diff @prod @staging --sql > changes.sql

# Compare only tables and views, ignoring statistics
pgc diff @prod @staging --only tables,views --ignore statistics
```

---

## `pgc script`

Compares two data sources and generates a dependency-ordered SQL migration script. Scripts are wrapped in a transaction by default.

### Syntax

```
pgc script <source> <target> [options]
```

### Options

| Option | Description |
|---|---|
| `-o, --output <FILE>` | Write script to a file instead of stdout |
| `--direction <DIR>` | Script direction: `to-target` (default) or `to-source` / `reverse` |
| `--no-transaction` | Don't wrap script in a transaction (transactions are on by default) |
| `--progress-logging` | Include `RAISE NOTICE` statements for progress tracking |
| `--dry-run-checks` | Add pre-flight validation checks to the script |
| `--pre-script <SCRIPT_OR_FILE>` | SQL to prepend (inline string or path to `.sql` file) |
| `--post-script <SCRIPT_OR_FILE>` | SQL to append (inline string or path to `.sql` file) |
| `-s, --schema <SCHEMA>` | Compare only specific schema(s), comma-separated |
| `--only <TYPES>` | Compare only specific object types |
| `--ignore <OPTIONS>` | Comma-separated properties to ignore |
| `-q, --quiet` | Suppress progress output |
| `-c, --config <FILE>` | Path to config file |

### Script Direction

- **`to-target`** (default): Generate a script that makes the **source** match the **target**. This is the typical migration direction — your source is the database you want to update, and the target is the desired state.
- **`to-source`** or **`reverse`**: Generate a script that makes the **target** match the **source**. Useful for rollback scripts.

### Examples

```bash
# Generate a migration script to update staging to match production
pgc script @production @staging -o update_staging.sql

# Generate a rollback script
pgc script @production git:v1.1.0:schema.sql --direction to-source

# Script with progress logging for monitoring deployments
pgc script @prod @staging --progress-logging -o deploy.sql

# Script without transaction wrapping (e.g. for CREATE INDEX CONCURRENTLY)
pgc script @prod @staging --no-transaction -o deploy.sql
```

---

## `pgc apply`

Applies a migration SQL script to a target database. Includes safety features to prevent accidental destructive changes.

### Syntax

```
pgc apply <target> <script> [options]
```

The `<script>` argument is a path to a `.sql` file, or `-` to read from stdin.

### Options

| Option | Description |
|---|---|
| `--confirm` | **Required** to actually execute the script. Without this flag, the command will refuse to run. |
| `--dry-run` | Parse and validate the script without executing |
| `--max-statements <N>` | Abort if the script exceeds N statements |
| `--max-drops <N>` | Abort if the script contains more than N `DROP` statements |
| `--confirm-file <PATH>` | Require a specific file to exist before executing (CI safety gate) |
| `--single-transaction` | Run entire script in a single transaction (default: `true`) |
| `--timeout <SECONDS>` | Command timeout in seconds (default: `300`) |
| `-f, --format <FORMAT>` | Output format: `human` (default) or `json` |
| `-q, --quiet` | Suppress progress output |

### Safety Features

The `apply` command is designed to be safe by default:
- **Requires `--confirm`** — the command will not execute without this explicit flag.
- **Respects read-only environments** — refuses to apply to environments marked as `readonly`.
- **Statement and drop limits** — set guardrails with `--max-statements` and `--max-drops`.
- **Confirmation file** — for CI pipelines, require a file to exist (e.g. created by an approval step).

### Examples

```bash
# Validate a script without running it
pgc apply @staging migration.sql --dry-run

# Apply with explicit confirmation
pgc apply @staging migration.sql --confirm

# Apply with safety limits in CI
pgc apply @staging migration.sql --confirm --max-drops 5 --max-statements 100

# Pipe a script from pgc script directly
pgc script @prod @staging | pgc apply @staging - --confirm
```

---

## `pgc snapshot`

Captures the schema of a live database to a JSON file. Snapshots can be used as data sources for `diff`, `script`, and `report` commands.

### Syntax

```
pgc snapshot <source> [options]
```

**Note:** The source must be a live database connection (URI or environment). You cannot snapshot a SQL file or another snapshot.

Alias: `pgc snap`

### Options

| Option | Description |
|---|---|
| `-o, --output <FILE>` | Output file path (defaults to stdout if not specified) |
| `--pretty` | Pretty-print the JSON output |
| `-q, --quiet` | Suppress progress output |

### Examples

```bash
# Snapshot production to a dated file
pgc snapshot @production -o prod_schema_$(date +%Y-%m-%d).json

# Pretty-printed snapshot for readability
pgc snapshot postgres://localhost/mydb -o schema.json --pretty

# Pipe to stdout for processing
pgc snap @prod | jq '.Tables | length'
```

---

## `pgc watch`

Monitors a database for schema changes by comparing it against a baseline at a set interval.

### Syntax

```
pgc watch <source> [target] [options]
```

If no target is specified, the CLI captures an initial snapshot and compares against it on each interval. If a target is provided, it compares the source against the fixed target each time.

### Options

| Option | Description |
|---|---|
| `-i, --interval <SECONDS>` | Check interval in seconds (default: `30`) |
| `--on-change <COMMAND>` | Shell command to execute when changes are detected |
| `-s, --schema <SCHEMA>` | Compare only specific schema(s) |
| `--only <TYPES>` | Compare only specific object types |
| `--ignore <OPTIONS>` | Comma-separated properties to ignore |
| `-f, --format <FORMAT>` | Output format when changes detected: `human` (default), `json`, `quiet` |

Press `Ctrl+C` to stop watching. A summary of checks and changes is printed on exit.

### Examples

```bash
# Watch for any drift from the initial state
pgc watch postgres://localhost/dev_db

# Watch against a known-good snapshot, check every minute
pgc watch @production prod_baseline.json --interval 60

# Trigger a Slack notification on change
pgc watch @prod --on-change "curl -X POST https://hooks.slack.com/..."

# Quiet mode for scripting
pgc watch @prod schema.sql --format quiet --on-change "echo DRIFT >> /var/log/drift.log"
```

---

## `pgc interactive`

Launches a terminal user interface (TUI) to explore schema differences interactively.

### Syntax

```
pgc interactive <source> <target>
pgc i <source> <target>
```

### Keyboard Shortcuts

| Key | Action |
|---|---|
| `a` | Show all differences |
| `n` | Show only new (added) objects |
| `m` | Show only modified objects |
| `d` | Show only dropped objects |
| `s` | Switch to SQL output view |
| `j` | Switch to JSON output view |
| `q` / `Esc` | Quit |

### Example

```bash
pgc i @staging @production
```

---

## `pgc docs`

Generates schema documentation from any data source.

### Syntax

```
pgc docs generate <source> [options]
pgc docs gen <source> [options]
```

### Options

| Option | Description |
|---|---|
| `-o, --output <FILE>` | Output file path (defaults to stdout) |
| `-f, --format <FORMAT>` | Output format: `markdown` (default) or `html` |
| `-s, --schema <SCHEMA>` | Document only specific schema(s), comma-separated |
| `--only <TYPES>` | Document only specific object types |
| `--include-ddl` | Include full `CREATE` statements for each object |
| `--title <TITLE>` | Custom document title |
| `-q, --quiet` | Suppress progress output |

### Documented Object Types

The generated documentation covers: tables (with columns, constraints, foreign keys), views (with columns and optional query definition), functions (with parameters and optional body), sequences, and enums.

### Examples

```bash
# Generate Markdown docs
pgc docs generate @production -o SCHEMA.md

# Generate HTML documentation with DDL
pgc docs gen @production --format html --include-ddl -o docs/schema.html

# Document only tables and views from a snapshot
pgc docs gen snapshot.json --only tables,views -o docs.md
```

---

## `pgc report`

Generates a rich comparison report, optionally including the migration script. Supports HTML (with dark mode), Markdown, and JSON output.

### Syntax

```
pgc report <source> <target> [options]
```

### Options

| Option | Description |
|---|---|
| `-o, --output <FILE>` | Output file path (default: `report.html`) |
| `-f, --format <FORMAT>` | Report format: `html` (default), `markdown` / `md`, `json` |
| `--title <TITLE>` | Custom report title |
| `--include-script` | Include the migration script in the report |
| `--include-details` | Include detailed property differences (default: `true`) |
| `-s, --schema <SCHEMA>` | Compare only specific schema(s) |
| `--only <TYPES>` | Compare only specific object types |
| `--ignore <OPTIONS>` | Comma-separated properties to ignore |
| `-q, --quiet` | Suppress progress output |
| `-c, --config <FILE>` | Path to config file |

### Examples

```bash
# Generate an HTML report for stakeholders
pgc report @prod @staging -o report.html --include-script

# Markdown report for a pull request comment
pgc report @prod @staging --format markdown -o changes.md --title "Release 2.0 Changes"

# JSON report for CI/CD processing
pgc report source.sql target.sql --format json -o report.json
```

---

## `pgc validate`

Validates SQL scripts, configuration files, database connections, or snapshot files. Automatically detects the type based on the file extension or URI format, or you can force a type with `--type`.

### Syntax

```
pgc validate <target> [options]
```

### Options

| Option | Description |
|---|---|
| `--type <TYPE>` | Force validation type: `sql`, `config`, `connection`, `snapshot` |
| `--strict` | Fail (exit code 1) on warnings, not just errors |
| `-f, --format <FORMAT>` | Output format: `human` (default) or `json` |
| `--timeout <SECONDS>` | Connection timeout in seconds (default: `30`) |

### What Gets Validated

| Type | Checks |
|---|---|
| `sql` | File exists, statement count, dangerous statements (`DROP DATABASE`, `TRUNCATE`), balanced transactions |
| `config` | YAML parses correctly, environments have URIs |
| `connection` | Database is reachable, returns server version |
| `snapshot` | Valid JSON, expected structure |

### Examples

```bash
# Validate a migration script
pgc validate migration.sql

# Validate config, fail on warnings
pgc validate pgc.yaml --strict

# Test a database connection
pgc validate @production --timeout 5

# JSON output for CI
pgc validate migration.sql --format json
```

---

## `pgc env`

Manage saved connection environments. Environments are stored in `~/.pgc/environments.json`.

### Commands

#### `pgc env add <name> --uri <uri>`

Add or update a connection environment.

| Option | Description |
|---|---|
| `-u, --uri <URI>` | **(Required)** PostgreSQL connection URI |
| `-r, --readonly` | Mark the environment as read-only (prevents `pgc apply`) |
| `-d, --description <TEXT>` | Optional description |

#### `pgc env list`

List all saved environments.

| Option | Description |
|---|---|
| `--json` | Output as JSON |

#### `pgc env remove <name>`

Remove an environment.

| Option | Description |
|---|---|
| `-f, --force` | Don't ask for confirmation |

### Examples

```bash
# Add a production environment (read-only for safety)
pgc env add prod --uri "postgres://user:pass@prod-host/db" --readonly --description "Production"

# Add a staging environment
pgc env add staging --uri "postgres://user:pass@staging-host/db"

# List all environments
pgc env list

# List as JSON (for scripting)
pgc env list --json

# Remove an environment
pgc env remove staging --force
```

---

## `pgc config`

Manage project-level configuration using a `pgc.yaml` file. See [Configuration](/docs/cli/configuration/) for full details.

### Commands

#### `pgc config init`

Create an example `pgc.yaml` in the current directory.

| Option | Description |
|---|---|
| `-f, --force` | Overwrite an existing config file |
| `-o, --output <FILE>` | Output file path (default: `pgc.yaml`) |

#### `pgc config show`

Show the currently resolved configuration.

| Option | Description |
|---|---|
| `-c, --config <FILE>` | Path to config file |

---

## `pgc health`

Check database connectivity and CLI health.

### Syntax

```
pgc health [target] [options]
```

### Options

| Option | Description |
|---|---|
| `--all-envs` | Check all configured environments |
| `--timeout <SECONDS>` | Connection timeout in seconds (default: `10`) |
| `-f, --format <FORMAT>` | Output format: `human` (default) or `json` |

### Examples

```bash
# Check CLI health only
pgc health

# Check a specific database
pgc health @production

# Check all environments (great for CI health checks)
pgc health --all-envs --format json
```

---

## `pgc license`

Show license information. Alias: `pgc licence`.

The `license`, `env`, and `config` commands can always run, even without a valid license.
