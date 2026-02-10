---
title: Error Messages
description: Explanations and solutions for PostgresCompare error messages
category: troubleshooting
order: 15
permalink: /docs/troubleshooting/error-messages/
---

This reference explains error messages you may encounter in PostgresCompare and how to resolve them.

## Connection Errors

### "Connection refused"

```
could not connect to server: Connection refused
Is the server running on host "localhost" and accepting TCP/IP connections on port 5432?
```

**Cause:** PostgreSQL is not running or not accepting connections on the specified host/port.

**Solution:**
1. Verify PostgreSQL is running
2. Check the host and port are correct
3. Ensure PostgreSQL is configured to accept TCP/IP connections

### "No pg_hba.conf entry"

```
FATAL: no pg_hba.conf entry for host "192.168.1.100", user "postgres", database "mydb"
```

**Cause:** The PostgreSQL server's access control file doesn't allow connections from your IP.

**Solution:**
1. Add an entry to `pg_hba.conf` for your IP address
2. Reload PostgreSQL configuration: `SELECT pg_reload_conf();`

### "Password authentication failed"

```
FATAL: password authentication failed for user "postgres"
```

**Cause:** Incorrect username or password.

**Solution:**
1. Verify the username and password
2. Reset the password if needed: `ALTER USER postgres PASSWORD 'newpassword';`

### "SSL required"

```
FATAL: SSL connection is required. Please enable SSL for the connection.
```

**Cause:** Server requires SSL but connection isn't using it.

**Solution:**
1. Set `sslmode=require` in connection settings
2. Or configure full SSL with certificates

### "Certificate verify failed"

```
SSL error: certificate verify failed
```

**Cause:** SSL certificate validation failed.

**Solution:**
1. Use `sslmode=require` instead of `verify-full` for testing
2. Ensure certificate files are correct and valid
3. Check certificate hasn't expired

## Comparison Errors

### "Permission denied for relation"

```
ERROR: permission denied for relation pg_class
```

**Cause:** Database user lacks required permissions.

**Solution:**
Grant necessary permissions:
```sql
GRANT SELECT ON ALL TABLES IN SCHEMA pg_catalog TO your_user;
GRANT SELECT ON ALL TABLES IN SCHEMA information_schema TO your_user;
```

### "Relation does not exist"

```
ERROR: relation "schema_name.table_name" does not exist
```

**Cause:** Referenced object doesn't exist or user can't see it.

**Solution:**
1. Verify the object exists
2. Check schema search_path
3. Verify user has access to the schema

### "Out of memory"

```
Out of memory. The application will close.
```

**Cause:** Not enough memory to hold schema metadata.

**Solution:**
1. Close other applications
2. Compare schemas separately
3. Use filters to reduce scope
4. Increase system memory

### "Query timeout"

```
ERROR: canceling statement due to statement timeout
```

**Cause:** Query took longer than allowed.

**Solution:**
1. Increase timeout in connection settings
2. Check database performance
3. Compare during low-usage periods

## Script Execution Errors

### "Syntax error"

```
ERROR: syntax error at or near "PROCEDURE"
```

**Cause:** Generated SQL uses syntax not supported by target PostgreSQL version.

**Solution:**
1. Check PostgreSQL version compatibility
2. Review and modify unsupported syntax
3. Update PostgreSQL if possible

### "Cannot drop because other objects depend on it"

```
ERROR: cannot drop table orders because other objects depend on it
HINT: Use DROP ... CASCADE to drop the dependent objects too.
```

**Cause:** Object has dependencies that prevent dropping.

**Solution:**
1. Manually add CASCADE to the relevant DROP statements in the script
2. Or handle dependencies first before running the script
3. Review dependent objects before proceeding

### "Duplicate key value"

```
ERROR: duplicate key value violates unique constraint "users_email_key"
```

**Cause:** Trying to create data or constraint that conflicts with existing data.

**Solution:**
1. Clean up duplicate data first
2. Review constraint changes
3. Handle data migration separately

### "Foreign key violation"

```
ERROR: insert or update on table "orders" violates foreign key constraint "orders_user_id_fkey"
```

**Cause:** Foreign key references non-existent data.

**Solution:**
1. Create referenced data first
2. Disable constraints during migration
3. Fix data integrity issues

### "Lock not available"

```
ERROR: could not obtain lock on relation "users"
```

**Cause:** Another process holds a conflicting lock.

**Solution:**
1. Wait for other transactions to complete
2. Run during low-traffic period
3. Set appropriate lock timeout
4. Identify and resolve blocking queries

### "Column does not exist"

```
ERROR: column "new_column" of relation "users" does not exist
```

**Cause:** Script references a column that doesn't exist.

**Solution:**
1. Refresh comparison to get current schema
2. Ensure script steps are in correct order
3. Check for failed earlier migrations

## License Errors

### "License key is invalid"

```
The license key you entered is not valid. Please check and try again.
```

**Cause:** License key is malformed or incorrect.

**Solution:**
1. Copy key directly from email/portal
2. Remove any extra spaces
3. Verify all characters are correct

### "License has expired"

```
Your license has expired. Please renew to continue using PostgresCompare.
```

**Cause:** License subscription has ended.

**Solution:**
1. Renew license at postgrescompare.com
2. Enter new license key
3. Contact sales for extension

### "License server unreachable"

```
Could not connect to license server. Please check your internet connection.
```

**Cause:** Cannot reach license validation server.

**Solution:**
1. Check internet connection
2. Check firewall/proxy settings
3. Try again later

## File Errors

### "Project file is corrupted"

```
The project file appears to be corrupted and cannot be opened.
```

**Cause:** The project database is damaged or invalid.

**Solution:**
1. Try opening a backup of the project
2. Create a new project and reconfigure your environments
3. Contact support with details of the issue

### "Cannot write to file"

```
Cannot write to the specified file. Check permissions and try again.
```

**Cause:** No write permission to the target directory.

**Solution:**
1. Choose a different location
2. Check file/folder permissions
3. Run as administrator (Windows)

### "File is in use"

```
The file is in use by another process.
```

**Cause:** Another application has the file open.

**Solution:**
1. Close other applications using the file
2. Check for PostgresCompare instances
3. Restart the application

## Getting More Help

If you encounter an error not listed here:

1. **Check the error code** - Search online for the specific PostgreSQL error code
2. **Review logs** - Check PostgresCompare log files for details
3. **Contact support** - Email support@postgrescompare.com with:
   - Full error message
   - Steps to reproduce
   - Log files
   - PostgresCompare and PostgreSQL versions

## Next Steps

- [Common Issues](/docs/troubleshooting/common-issues/) - General troubleshooting
- [FAQ](/faq) - Frequently asked questions
- [Contact Support](/contact) - Get help from our team
