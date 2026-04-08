---
title: MCP Server for AI Agents
description: Integrate PostgresCompare with AI coding assistants like Claude Desktop and Claude Code using the Model Context Protocol (MCP).
category: cli
order: 12
permalink: /docs/cli/mcp-server/
---

PostgresCompare includes a built-in MCP (Model Context Protocol) server that allows AI coding assistants to interact with your PostgreSQL databases for schema exploration, comparison, and migration generation.

## Quick Start

Start the MCP server:

```bash
pgc mcp serve
```

The server runs on stdio using JSON-RPC, making it compatible with any MCP client.

## Claude Desktop Configuration

Add PostgresCompare to your Claude Desktop MCP configuration (`claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "postgres-compare": {
      "command": "pgc",
      "args": ["mcp", "serve"]
    }
  }
}
```

For production use with safety controls:

```json
{
  "mcpServers": {
    "postgres-compare": {
      "command": "pgc",
      "args": ["mcp", "serve", "--read-only", "--allowed-envs", "dev,staging"]
    }
  }
}
```

## Available Tools

The MCP server exposes 10 tools that AI agents can use:

### Schema Exploration

| Tool | Description |
|------|-------------|
| `list_environments` | List all configured database environments from `pgc.yaml` |
| `get_schema` | Get table, view, and function definitions for a database |
| `health_check` | Test database connectivity and return server version |

### Schema Comparison

| Tool | Description |
|------|-------------|
| `compare_schemas` | Compare two schemas and return a diff summary |
| `explain_difference` | Get detailed per-property diff for a specific object |
| `detect_drift` | Compare current database state against a baseline snapshot |

### Migration & Scripting

| Tool | Description |
|------|-------------|
| `generate_migration` | Generate a SQL migration script between two sources |
| `validate_sql` | Parse SQL, count statements, detect dangerous operations |
| `apply_migration` | Apply a SQL script to a database (with safety guards) |

### Snapshots

| Tool | Description |
|------|-------------|
| `create_snapshot` | Capture a database schema to a JSON snapshot file |

## Safety Controls

### Read-Only Mode

Prevent any write operations:

```bash
pgc mcp serve --read-only
```

In read-only mode, the `apply_migration` tool will refuse to execute any SQL.

### Environment Restrictions

Limit which environments the AI can access:

```bash
pgc mcp serve --allowed-envs dev,staging
```

Attempts to access other environments will be rejected.

### Apply Migration Guards

The `apply_migration` tool includes built-in safety features:

- Respects environment-level `readOnly: true` settings in `pgc.yaml`
- Supports `maxDrops` parameter to limit destructive operations
- Supports `maxStatements` parameter to limit script size
- Requires explicit confirmation for production environments

## Example Interactions

Once configured, you can ask your AI assistant questions like:

- "What tables are in my dev database?"
- "Compare the schema between dev and production"
- "Generate a migration script to sync staging with production"
- "Check if there's any schema drift from our baseline"
- "Explain what changed in the users table between these two snapshots"

## Data Sources

The MCP tools accept the same data source formats as the CLI:

- **Environment references**: `@dev`, `@production` (from `pgc.yaml`)
- **Connection strings**: `postgres://user:pass@host:5432/db`
- **Snapshot files**: `./baseline.json`
- **SQL files**: `./schema.sql` or `./schema-folder/`
- **Git refs**: `git:main:schema.sql`

## Troubleshooting

### Server won't start

Ensure the `pgc` CLI is on your PATH and you have a valid license.

### Environment not found

Check that your `pgc.yaml` configuration file exists and contains the environment definition.

### Connection refused

Verify database connectivity with `pgc health @env-name` before using MCP.

## See Also

- [CLI Installation](/docs/cli/installation)
- [CLI Configuration](/docs/cli/configuration)
- [CLI Commands Reference](/docs/cli/commands)
