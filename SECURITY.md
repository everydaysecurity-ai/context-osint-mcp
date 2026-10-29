# Security

## Reporting a vulnerability

Please report security issues in Context or its MCP servers privately to **support@everydaysecurity.ai** (or use GitHub private vulnerability reporting on this repository). Do not
open a public issue. Include what you found, how to reproduce it, and the endpoint involved. We'll acknowledge
your report and keep you informed as we fix it.

## Scope

- The hosted MCP endpoints `https://context.everydaysecurity.ai/api/mcp` and
  `https://context.everydaysecurity.ai/api/mcp/check`, and the Context web app they belong to.
- The contents of this repository, including `scripts/gen-tools` and the GitHub workflows.

Please don't test against indicators or accounts that aren't yours, and don't run automated scans that degrade
the service for other people.

## Keys

Never put a real key in an issue, pull request or config file you commit. If a key leaks, revoke it in Context
(**Integrations → Agents** for agent keys) or ask us to revoke it, then make a new one.
