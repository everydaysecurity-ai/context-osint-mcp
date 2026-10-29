# Domain

> Illustrative output. The values are synthetic and the shapes match the real tools.

## Prompt

> A customer got a text pointing at `secure-account-check.example.net`. What do we know about that domain? If
> nothing, investigate it, and tell me what the verdict rests on.

## Tool calls

**1. Check what's already known.** `get_fact` reads existing evidence and never touches the target.

```json
{ "tool": "get_fact", "arguments": { "value": "secure-account-check.example.net" } }
```

If nothing is known, the tool says so, and does not hand back an empty profile you could mistake for a clean
one:

```text
nothing is known about "secure-account-check.example.net" yet; enqueue_crawl can collect evidence for a url or domain
```

**2. Investigate.** This is the same as pressing "Autonomously investigate" in Context. It returns at once.

```json
{ "tool": "start_investigation", "arguments": { "value": "secure-account-check.example.net", "noise": "crawl" } }
```

**3. Poll until `done` is true.**

```json
{ "tool": "get_investigation", "arguments": { "investigation_id": "inv_01EXAMPLE" } }
```

## What the evidence looks like

```json
{
  "investigation_id": "inv_01EXAMPLE",
  "title": "secure-account-check.example.net",
  "path": "/investigations/inv_01EXAMPLE",
  "seed_type": "domain",
  "seed_value": "secure-account-check.example.net",
  "status": "complete",
  "done": true,
  "verdict": {
    "category": "scam",
    "confidence": "medium",
    "taxonomy_version": 2,
    "decided_at": "2026-10-20T14:05:11Z",
    "rests_on": [
      {
        "category": "scam",
        "strength": "strong",
        "source": "third_party",
        "node_type": "url",
        "node_label": "https://secure-account-check.example.net/verify",
        "what": "listed by a phishing blocklist",
        "observed_at": "2026-10-20T13:58:40Z"
      },
      {
        "category": "scam",
        "strength": "supporting",
        "source": "model",
        "node_type": "url",
        "node_label": "https://secure-account-check.example.net/verify",
        "what": "page asks for a card number to 'unlock' an account",
        "observed_at": "2026-10-20T14:01:02Z"
      }
    ]
  },
  "verdict_history": [],
  "nodes": 14,
  "edges": 17,
  "timeline": [
    { "time": "2026-10-20T13:58:02Z", "action": "crawl queued", "detail": "secure-account-check.example.net" },
    { "time": "2026-10-20T14:01:02Z", "action": "crawl complete", "detail": "1 page captured, screenshot and DOM saved" },
    { "time": "2026-10-20T14:05:11Z", "action": "verdict", "detail": "scam (medium)" }
  ]
}
```

## Reading it

- **The verdict is a conclusion, and it's marked as one.** It has a category from a fixed list (`malicious`,
  `fraud`, `scam`, `c2`, `compromised_legitimate`, `legitimate`, `inconclusive`) and a confidence band.
- **`rests_on` is the evidence.** Each item names its source class. `third_party` is a report from someone
  else, `observed` is Context's own capture, and `model` is a language model's reading of the captured page.
  Agreement between different classes counts for more than one class repeating itself. `node_label` is the node
  in the investigation graph you can open to check the item. `observed_at` is when the capture was taken.
- **`path` opens the same investigation in Context's web app**, where you can see the graph and every
  captured artifact.
- Without strong evidence the verdict would be `inconclusive`, never `legitimate`.

Run it again later and Context continues your existing investigation instead of opening a duplicate. Each new
verdict goes into `verdict_history`.
