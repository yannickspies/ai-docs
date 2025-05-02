# SQLite MCP Server

## Installation Command

Add the SQLite MCP server with local scope (specific to current project):

```bash
claude mcp add --scope local sqlite -- uvx mcp-server-sqlite --db-path ./db.sqlite
```

## Usage

This server provides tools for SQLite database interactions:

### Query Tools
- `read_query`: Execute SELECT queries
- `write_query`: Execute INSERT/UPDATE/DELETE queries
- `create_table`: Create new tables

### Schema Tools
- `list_tables`: Get all tables
- `describe-table`: View table schema

### Analysis Tools
- `append_insight`: Add business insights to memo

## Resources
- `memo://insights`: Business insights memo

## Prompts
- `mcp-demo`: Interactive database operations guide