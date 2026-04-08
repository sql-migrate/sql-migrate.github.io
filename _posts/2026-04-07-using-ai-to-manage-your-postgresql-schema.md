---
layout: post
title: "Using AI to Manage Your PostgreSQL Schema"
author: "The PostgresCompare Team"
date: 2026-04-07
excerpt: "PostgresCompare 1.2.2 introduces MCP server support, letting AI assistants like Claude explore your databases, compare schemas, and generate migration scripts—all through natural conversation."
---

What if you could manage your PostgreSQL schemas by simply *asking*?

"What tables are in my production database?"  
"Compare staging to production and show me the differences."  
"Generate a migration script to sync them."

With **PostgresCompare 1.2.2**, you can. We've added support for the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/), allowing AI coding assistants like Claude Desktop and Claude Code to interact directly with your PostgreSQL databases through PostgresCompare.

This isn't just a gimmick—it's a genuinely useful way to explore schemas, understand drift, and generate migrations without memorizing CLI flags or navigating through UI screens.

## What is MCP?

MCP (Model Context Protocol) is an open standard that allows AI assistants to securely connect to external tools and data sources. Instead of copying and pasting schema definitions into a chat window, the AI can query your databases directly, getting real-time, accurate information.

When you run `pgc mcp serve`, PostgresCompare starts a local server that exposes 10 tools to any MCP-compatible AI assistant:

| Tool | What It Does |
|------|--------------|
| `list_environments` | Shows your configured database connections |
| `get_schema` | Returns table, view, and function definitions |
| `compare_schemas` | Compares two databases and summarizes differences |
| `generate_migration` | Creates a SQL migration script |
| `explain_difference` | Deep-dives into a specific object's changes |
| `detect_drift` | Checks for unexpected changes against a baseline |
| `validate_sql` | Parses SQL and flags dangerous operations |
| `apply_migration` | Executes a migration (with safety guards) |
| `create_snapshot` | Captures a schema to a JSON file |
| `health_check` | Tests database connectivity |

## Setting It Up

### 1. Install the CLI

If you haven't already, [download the PostgresCompare CLI](/get) for your platform. It's a single executable—just add it to your PATH.

### 2. Configure Your Environments

Create a `pgc.yaml` file in your project directory (or run `pgc config init`):

```yaml
environments:
  dev:
    uri: postgres://localhost:5432/myapp_dev
    description: Local development
  staging:
    uri: postgres://staging.example.com/myapp
    description: Staging environment
  prod:
    uri: postgres://prod.example.com/myapp
    readonly: true
    description: Production (read-only)
```

### 3. Add to Claude Desktop

Open your Claude Desktop configuration file:
- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

Add the PostgresCompare MCP server:

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

Restart Claude Desktop, and you're ready to go.

## Real-World Examples

Here's what working with AI-assisted schema management actually looks like.

### Exploring an Unfamiliar Database

You've just joined a project and need to understand the data model. Instead of digging through documentation (if it even exists), just ask:

> **You:** What tables are in the dev database? Give me a quick overview of the schema.

Claude will call `get_schema` and return a summary of tables, their columns, and relationships. You can follow up naturally:

> **You:** Tell me more about the `orders` table. What foreign keys does it have?

### Checking for Schema Drift

Before a deployment, you want to verify that staging matches what you expect:

> **You:** Compare staging against the schema.sql file in this repo. Are there any differences?

Claude calls `compare_schemas` and reports back:

> **Claude:** I found 3 differences between staging and schema.sql:
> - **Table `users`**: Missing column `last_login_at` (exists in schema.sql)
> - **Index `idx_orders_customer`**: Different column order
> - **Function `calculate_total`**: Body differs (schema.sql has updated tax logic)

### Generating a Migration Script

Once you've identified the drift, generating the fix is one message away:

> **You:** Generate a migration script to update staging to match schema.sql

Claude calls `generate_migration` and returns the SQL:

```sql
-- Migration: staging → schema.sql
-- Generated: 2026-04-07 14:32:01

BEGIN;

-- Add missing column
ALTER TABLE public.users 
  ADD COLUMN last_login_at timestamp with time zone;

-- Recreate index with correct column order
DROP INDEX IF EXISTS public.idx_orders_customer;
CREATE INDEX idx_orders_customer ON public.orders (customer_id, created_at);

-- Update function
CREATE OR REPLACE FUNCTION public.calculate_total(order_id integer)
RETURNS numeric AS $$
  -- ... updated function body ...
$$ LANGUAGE sql;

COMMIT;
```

### Understanding a Specific Change

Sometimes you need more detail about what exactly changed:

> **You:** Explain what's different about the calculate_total function between prod and staging

Claude calls `explain_difference` and breaks it down:

> **Claude:** The `calculate_total` function differs in these ways:
> - **Line 12:** Tax rate changed from `0.08` to `0.0825`
> - **Line 15:** Added handling for discount codes
> - **Return type:** Unchanged (`numeric`)

### Validating Before You Deploy

Before running a migration, you can ask Claude to check it:

> **You:** Validate this migration script. Is it safe to run?

Claude calls `validate_sql` and warns you about potential issues:

> **Claude:** ⚠️ This script contains 1 potentially dangerous operation:
> - Line 8: `DROP INDEX` — This will briefly lock the table. Consider using `DROP INDEX CONCURRENTLY` for zero-downtime deployments.
> 
> The script contains 4 statements total and is wrapped in a transaction.

## Safety First

Giving an AI access to your databases might sound scary—but we've built in multiple layers of protection.

### Read-Only Mode

Start the MCP server in read-only mode to prevent any write operations:

```bash
pgc mcp serve --read-only
```

In this mode, the AI can explore schemas and generate scripts, but `apply_migration` will always be rejected.

### Environment Restrictions

Limit which databases the AI can access:

```bash
pgc mcp serve --allowed-envs dev,staging
```

Even if you ask Claude to check production, it won't be able to connect.

### Per-Environment Read-Only

In your `pgc.yaml`, you can mark specific environments as read-only:

```yaml
environments:
  prod:
    uri: postgres://prod.example.com/myapp
    readonly: true  # CLI and MCP will refuse to apply changes here
```

### Apply Guards

Even when writes are allowed, the `apply_migration` tool respects the same safety guards as `pgc apply`:
- Requires explicit confirmation
- Supports `maxDrops` to limit destructive operations
- Supports `maxStatements` to prevent runaway scripts

## Beyond Claude Desktop

The MCP server works with any MCP-compatible client, including:

- **Claude Code** (Anthropic's CLI coding assistant)
- **Zed Editor** (with MCP extension)
- **Custom integrations** using the MCP SDK

Since it uses stdio JSON-RPC, you can also script it directly for advanced automation scenarios.

## Getting Started

1. **Update to 1.2.2:** [Download the latest release](/get)
2. **Configure your environments:** Create a `pgc.yaml` with your database connections
3. **Start the MCP server:** Run `pgc mcp serve` (add `--read-only` for safety)
4. **Connect your AI assistant:** Add the server to Claude Desktop's config
5. **Start asking questions:** "What tables are in my database?"

For complete setup instructions and the full list of available tools, see the [MCP Server documentation](/docs/cli/mcp-server/).

---

AI-assisted database management isn't science fiction—it's available today in PostgresCompare 1.2.2. Whether you're onboarding onto a new project, debugging schema drift, or just tired of writing SQL by hand, give it a try.

**Ready to let AI help manage your schemas?** [Download PostgresCompare 1.2.2](/get) and start the conversation.
