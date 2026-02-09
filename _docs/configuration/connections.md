---
title: Connections
description: Configure database connections in PostgresCompare
category: configuration
order: 11
permalink: /docs/configuration/connections/
---

PostgresCompare connects directly to PostgreSQL databases to read schema information. This guide covers connection configuration options.

## Connection Settings

### Basic Settings

| Setting | Description | Example |
|---------|-------------|---------|
| Host | Database server address | `localhost`, `db.example.com` |
| Port | PostgreSQL port | `5432` (default) |
| Database | Database name | `production` |
| Username | Database user | `postgres` |
| Password | User password | (stored securely) |

### Connection String Format

PostgresCompare accepts standard PostgreSQL connection strings:

```
host=localhost port=5432 dbname=mydb user=postgres password=secret
```

Or URI format:

```
postgresql://postgres:secret@localhost:5432/mydb
```

## SSL/TLS Configuration

### SSL Modes

| Mode | Description |
|------|-------------|
| `disable` | No SSL |
| `allow` | Try SSL, fall back to non-SSL |
| `prefer` | Try SSL first (default) |
| `require` | Require SSL, don't verify certificate |
| `verify-ca` | Require SSL, verify server certificate |
| `verify-full` | Require SSL, verify certificate and hostname |

### SSL Settings

```
sslmode=verify-full
sslcert=/path/to/client.crt
sslkey=/path/to/client.key
sslrootcert=/path/to/ca.crt
```

## SSH Tunnel Connections

Connect through an SSH tunnel for secure remote access:

1. In the connection dialog, enable **SSH Tunnel**
2. Configure SSH settings:
   - **SSH Host**: Bastion/jump server
   - **SSH Port**: SSH port (default: 22)
   - **SSH User**: SSH username
   - **Authentication**: Password or private key
   - **Private Key**: Path to SSH private key

The tunnel connects to the SSH host and forwards connections to the database.

### SSH Key Authentication

For passwordless authentication:

1. Generate an SSH key pair if needed
2. Add the public key to the SSH server's `authorized_keys`
3. In PostgresCompare, select **Private Key** authentication
4. Browse to your private key file

## Connection Pooling

PostgresCompare uses a single connection per database. For large schemas:

- Connections may take longer to establish
- Consider increasing `connect_timeout`
- Use read replicas if available

### Timeout Settings

```
connect_timeout=30    # Connection timeout in seconds
statement_timeout=60000   # Query timeout in milliseconds
```

## Required Permissions

PostgresCompare needs read access to schema metadata. Minimum required grants:

```sql
-- Create a read-only user for PostgresCompare
CREATE USER pgcompare_readonly WITH PASSWORD 'secure-password';

-- Grant connect to database
GRANT CONNECT ON DATABASE mydb TO pgcompare_readonly;

-- Grant usage on schemas
GRANT USAGE ON SCHEMA public TO pgcompare_readonly;
GRANT USAGE ON SCHEMA app TO pgcompare_readonly;

-- Grant select on metadata tables
GRANT SELECT ON ALL TABLES IN SCHEMA information_schema TO pgcompare_readonly;
GRANT SELECT ON ALL TABLES IN SCHEMA pg_catalog TO pgcompare_readonly;
```

For comparing privileges and ownership:

```sql
-- Additional grants for privilege comparison
GRANT SELECT ON pg_roles TO pgcompare_readonly;
GRANT SELECT ON pg_auth_members TO pgcompare_readonly;
```

## Saving Connections

### Project Connections

Connections saved in a project file are encrypted and specific to that project.

### Shared Connections

Create shared connections available to all projects:

1. Open **Settings > Connections**
2. Click **Add Connection**
3. Configure and save

Shared connections are stored in your user profile.

## Cloud Database Connections

### Amazon RDS

```
host=mydb.abc123.us-east-1.rds.amazonaws.com
port=5432
dbname=production
user=postgres
sslmode=require
```

### Azure Database for PostgreSQL

```
host=myserver.postgres.database.azure.com
port=5432
dbname=production
user=myuser@myserver
sslmode=require
```

### Google Cloud SQL

Use the Cloud SQL Auth Proxy for secure connections, or:

```
host=<INSTANCE_IP>
port=5432
dbname=production
user=postgres
sslmode=verify-ca
sslrootcert=/path/to/server-ca.pem
sslcert=/path/to/client-cert.pem
sslkey=/path/to/client-key.pem
```

### Heroku Postgres

```
host=<hostname>.compute-1.amazonaws.com
port=5432
dbname=<dbname>
user=<username>
password=<password>
sslmode=require
```

## Troubleshooting Connections

### Connection Refused

- Verify the host and port are correct
- Check firewall rules
- Ensure PostgreSQL is listening on the correct interface

### Authentication Failed

- Verify username and password
- Check `pg_hba.conf` allows your connection method
- For SSL, verify certificate paths

### SSL Errors

- Ensure SSL mode matches server requirements
- Verify certificate files are readable
- Check certificate validity

### Timeout Errors

- Increase `connect_timeout` value
- Check network connectivity
- Consider using SSH tunnel for remote databases

## Next Steps

- [Ignore Rules](/docs/configuration/ignore-rules/) - Configure what to exclude
- [Options](/docs/configuration/options/) - Comparison options
