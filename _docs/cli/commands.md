---
title: CLI Commands
description: Complete reference for PostgresCompare CLI commands
category: cli
order: 9
permalink: /docs/cli/commands/
---

This page documents all PostgresCompare CLI commands and their options.

## Global Options

These options are available for all commands:

| Option | Description |
|--------|-------------|
| `--help, -h` | Show help for a command |
| `--version` | Show version information |
| `--license <key>` | Use specified license key |
| `--quiet, -q` | Suppress non-essential output |
| `--verbose, -v` | Show detailed output |
| `--output-format` | Output format: `text`, `json`, `xml` |

## compare

Compare two PostgreSQL databases or snapshots.

```bash
pgcompare compare [options]
```

### Connection Options

| Option | Description |
|--------|-------------|
| `--source <connection>` | Source connection string or snapshot file |
| `--target <connection>` | Target connection string or snapshot file |
| `--source-host <host>` | Source database host |
| `--source-port <port>` | Source database port (default: 5432) |
| `--source-database <name>` | Source database name |
| `--source-user <user>` | Source database user |
| `--source-password <pass>` | Source database password |

The same options are available for target with `--target-` prefix.

### Comparison Options

| Option | Description |
|--------|-------------|
| `--schemas <list>` | Compare only these schemas (comma-separated) |
| `--exclude-schemas <list>` | Exclude these schemas |
| `--object-types <list>` | Compare only these types |
| `--ignore-owner` | Ignore ownership differences |
| `--ignore-tablespace` | Ignore tablespace differences |
| `--ignore-privileges` | Ignore permission differences |

### Output Options

| Option | Description |
|--------|-------------|
| `--output, -o <file>` | Write deployment script to file |
| `--report <file>` | Write comparison report to file |
| `--report-format` | Report format: `text`, `html`, `json` |

### Examples

```bash
# Compare two databases
pgcompare compare \
  --source "host=localhost dbname=source_db user=postgres" \
  --target "host=localhost dbname=target_db user=postgres" \
  --output deploy.sql

# Compare with individual options
pgcompare compare \
  --source-host prod.example.com \
  --source-database production \
  --source-user readonly \
  --source-password secret \
  --target-host localhost \
  --target-database development \
  --target-user postgres \
  --output differences.sql

# Compare snapshot to database
pgcompare compare \
  --source production.pgs \
  --target "host=localhost dbname=staging" \
  --output staging-update.sql
```

## snapshot

Create a snapshot of a database schema.

```bash
pgcompare snapshot [options]
```

### Options

| Option | Description |
|--------|-------------|
| `--connection <string>` | Database connection string |
| `--host <host>` | Database host |
| `--port <port>` | Database port |
| `--database <name>` | Database name |
| `--user <user>` | Database user |
| `--password <pass>` | Database password |
| `--output, -o <file>` | Snapshot output file (.pgs) |
| `--schemas <list>` | Include only these schemas |
| `--exclude-schemas <list>` | Exclude these schemas |

### Examples

```bash
# Create snapshot
pgcompare snapshot \
  --host prod.example.com \
  --database production \
  --user readonly \
  --output production-$(date +%Y%m%d).pgs

# Snapshot specific schemas
pgcompare snapshot \
  --connection "host=localhost dbname=myapp" \
  --schemas "public,app" \
  --output public-schema.pgs
```

## project

Work with PostgresCompare project files.

```bash
pgcompare project <subcommand> [options]
```

### Subcommands

#### project run

Run comparison using a project file:

```bash
pgcompare project run \
  --project myproject.pgc \
  --output deploy.sql
```

#### project export-rules

Export ignore rules from a project:

```bash
pgcompare project export-rules myproject.pgc > rules.yaml
```

#### project import-rules

Import ignore rules into a project:

```bash
pgcompare project import-rules myproject.pgc < rules.yaml
```

## Environment Variables

Configure the CLI using environment variables:

| Variable | Description |
|----------|-------------|
| `PGCOMPARE_LICENSE` | License key |
| `PGCOMPARE_SOURCE_PASSWORD` | Source database password |
| `PGCOMPARE_TARGET_PASSWORD` | Target database password |
| `PGCOMPARE_OUTPUT_FORMAT` | Default output format |

Using environment variables for passwords is more secure than command-line arguments:

```bash
export PGCOMPARE_SOURCE_PASSWORD="source-secret"
export PGCOMPARE_TARGET_PASSWORD="target-secret"

pgcompare compare \
  --source-host prod --source-database app --source-user app \
  --target-host dev --target-database app --target-user app \
  --output deploy.sql
```

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success, no differences found |
| 1 | Success, differences found |
| 2 | Error during comparison |
| 3 | Connection error |
| 4 | License error |
| 5 | Invalid arguments |

Use exit codes in scripts:

```bash
pgcompare compare --source prod --target staging --output diff.sql
status=$?

if [ $status -eq 0 ]; then
  echo "Databases are in sync"
elif [ $status -eq 1 ]; then
  echo "Differences found, review diff.sql"
else
  echo "Error occurred: $status"
  exit $status
fi
```

## Next Steps

- [CLI Examples](/docs/cli/examples/) - Common usage patterns
- [CI/CD Integration](/docs/guides/ci-cd-integration/) - Automate comparisons
