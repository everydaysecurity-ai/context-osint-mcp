# Claude Code

## Investigation server (browser sign-in)

```bash
claude mcp add --transport http context https://context.everydaysecurity.ai/api/mcp
```

Then run `/mcp` inside Claude Code and authenticate `context`. A browser window opens for your Everyday Security
sign-in. You copy and paste no key. When it finishes, `/mcp` lists `context` as connected.

Add `--scope user` to make it available in every project, or `--scope project` to write it into the project's
`.mcp.json` file for your team. Teammates still sign in as themselves.

## Scam check server (agent key)

```bash
claude mcp add --transport http context-check https://context.everydaysecurity.ai/api/mcp/check \
  --header "Authorization: Bearer ctx_live_YOUR_AGENT_KEY"
```

Make the agent key in Context under **Integrations → Agents**.
