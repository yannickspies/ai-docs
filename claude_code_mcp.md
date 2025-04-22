# Claude Code MCP (Model Context Protocol) Configuration Guide

MCP (Model Context Protocol) allows you to configure additional servers for Claude Code, extending its capabilities.

## Adding MCP Servers

### Add an MCP Stdio Server

```bash
claude mcp add <name> -e API_KEY=your_key -- /path/to/server arg1 arg2
```

### Add an MCP SSE Server

```bash
claude mcp add --transport sse <name> <url>
```

## Managing MCP Servers

```bash
# List all configured servers
claude mcp list

# Get details about a specific server
claude mcp get <name>

# Remove a server
claude mcp remove <name>
```

## Configuration Scopes

Use `-s` or `--scope` to specify configuration scope:

- `local` (default): Personal to current project
- `project`: Shared via `.mcp.json`
- `user`: Available across all projects

Example:
```bash
claude mcp add --scope project my_server -e API_KEY=123 -- /path/to/server
```

## Additional Options

- Set environment variables with `-e` or `--env`
- Configure server timeout with `MCP_TIMEOUT` environment variable
- Check server status by using the `/mcp` command in Claude Code

## Security Warning

Use third-party MCP servers at your own risk due to potential prompt injection vulnerabilities.