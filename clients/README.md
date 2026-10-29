# Connecting a client

Context has two MCP endpoints. Both use the streamable HTTP transport.

| Endpoint | What it's for | How you authenticate |
|---|---|---|
| `https://context.everydaysecurity.ai/api/mcp` | Investigations: indicator lookups, crawls and their evidence, autonomous investigations, business-record search | Browser sign-in (OAuth) in Claude Code and VS Code. An API key header in everything else. |
| `https://context.everydaysecurity.ai/api/mcp/check` | Scam checks only: `check_scam` and `get_check` | An agent key, sent as a bearer token |

**Before you start:** you need a Context account with the free license accepted. Sign up at
https://context.everydaysecurity.ai and accept the license there. With browser sign-in you can also connect
first: until you accept, each tool call answers with a message telling you where to.

## Which method does my client use?

That depends on the client, not on you. Browser sign-in works only for clients that publish an OAuth client
metadata document we have registered. Today those are Claude Code and VS Code. Other clients use a key header.

| Client | Method | Config |
|---|---|---|
| Claude Code | Browser sign-in | [claude-code/](claude-code/) |
| VS Code (Copilot agent mode) | Browser sign-in | [vscode/mcp.json](vscode/mcp.json) |
| Cursor | API key header | [cursor/mcp.json](cursor/mcp.json) |
| Claude Desktop | API key header, through a local bridge | [claude-desktop/](claude-desktop/) |
| Any other streamable HTTP client | API key header | [streamable-http/](streamable-http/) |

**Getting a key for `/api/mcp`.** Keys for the investigation endpoint are issued on request. Once you've
accepted the license, open **MCP Setup** in Context and use the request form at the bottom of the page. A key carries your
own access and nobody else's. Keep it out of shared config files and repositories.

**Getting an agent key for `/api/mcp/check`.** Make one yourself in Context under **Integrations → Agents**. It
is shown once and starts with `ctx_live_`. An agent key can run scam checks and nothing else, and you can revoke
one without affecting the others.

Every snippet here uses a placeholder such as `ctx_live_YOUR_KEY`. None of them contains a real credential.
