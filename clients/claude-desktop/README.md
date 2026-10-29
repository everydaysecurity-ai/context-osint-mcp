# Claude Desktop

**Browser sign-in is not available for Claude Desktop yet.** Its custom-connector sign-in uses an OAuth
registration method that Context does not offer to every account. Until it does, connect with an API key
through [`mcp-remote`](https://www.npmjs.com/package/mcp-remote), a community bridge that runs locally and
forwards Claude Desktop's requests to a remote server. You need Node.js installed.

1. Get a key. Once you've accepted the license, use the request form on Context's **MCP Setup** page.
2. Open Claude Desktop's config file (**Settings → Developer → Edit Config**) and merge in
   [`claude_desktop_config.json`](claude_desktop_config.json), with your key in place of `ctx_live_YOUR_KEY`.
3. Restart Claude Desktop.

The key goes in `env` and reaches the header through `${CONTEXT_API_KEY}`, so the key isn't part of the
command line. `mcp-remote` is maintained outside Everyday Security, and we have not tested this exact setup
end to end. If it doesn't connect, tell us through the feedback form in Context.

For the scam check server, use the same pattern with `https://context.everydaysecurity.ai/api/mcp/check` and
`--header` `Authorization:Bearer ${CONTEXT_AGENT_KEY}`, using an agent key from **Integrations → Agents**.
