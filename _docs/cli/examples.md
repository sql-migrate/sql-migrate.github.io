---
title: CLI Examples
description: Practical examples and CI/CD integration for the PostgresCompare CLI
category: cli
order: 11
permalink: /docs/cli/examples/
---

This page provides practical examples for common use cases, including local development workflows, CI/CD integration, and automated reporting.

## Daily Development

### Quick Schema Drift Check

Check if your local development database has drifted from the canonical schema. The `quiet` format produces no output and relies entirely on the exit code.

```bash
pgc diff postgres://localhost/dev_db schema/main.sql --format quiet
echo "Exit code: $?"
# 0 = identical, 1 = differences, 2 = error
```

### Generate a Migration Script

Create a migration script to update staging to match the current state of production:

```bash
pgc script @production @staging -o migrations/sync_staging.sql
```

Add progress logging so you can monitor execution in psql or any SQL client:

```bash
pgc script @production @staging --progress-logging -o migrations/sync_staging.sql
```

### Explore Changes Interactively

Before generating a script, use interactive mode to browse differences in a tree view:

```bash
pgc i @staging @production
```

### Generate Schema Documentation

Produce a Markdown reference of your production schema:

```bash
pgc docs generate @production -o SCHEMA.md --include-ddl
```

### Validate Before You Ship

Validate your migration script for common issues before applying:

```bash
pgc validate migration.sql --strict
```

Then apply it with safety guardrails:

```bash
pgc apply @staging migration.sql --dry-run
pgc apply @staging migration.sql --confirm --max-drops 3
```

---

## CI/CD Integration

### Exit Codes for Pipelines

All comparison commands return structured exit codes:

| Code | Meaning | CI Interpretation |
|---|---|---|
| `0` | Schemas are identical | ✅ Pass |
| `1` | Differences found | ⚠️ Fail or warn (your choice) |
| `2` | Error | ❌ Fail |

### GitHub Actions: Schema Drift Check on Pull Request

Compare the schema file in the PR branch against `main`. The `github` format creates annotations directly on the PR.

```yaml
# .github/workflows/schema_check.yml
name: Schema Drift Check

on:
  pull_request:
    paths:
      - 'db/schema.sql'

jobs:
  check-schema:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0  # Full history for git ref comparison

      - name: Install pgc
        run: |
          # Download pgc from your CI secret or artifact storage
          # See https://www.postgrescompare.com/downloads/ to get the binary
          chmod +x pgc
          sudo mv pgc /usr/local/bin/

      - name: Compare schema against main
        run: |
          pgc diff \
            git:main:db/schema.sql \
            git:${{ github.head_ref }}:db/schema.sql \
            --format github
```

### GitHub Actions: Generate and Archive a Deployment Script

Generate a migration script on every push to `main` and store it as a build artifact:

```yaml
# .github/workflows/build_migration.yml
name: Build Migration Script

on:
  push:
    branches: [main]

jobs:
  build-script:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install pgc
        run: # ... (see above)

      - name: Generate migration script
        env:
          PROD_USER: ${{ secrets.PROD_USER }}
          PROD_PASS: ${{ secrets.PROD_PASS }}
        run: |
          pgc script \
            "postgres://${PROD_USER}:${PROD_PASS}@prod-host/mydb" \
            git:main:db/schema.sql \
            --direction to-source \
            --progress-logging \
            -o deploy.sql

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: migration-script
          path: deploy.sql
```

### GitLab CI: Validate Schema Before Deployment

Validate that the production database matches the committed schema file. If there's drift, the pipeline fails.

```yaml
# .gitlab-ci.yml
stages:
  - validate
  - deploy

schema-check:
  stage: validate
  script:
    - pgc diff @production db/schema.sql --format quiet
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
```

### Health Checks in CI

Use `pgc health` to verify all database connections before running migrations:

```yaml
- name: Verify database connectivity
  run: pgc health --all-envs --format json
```

### JUnit Output for Test Reporters

Many CI systems (Jenkins, GitHub Actions, Azure DevOps, GitLab) can parse JUnit XML to display test results natively:

```bash
pgc diff @prod @staging --format junit > test-results.xml
```

In GitHub Actions:

```yaml
- name: Compare schemas
  run: pgc diff @prod @staging --format junit > test-results.xml
  continue-on-error: true

- name: Publish test results
  uses: dorny/test-reporter@v1
  with:
    name: Schema Comparison
    path: test-results.xml
    reporter: java-junit
```

---

## Monitoring and Alerting

### Watch for Schema Drift

Monitor a production database for unexpected changes. When drift is detected, trigger a notification:

```bash
pgc watch @production db/schema.sql \
  --interval 300 \
  --on-change "curl -X POST https://hooks.slack.com/services/T.../B.../xxx -d '{\"text\": \"Schema drift detected in production!\"}'"
```

### Periodic Reporting

Generate a nightly comparison report and email it to the team:

```bash
#!/bin/bash
# nightly_schema_report.sh
DATE=$(date +%Y-%m-%d)
pgc report @prod @staging \
  -o "/reports/schema_${DATE}.html" \
  --include-script \
  --title "Nightly Schema Report — ${DATE}"
```
