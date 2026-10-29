# Any streamable HTTP client

Any MCP client that can send a header can connect.

| | Investigation server | Scam check server |
|---|---|---|
| URL | `https://context.everydaysecurity.ai/api/mcp` | `https://context.everydaysecurity.ai/api/mcp/check` |
| Transport | streamable HTTP | streamable HTTP |
| Header | `X-API-Key: ctx_live_YOUR_KEY` or `Authorization: Bearer ctx_live_YOUR_KEY` | `Authorization: Bearer ctx_live_YOUR_AGENT_KEY` (or `X-API-Key`) |
| Key from | the request form on Context's **MCP Setup** page | **Integrations → Agents** in Context |

Both servers are stateless and answer `POST` with JSON. An unauthenticated request to the investigation server
gets `401` and a pointer to its OAuth protected-resource metadata. A client that does OAuth discovery can find
the sign-in that way, but it succeeds only for registered clients (see [../README.md](../README.md)).

[`check-server.json`](check-server.json) is the check server in the common `mcpServers` format.

## Talking to it by hand

To see the raw protocol, list the tools with any HTTP client:

```bash
# 1. initialize
curl -sS https://context.everydaysecurity.ai/api/mcp \
  -H "Authorization: Bearer $CONTEXT_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-06-18","capabilities":{},"clientInfo":{"name":"curl","version":"1"}}}'

# 2. list tools
curl -sS https://context.everydaysecurity.ai/api/mcp \
  -H "Authorization: Bearer $CONTEXT_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -H "MCP-Protocol-Version: 2025-06-18" \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/list"}'
```

`scripts/gen-tools --live` does the same thing and renders the result as [TOOLS.md](../../TOOLS.md).
