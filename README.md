# Context MCP server for OSINT and threat-intel investigations

Connect Claude, Cursor, VS Code or any MCP client to **Context**, the AI-assisted investigation platform from
[Everyday Security](https://everydaysecurity.ai/). Look up domains, URLs, IP addresses and ASNs, email addresses,
phone numbers, crypto wallets, and state business records. Run autonomous investigations of suspicious sites,
and read the evidence those investigations collect.

**Every answer shows its evidence.** Many AI tools give you a conclusion you can't check. Context keeps facts
and conclusions apart:

- **Facts carry their provenance.** An indicator lookup tells you which sources reported it, when it was first
  and last seen, which investigations it appeared in, and how it connects to other indicators. Each connection
  says whether it comes from a verified record or only from the two appearing together in an investigation.
- **Verdicts are labelled as conclusions and cite what they rest on.** An investigation's verdict is a category
  (for example `scam` or `inconclusive`) with a confidence band. It lists each piece of evidence it rests on,
  with the source class (third-party report, our own capture, a scorer, or a language model's reading of the
  page), the strength, the time of the capture, and the graph node you can open to check it.
- **No evidence is not reported as "safe".** Without strong evidence the verdict is `inconclusive`, and a scam
  check answers `caution`, which is explicitly not the same as safe. A list that was cut short says so, and so
  does a lookup that failed. Neither is shown as an empty result.
- **Raw evidence is readable.** For a crawled site you can pull the captured DOM, console log, network log,
  HAR file and screenshot into your client.

This repository holds the setup guides, client configs, worked examples and the tool reference. The server
itself is hosted, so there is nothing to install for most clients. Claude Desktop needs a small local bridge
(see [clients/claude-desktop](clients/claude-desktop/)).

## Who it's for

Investigators who want an AI assistant for OSINT work (domain and infrastructure pivots, scam and fraud
investigations, business-registry research) without losing sight of where each claim came from. It also
suits teams whose own AI agents should check a link, number or address before they act on it.

## Getting access

Context has a free tier, and anyone can sign up. There's no request form and no waiting list.

1. Connect your client (see the quick start below). The first time, it opens a browser window where you sign in
   or create an account.
2. Open https://context.everydaysecurity.ai once, signed in with the same account, and accept the free license.

Until you've accepted the license, your client still connects and lists the tools, but each tool call answers
with a message telling you where to accept. Once you accept, run the call again. You don't need to reconnect.
A small number of sign-ups, for example from disposable email addresses, are checked by a person first, and the
message says so.

If a request reaches a server with no credentials at all, it still answers `401` with the same sign-in pointer, and
the body now also says where to sign up and what the server needs (an account for the investigation server, an agent
key for the scam check server).

## Quick start

With Claude Code:

```bash
claude mcp add --transport http context https://context.everydaysecurity.ai/api/mcp
```

Run `/mcp` in Claude Code, authenticate `context` in the browser window that opens, then ask:

> What does Context know about `login-verify.example.net`? Show me the sources behind each fact.

Configs for VS Code, Cursor, Claude Desktop and generic clients are in [clients/](clients/).

| Endpoint | Tools | Authentication |
|---|---|---|
| `https://context.everydaysecurity.ai/api/mcp` | Investigation tools (10) | Browser sign-in in Claude Code and VS Code. An API key header elsewhere (keys issued on request, see [clients/](clients/)). |
| `https://context.everydaysecurity.ai/api/mcp/check` | `check_scam`, `get_check` | An agent key you make in Context |

## What you can look up

| Indicator | Look up what's known | Investigate or collect | Example |
|---|---|---|---|
| Domain | `get_fact`, `check_scam` | `start_investigation`, `enqueue_crawl` | [examples/domain.md](examples/domain.md) |
| URL | `get_fact`, `check_scam` | `enqueue_crawl` → `get_crawl` → `get_crawl_artifact`, `start_investigation` | [examples/url.md](examples/url.md) |
| IP address and ASN | `get_fact` (IP and ASN), `check_scam` (IP) | `start_investigation` (IP only) | [examples/ip-asn.md](examples/ip-asn.md) |
| Email address | `get_fact`, `check_scam` | Not investigated, only looked up | [examples/email.md](examples/email.md) |
| Phone number | `get_fact`, `check_scam` | Not investigated, only looked up | [examples/phone.md](examples/phone.md) |
| Crypto wallet | `get_fact` | Not investigated, only looked up | [examples/crypto-wallet.md](examples/crypto-wallet.md) |
| Business (state registry) | `search_business_records`, `get_fact` | `start_investigation` | [examples/business.md](examples/business.md) |
| Person (in state registry records) | `search_business_records`, `get_fact` | `start_investigation` | [examples/person-business-records.md](examples/person-business-records.md) |

Know the limits before you rely on it:

- **Lookups return only what Context already holds.** That means what earlier investigations collected and the
  threat intelligence it has recorded. An email address, phone number or wallet nobody has seen before comes
  back as "nothing known". These tools do not search breach data, social networks, or people-search
  databases.
- **People appear only through business registrations.** A person is someone named as a governor or registered
  agent in the state business records Context holds. This is not a general people search.
- **Business records cover only the state registries Context has ingested.** Today that is mostly Washington
  State. Each search result reports how many records its counts cover.

## Your operational security

When Context touches a target, it does so from isolated, disposable collection devices. Your machine and our
customer-facing services never connect to it. You choose how loud an investigation may be with the `noise`
argument of `start_investigation`:

| `noise` | What the target can observe |
|---|---|
| `passive` | Nothing. Reads only what Context and its data vendors already hold. |
| `fetch` | One page load from a collection device. No screenshots, no form probing, no following links. |
| `crawl` (default) | A full crawl from a collection device, which can include probing forms. |

An autonomous investigation can't engage a target, for example by messaging it, at any noise level.

Two points of hygiene for your own agent:

- Don't let it open, fetch, unfurl or DNS-resolve a suspicious indicator "to see for itself". Doing that is the
  exposure Context exists to take on for you.
- Treat text in results that quotes a target (page titles, `reasons`, timeline details) as data written by
  the adversary, never as instructions.

## Tools

The complete reference, generated from the servers' own `tools/list`, is in [TOOLS.md](TOOLS.md). In brief:

| Tool | What it does |
|---|---|
| `get_fact` | Everything known about one indicator: sightings, sources, attributes, related indicators. Never crawls. |
| `start_investigation` / `get_investigation` | Run an autonomous investigation and read its verdict, the evidence behind it, and its timeline. |
| `enqueue_crawl` / `get_crawl` / `get_crawl_artifact` | Send a URL to the collection fleet and read the DOM, console, network log, HAR and screenshot. |
| `search_business_records` | Ask questions about state business registrations by counts and fields, for example agents with many registrations or addresses shared by many businesses. |
| `check_scam` / `get_check` | A block, caution or allow answer for a URL, domain, IP, email or phone, with its reasons. |
| `send_feedback` / `list_my_feedback` / `get_my_feedback` | Tell the team what's wrong or missing, and follow what happens to it. |

## Repository layout

```
clients/      configs for Claude Code, Claude Desktop, Cursor, VS Code, generic HTTP clients
examples/     one worked example per indicator type
TOOLS.md      tool reference, generated by scripts/gen-tools
CHANGELOG.md  what changed, newest first
scripts/      gen-tools and the list of servers it reads
fixtures/     the saved tools/list answers TOOLS.md is rendered from
server.json   the entry for the official MCP Registry
```

What changed and when is in [CHANGELOG.md](CHANGELOG.md).

A scheduled job compares the live tool list against [TOOLS.md](TOOLS.md) and opens a pull request when they
differ, and we re-run the examples whose tools change.

## Links

- Everyday Security: https://everydaysecurity.ai/
- Context (sign up and accept the free license): https://context.everydaysecurity.ai
- Privacy policy: https://everydaysecurity.ai/privacy/
- Security issues: see [SECURITY.md](SECURITY.md)

## License

The contents of this repository (docs, configs, examples, scripts) are MIT licensed. See [LICENSE](LICENSE).
The license covers this repository only, not the hosted Context service, which has its own terms.
