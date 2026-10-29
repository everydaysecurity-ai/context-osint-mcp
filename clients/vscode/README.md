# VS Code

Add [`mcp.json`](mcp.json) to `.vscode/mcp.json` in your project, or to your user configuration with the
**MCP: Open User Configuration** command. The first time the server is used, VS Code asks you to sign in with
your Everyday Security account. You paste no key.

VS Code spells the top-level key `servers` and uses `"type": "http"`. Cursor uses `mcpServers`. If you use one
tool's format in the other, the server is silently ignored.
