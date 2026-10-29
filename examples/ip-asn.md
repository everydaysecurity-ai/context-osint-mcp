# IP address and ASN

> Illustrative output. The values are synthetic and the shapes match the real tools.

## Prompt

> `203.0.113.45` hosted one of the phishing pages in our case. What else does Context know is on that address,
> and what network announces it? Is that network itself flagged?

## Tool calls

**1. The IP.**

```json
{ "tool": "get_fact", "arguments": { "value": "203.0.113.45" } }
```

**2. The ASN.** Pass it as `AS` plus the number. Context recognises the form and looks the network up.

```json
{ "tool": "get_fact", "arguments": { "value": "AS64500" } }
```

## What the evidence looks like

```json
{
  "type": "ip",
  "value": "203.0.113.45",
  "first_seen": "2026-09-02T08:11:40Z",
  "last_seen": "2026-10-19T22:03:15Z",
  "observation_count": 9,
  "is_primary_target": false,
  "tags": ["phishing-host"],
  "sources": ["investigation", "passive-dns"],
  "attrs": { "asn": "AS64500", "asn_org": "Example Hosting Ltd", "country": "ZZ" },
  "occurrences": [
    {
      "type": "ip",
      "value": "203.0.113.45",
      "investigation_id": "inv_01EXAMPLE",
      "investigation_title": "secure-account-check.example.net",
      "role": "hosting",
      "seen_at": "2026-10-19T22:03:15Z"
    }
  ],
  "related": [
    {
      "type": "domain",
      "value": "secure-account-check.example.net",
      "label": "resolves_to",
      "verified": true,
      "last_seen": "2026-10-19T22:03:15Z"
    },
    {
      "type": "asn",
      "value": "AS64500",
      "label": "announced_by",
      "verified": true,
      "outbound": true
    },
    {
      "type": "domain",
      "value": "parcel-redelivery.example.com",
      "verified": false
    }
  ],
  "related_verified_total": 23,
  "related_verified_shown": 6,
  "related_truncated": true
}
```

## Reading it

- **`verified: true`** means the relationship comes from an evidence-backed record, such as a resolution
  Context recorded. **`verified: false`** means the two only appeared together in one of your investigations.
  `label` names the relationship and `last_seen` is when it was last confirmed.
- **`related_truncated` with `related_verified_total: 23` and `related_verified_shown: 6`** means there are 23
  verified neighbours and you are seeing 6. A truncated list is never presented as the complete one. If the
  neighbour lookup failed, `related_lookup_failed` is set, which means the count is unknown, not zero.
- **For co-hosting,** a domain's profile can carry a `cohost` summary on its IP entries showing who else Context
  knows answers on that address. When `cohost` is absent, that wasn't checked. It doesn't mean nobody else is
  there.
- **The ASN profile** lists whatever Context holds on the network: its attributes, tags and sources, and the
  indicators related to it.

You can investigate an IP with `start_investigation`. An ASN can only be looked up.
