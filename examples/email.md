# Email address

> Illustrative output. The values are synthetic and the shapes match the real tools.

**What this covers:** what Context already holds about an address. That includes where it turned up in
investigations (for example as a contact on a scam site) and any recorded threat intelligence. It is **not**
breach lookup, account enumeration or a people search. Context never sends mail to an address or probes it.

## Prompt

> An invoice came from `accounts@billing-notice.example.org`. Has Context seen that address, or its domain,
> in anything?

## Tool calls

```json
{ "tool": "get_fact", "arguments": { "value": "accounts@billing-notice.example.org" } }
```

Your assistant will often check the domain as well, which can be investigated:

```json
{ "tool": "get_fact", "arguments": { "value": "billing-notice.example.org" } }
```

## What the evidence looks like

```json
{
  "type": "email",
  "value": "accounts@billing-notice.example.org",
  "first_seen": "2026-08-14T17:20:05Z",
  "last_seen": "2026-08-14T17:20:05Z",
  "observation_count": 1,
  "is_primary_target": false,
  "tags": [],
  "sources": ["investigation"],
  "attrs": {},
  "occurrences": [
    {
      "type": "email",
      "value": "accounts@billing-notice.example.org",
      "investigation_id": "inv_02EXAMPLE",
      "investigation_title": "fake-invoice-portal.example.com",
      "role": "contact",
      "seen_at": "2026-08-14T17:20:05Z"
    }
  ],
  "related": [
    { "type": "domain", "value": "fake-invoice-portal.example.com", "label": "mentions", "verified": false }
  ]
}
```

## The quick yes/no version

On the scam check server, `check_scam` answers from the same recorded knowledge:

```json
{ "tool": "check_scam", "arguments": { "value": "accounts@billing-notice.example.org" } }
```

```json
{
  "check_id": "chk_01EXAMPLE",
  "indicator": { "type": "email", "value": "accounts@billing-notice.example.org" },
  "status": "complete",
  "action": "caution",
  "basis": "none",
  "deep_skipped": "not_investigable",
  "summary": "Caution: nothing known about this address.",
  "reasons": []
}
```

`deep_skipped: not_investigable` tells you no investigation was started. Email addresses and phone numbers are
answered only from what is already known. `caution` means unverified, not safe.

## Reading it

An occurrence tells you which investigation the address came from and the role it played there. Open that
investigation to see the page it was captured from. If nothing is known, you'll get "nothing is known" and not
an empty profile.
