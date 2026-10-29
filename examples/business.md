# Business (state registry records)

> Illustrative output. The values are synthetic and the shapes match the real tools. "Example Harbor Holdings
> LLC" and every name and address here are fictional.

**What this covers:** the state business-registration records Context has ingested, which today are mostly
Washington State's. Each result reports how many records its counts cover. These are filings, not a verdict on
whether the business is legitimate.

## Prompt

> Which addresses in Washington have between 2 and 9 businesses registered at their own office? Then tell me
> everything Context knows about "Example Harbor Holdings LLC", which is registered at one of them.

## Tool calls

**1. Search by counts and fields.** `search_business_records` takes a question in plain words.

```json
{
  "tool": "search_business_records",
  "arguments": { "question": "addresses in WA with 2 to 9 businesses at their own office" }
}
```

**2. Look up one business**, using its label exactly as Context shows it.

```json
{ "tool": "get_fact", "arguments": { "value": "Example Harbor Holdings LLC", "type": "business" } }
```

**3. (Optional) Investigate it.** The investigation starts from the registry record and builds a graph you can
open in Context.

```json
{ "tool": "start_investigation", "arguments": { "value": "Example Harbor Holdings LLC", "type": "business" } }
```

## What the evidence looks like

The search first states how it read your question. Check that before you trust the rows.

```json
{
  "query": {
    "tool": "find_addresses",
    "args": { "state": "WA", "min_businesses": 2, "max_businesses": 9, "role": "business" },
    "summary": "Addresses in WA with 2 to 9 businesses (the businesses' own addresses, not their agents')",
    "parsed_by": "rules"
  },
  "result": {
    "rows": [
      { "label": "100 Example Way Ste 200, Seattle, WA", "businesses": 7 },
      { "label": "42 Sample St, Tacoma, WA", "businesses": 3 }
    ],
    "total": 2,
    "truncated": false,
    "corpus": 700000
  }
}
```

The `get_fact` profile for the business lists the registration attributes Context holds, and its `related`
entries are the registered relationships:

```json
{
  "type": "business",
  "value": "Example Harbor Holdings LLC",
  "sources": ["state-registry"],
  "attrs": { "jurisdiction": "WA", "status": "active", "formed": "2024-03-11" },
  "related": [
    { "type": "address", "value": "100 Example Way Ste 200, Seattle, WA", "label": "registered_at", "verified": true },
    { "type": "person", "value": "Jordan Sample", "label": "governs", "verified": true },
    { "type": "organization", "value": "Sample Registered Agent Services LLC", "label": "agent_for", "verified": true }
  ]
}
```

## Reading it

- **`query.summary` is the search Context actually ran.** If it misread your question, rephrase it. Don't rely on
  the rows.
- **`result.corpus` is the denominator.** "7 businesses at this address" means 7 among the records Context
  holds, not every business in the state.
- **Registry relationships are `verified`,** because they come from the filing itself. They show who filed what,
  not who controls the business in practice.
