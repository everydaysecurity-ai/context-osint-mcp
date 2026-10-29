# Person, through business records

> Illustrative output. The values are synthetic and the shapes match the real tools. "Jordan Sample" and every
> business named here are fictional.

**What this covers:** people as they appear in the state business-registration records Context holds, which
means governors (officers, members, managers) and registered agents named in filings. It is **not** a people
search. It returns no contact details, social profiles, addresses of residence, or anything from outside those
filings.

## Prompt

> Find people in the Washington records who govern 3 or more businesses formed in the last year. Then show me
> everything filed under one of them.

## Tool calls

**1. Search by counts.**

```json
{
  "tool": "search_business_records",
  "arguments": { "question": "people who govern 3 or more businesses formed since October 2025" }
}
```

**2. Look up the person**, using the label exactly as Context shows it.

```json
{ "tool": "get_fact", "arguments": { "value": "Jordan Sample", "type": "person" } }
```

## What the evidence looks like

```json
{
  "query": {
    "tool": "find_people",
    "args": { "kind": "person", "role": "governor", "min_businesses": 3, "formed_after": "2025-10-01" },
    "summary": "People governing 3 or more businesses formed after 2025-10-01",
    "parsed_by": "rules"
  },
  "result": {
    "rows": [
      { "label": "Jordan Sample", "businesses": 5 },
      { "label": "Casey Placeholder", "businesses": 3 }
    ],
    "total": 2,
    "truncated": false,
    "corpus": 700000
  }
}
```

```json
{
  "type": "person",
  "value": "Jordan Sample",
  "sources": ["state-registry"],
  "related": [
    { "type": "business", "value": "Example Harbor Holdings LLC", "label": "governs", "verified": true, "outbound": true },
    { "type": "business", "value": "Sample Logistics LLC", "label": "governs", "verified": true, "outbound": true },
    { "type": "business", "value": "Placeholder Ventures LLC", "label": "governs", "verified": true, "outbound": true }
  ],
  "related_verified_total": 5,
  "related_verified_shown": 5
}
```

## Reading it

- **A name is not an identity.** Registry records match on the name as filed, so two different people with the
  same name can be merged, and one person can appear under spellings that don't merge. Confirm with the
  filings before you draw a conclusion about an individual.
- **Counts are within the records Context holds** (`result.corpus`), not the whole state.
- **Use it responsibly.** Many people govern several small businesses for ordinary reasons. A high count is a
  reason to look closer, not a finding.
