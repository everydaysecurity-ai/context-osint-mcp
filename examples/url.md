# URL

> Illustrative output. The values are synthetic and the shapes match the real tools.

## Prompt

> Crawl `https://parcel-redelivery.example.com/fee?id=8812` and show me what the page actually asks for. Pull
> the screenshot and the network log. Don't visit it yourself.

## Tool calls

**1. Send the URL to the collection fleet.** A disposable device visits the page. Neither your machine nor
Context's own servers connect to it.

```json
{
  "tool": "enqueue_crawl",
  "arguments": {
    "url": "https://parcel-redelivery.example.com/fee?id=8812",
    "reason": "smishing lure reported by a customer"
  }
}
```

```json
{ "job_id": "job_01EXAMPLE", "deduped": false }
```

`deduped: true` means the same URL was already queued and you were attached to that job instead.

**2. Read the job.** Poll until the status is final.

```json
{ "tool": "get_crawl", "arguments": { "job_id": "job_01EXAMPLE" } }
```

```json
{
  "job_id": "job_01EXAMPLE",
  "status": "completed",
  "seed_url": "https://parcel-redelivery.example.com/fee?id=8812",
  "risk": { "level": "high" },
  "analysis": { "title": "Redelivery fee", "forms": 1, "requests_card_data": true },
  "artifacts": [
    { "evidence_type": "screenshot", "key": "…" },
    { "evidence_type": "dom", "key": "…" },
    { "evidence_type": "network", "key": "…" },
    { "evidence_type": "har", "key": "…" },
    { "evidence_type": "console", "key": "…" }
  ]
}
```

The artifact list is an index only: the evidence type and a key. It contains no download links.

**3. Read the evidence.**

```json
{ "tool": "get_crawl_artifact", "arguments": { "job_id": "job_01EXAMPLE", "evidence_type": "screenshot" } }
{ "tool": "get_crawl_artifact", "arguments": { "job_id": "job_01EXAMPLE", "evidence_type": "network" } }
```

## What the evidence looks like

- **`screenshot`** comes back as an embedded image your client can display. Above 2 MB you get its type and size
  instead.
- **`dom`, `network`, `console` and `cdp-status`** come back as text, truncated with a note above 256 KB. For
  example, the network log shows the form posting to `https://collect.example.org/api/submit`, a second host you
  can pivot on with `get_fact` or a new crawl.
- **`har`** is the full HTTP archive, embedded the same way as the screenshot.

The server reads every artifact on its side. No tool hands your client a fetchable link to the evidence store.

## Reading it

`risk` and `analysis` are the crawler's own assessment of the page. The artifacts are the raw captures that
assessment was made from, so you can check it yourself. For a verdict that weighs this capture against
third-party reports, run `start_investigation` on the URL (see [domain.md](domain.md)).
