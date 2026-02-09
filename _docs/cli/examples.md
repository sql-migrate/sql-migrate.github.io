---
title: CLI Examples
description: Common PostgresCompare CLI usage patterns
category: cli
order: 10
permalink: /docs/cli/examples/
---

This page provides practical examples of using the PostgresCompare CLI for common scenarios.

## Basic Comparison

Compare two databases and generate a deployment script:

```bash
pgcompare compare \
  --source "host=source.example.com dbname=production user=readonly" \
  --target "host=localhost dbname=development user=postgres" \
  --output changes.sql
```

## CI/CD Pipeline

### GitHub Actions

```yaml
name: Database Schema Check

on:
  pull_request:
    paths:
      - 'migrations/**'

jobs:
  schema-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Download PostgresCompare
        run: |
          curl -L https://downloads.postgrescompare.com/cli/linux/pgcompare -o pgcompare
          chmod +x pgcompare

      - name: Compare schemas
        env:
          PGCOMPARE_LICENSE: ${{ secrets.PGCOMPARE_LICENSE }}
          PGCOMPARE_SOURCE_PASSWORD: ${{ secrets.PROD_DB_PASSWORD }}
          PGCOMPARE_TARGET_PASSWORD: ${{ secrets.STAGING_DB_PASSWORD }}
        run: |
          ./pgcompare compare \
            --source-host ${{ vars.PROD_DB_HOST }} \
            --source-database production \
            --source-user readonly \
            --target-host ${{ vars.STAGING_DB_HOST }} \
            --target-database staging \
            --target-user deployer \
            --report schema-diff.html \
            --report-format html

      - name: Upload report
        uses: actions/upload-artifact@v4
        with:
          name: schema-diff
          path: schema-diff.html
```

### GitLab CI

```yaml
schema-compare:
  stage: test
  script:
    - pgcompare compare
        --source-host $PROD_DB_HOST
        --source-database production
        --source-user $PROD_DB_USER
        --source-password $PROD_DB_PASSWORD
        --target-host $STAGING_DB_HOST
        --target-database staging
        --target-user $STAGING_DB_USER
        --target-password $STAGING_DB_PASSWORD
        --output diff.sql
        --report report.json
        --report-format json
  artifacts:
    paths:
      - diff.sql
      - report.json
    when: always
```

### Jenkins

```groovy
pipeline {
    agent any

    environment {
        PGCOMPARE_LICENSE = credentials('pgcompare-license')
        PROD_DB_CREDS = credentials('prod-db-credentials')
    }

    stages {
        stage('Compare Schemas') {
            steps {
                sh '''
                    pgcompare compare \
                        --source-host prod-db.internal \
                        --source-database production \
                        --source-user ${PROD_DB_CREDS_USR} \
                        --source-password ${PROD_DB_CREDS_PSW} \
                        --target-host staging-db.internal \
                        --target-database staging \
                        --target-user deployer \
                        --output deploy.sql
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'deploy.sql', allowEmptyArchive: true
        }
    }
}
```

## Automated Snapshots

Create daily snapshots of your production database:

```bash
#!/bin/bash
# daily-snapshot.sh

DATE=$(date +%Y%m%d)
SNAPSHOT_DIR="/backups/schema-snapshots"

pgcompare snapshot \
  --host prod.example.com \
  --database production \
  --user readonly \
  --output "${SNAPSHOT_DIR}/production-${DATE}.pgs"

# Keep only last 30 snapshots
find "${SNAPSHOT_DIR}" -name "*.pgs" -mtime +30 -delete

echo "Snapshot created: production-${DATE}.pgs"
```

Schedule with cron:

```cron
0 2 * * * /scripts/daily-snapshot.sh >> /var/log/schema-snapshots.log 2>&1
```

## Migration Validation

Validate migrations before deployment:

```bash
#!/bin/bash
# validate-migration.sh

set -e

echo "Creating pre-migration snapshot..."
pgcompare snapshot \
  --connection "$TARGET_DB" \
  --output pre-migration.pgs

echo "Applying migrations..."
psql "$TARGET_DB" -f migrations/latest.sql

echo "Comparing against expected schema..."
pgcompare compare \
  --source expected-schema.pgs \
  --target "$TARGET_DB" \
  --report validation.html \
  --report-format html

status=$?

if [ $status -eq 0 ]; then
  echo "Migration validated successfully"
elif [ $status -eq 1 ]; then
  echo "WARNING: Unexpected differences after migration"
  echo "Review validation.html for details"
  exit 1
else
  echo "ERROR: Comparison failed"
  exit $status
fi
```

## Multi-Environment Comparison

Compare one source against multiple targets:

```bash
#!/bin/bash
# compare-all-environments.sh

SOURCE="production-snapshot.pgs"
ENVIRONMENTS=("staging" "qa" "development")

for env in "${ENVIRONMENTS[@]}"; do
  echo "Comparing against ${env}..."

  pgcompare compare \
    --source "$SOURCE" \
    --target "host=${env}-db.internal dbname=app user=readonly" \
    --output "deploy-to-${env}.sql" \
    --report "${env}-report.json" \
    --report-format json \
    --quiet

  status=$?

  if [ $status -eq 0 ]; then
    echo "  ${env}: In sync"
  elif [ $status -eq 1 ]; then
    echo "  ${env}: Differences found (see deploy-to-${env}.sql)"
  else
    echo "  ${env}: ERROR"
  fi
done
```

## Generating Reports

Create an HTML report for review:

```bash
pgcompare compare \
  --source production \
  --target staging \
  --report schema-diff.html \
  --report-format html
```

Create a JSON report for processing:

```bash
pgcompare compare \
  --source production \
  --target staging \
  --report diff.json \
  --report-format json

# Process with jq
cat diff.json | jq '.differences | length'
cat diff.json | jq '.differences[] | select(.type == "table")'
```

## Using Project Files

Run comparison using saved project settings:

```bash
# Use project with all saved settings
pgcompare project run \
  --project myproject.pgc \
  --output deploy.sql

# Override specific settings
pgcompare project run \
  --project myproject.pgc \
  --target-host staging.example.com \
  --output deploy-to-staging.sql
```

## Filtering Comparisons

Compare only specific schemas or object types:

```bash
# Only public schema
pgcompare compare \
  --source production \
  --target staging \
  --schemas public \
  --output public-changes.sql

# Only tables and views
pgcompare compare \
  --source production \
  --target staging \
  --object-types tables,views \
  --output tables-views-changes.sql

# Exclude test schemas
pgcompare compare \
  --source production \
  --target staging \
  --exclude-schemas testing,staging_data \
  --output changes.sql
```

## Error Handling

Robust error handling in scripts:

```bash
#!/bin/bash
set -e

compare_databases() {
  pgcompare compare \
    --source "$1" \
    --target "$2" \
    --output "$3" \
    2>&1

  return $?
}

main() {
  local output_file="deploy-$(date +%Y%m%d-%H%M%S).sql"

  if ! compare_databases "$SOURCE_DB" "$TARGET_DB" "$output_file"; then
    case $? in
      2) echo "Comparison error occurred" ;;
      3) echo "Could not connect to database" ;;
      4) echo "License error" ;;
      *) echo "Unknown error" ;;
    esac
    exit 1
  fi

  if [ -s "$output_file" ]; then
    echo "Differences found. Review: $output_file"
  else
    echo "Databases are in sync"
    rm "$output_file"
  fi
}

main
```

## Next Steps

- [CLI Commands](/docs/cli/commands/) - Full command reference
- [CI/CD Integration](/docs/guides/ci-cd-integration/) - More CI/CD examples
- [Safe Deployments](/docs/guides/safe-deployments/) - Deployment best practices
