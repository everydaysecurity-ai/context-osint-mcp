# Cursor

Cursor can't complete Context's browser sign-in. It publishes no OAuth client metadata document and doesn't
register itself dynamically, so it connects with a key header instead.

Put [`mcp.json`](mcp.json) in `~/.cursor/mcp.json` to use it in every project, or in `.cursor/mcp.json` for one
project, with your key in place of `ctx_live_YOUR_KEY`. Keys for the investigation endpoint are issued on
request. Use the form on Context's **MCP Setup** page once you've accepted the license.

Don't commit a project-level `.cursor/mcp.json` that contains a real key.

For the scam check server, use the URL `https://context.everydaysecurity.ai/api/mcp/check` and the header
`"Authorization": "Bearer ctx_live_YOUR_AGENT_KEY"`, with an agent key from **Integrations → Agents**.
