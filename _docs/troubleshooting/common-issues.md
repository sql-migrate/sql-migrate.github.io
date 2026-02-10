---
title: Common Issues
description: Solutions to frequently encountered problems
category: troubleshooting
order: 14
permalink: /docs/troubleshooting/common-issues/
---

This page covers common issues users encounter with PostgresCompare and their solutions.

## Connection Issues

### Cannot connect to database

**Symptoms:**
- Connection timeout
- Connection refused errors

**Solutions:**

1. **Verify connection details**
   - Double-check host, port, database name, username, and password
   - Test connection with `psql` to confirm credentials work

2. **Check network connectivity**
   ```bash
   # Test if port is reachable
   telnet db.example.com 5432

   # Or use nc
   nc -zv db.example.com 5432
   ```

3. **Check PostgreSQL configuration**
   - Ensure PostgreSQL is listening on the correct interface
   - Check `listen_addresses` in `postgresql.conf`
   - Verify `pg_hba.conf` allows connections from your IP

4. **Firewall rules**
   - Check local firewall settings
   - Check cloud security groups (AWS, Azure, GCP)
   - Check VPN requirements

### SSL connection errors

**Symptoms:**
- "SSL connection is required"
- Certificate verification errors

**Solutions:**

1. **Set appropriate SSL mode**
   - Use `sslmode=require` for encrypted connection without certificate verification
   - Use `sslmode=verify-full` for production with proper certificates

2. **Certificate issues**
   - Ensure certificate files are readable
   - Verify certificate hasn't expired
   - Check certificate chain is complete

### Authentication failed

**Symptoms:**
- "password authentication failed for user"
- "no pg_hba.conf entry for host"

**Solutions:**

1. **Verify credentials**
   - Confirm username and password are correct
   - Check for special characters that may need escaping

2. **Check pg_hba.conf**
   - Ensure your IP/host is listed
   - Verify authentication method (md5, scram-sha-256, etc.)

3. **User permissions**
   - Confirm user exists in the database
   - Verify user has CONNECT privilege

## Comparison Issues

### Comparison takes too long

**Symptoms:**
- Progress bar stalls
- Application becomes unresponsive

**Solutions:**

1. **Limit scope**
   - Use schema filters to compare only relevant schemas
   - Exclude large schemas you don't need

2. **Check database load**
   - Compare during low-usage periods
   - Use a read replica if available

### Missing objects in comparison

**Symptoms:**
- Expected tables/views not appearing
- Objects show as identical when they're different

**Solutions:**

1. **Check schema filters**
   - Review Project Settings > Schema Filters
   - Ensure the schema isn't excluded

2. **Check comparison options**
   - Review Project Settings > Comparison Options
   - Check if the object type is disabled or if an exclusion pattern matches

3. **Check object type filters**
   - Ensure the object type is enabled in settings

4. **Permissions**
   - Verify the database user has SELECT access to the object
   - Some metadata requires additional grants

### Differences shown incorrectly

**Symptoms:**
- Objects marked different when they're the same
- Wrong ALTER statements generated

**Solutions:**

1. **Check comparison options**
   - Review options like "Ignore whitespace" and "Ignore column order"
   - Adjust to match your needs

2. **Clear and refresh**
   - Close and reopen the project
   - Run comparison again

3. **Check PostgreSQL versions**
   - Syntax differences between versions may cause false positives
   - Some features aren't available in older versions

## Script Generation Issues

### Script fails to execute

**Symptoms:**
- Syntax errors when running script
- Dependency errors

**Solutions:**

1. **Check PostgreSQL version compatibility**
   - Generated SQL may use features not in target version
   - Review script for version-specific syntax

2. **Run in order**
   - PostgresCompare orders by dependency
   - Don't reorder statements manually unless necessary

3. **Handle existing objects**
   - Refresh the comparison to get the current state
   - Review the script for conflicts with existing objects

### Foreign key errors

**Symptoms:**
- "insert or update on table violates foreign key constraint"
- "cannot drop table because other objects depend on it"

**Solutions:**

1. **Check data integrity**
   - Ensure referenced data exists before creating foreign keys
   - Handle orphaned records

2. **Use CASCADE**
   - Enable CASCADE for drops if appropriate
   - Be aware this will drop dependent objects

3. **Disable constraints temporarily**
   ```sql
   SET session_replication_role = replica;
   -- Run your script
   SET session_replication_role = DEFAULT;
   ```

### Lock timeout errors

**Symptoms:**
- "canceling statement due to lock timeout"
- "deadlock detected"

**Solutions:**

1. **Deploy during low traffic**
   - Schedule for maintenance windows
   - Reduce concurrent connections

2. **Use lock timeout**
   ```sql
   SET lock_timeout = '5s';
   ```

3. **Split into smaller transactions**
   - Run statements individually
   - Commit between major changes

## Application Issues

### PostgresCompare won't start

**Solutions:**

1. **Check system requirements**
   - Verify OS version is supported
   - Ensure sufficient disk space and memory

2. **Reinstall**
   - Download latest version from website
   - Uninstall and reinstall

3. **Check logs**
   - Check the application logs for error details

### License issues

**Symptoms:**
- "License expired" message
- "Invalid license key"

**Solutions:**

1. **Verify license key**
   - Check for typos in the key
   - Ensure no extra spaces

2. **Check license status**
   - Visit your account at postgrescompare.com
   - Verify license hasn't expired

3. **Internet connectivity**
   - License validation requires internet access
   - Check firewall allows outbound HTTPS

### Crash or hang

**Solutions:**

1. **Save your work**
   - Save projects frequently

2. **Check for updates**
   - Install latest version
   - Review release notes for fixes

3. **Report the issue**
   - Include steps to reproduce
   - Attach log files
   - Contact support@postgrescompare.com

## Getting Help

If you can't resolve your issue:

1. **Check the FAQ** - [FAQ page](/faq/)
2. **Search articles** - [Articles page](/articles/)
3. **Contact support** - [Contact page](/contact/)

When contacting support, include:
- PostgresCompare version
- Operating system
- PostgreSQL version(s)
- Steps to reproduce the issue
- Error messages or screenshots
- Log files if applicable

## Next Steps

- [Error Messages](/docs/troubleshooting/error-messages/) - Specific error explanations
- [FAQ](/faq/) - Frequently asked questions
