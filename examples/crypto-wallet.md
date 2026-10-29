# Crypto wallet

> Illustrative output. The values are synthetic and the shapes match the real tools. The address below is a
> made-up placeholder, not a wallet known to belong to anyone.

**What this covers:** whether Context has seen a wallet address, usually because an investigation captured it on
a scam page (a fake exchange's deposit address, an "investment" site's payment page). Context recognises
Ethereum-style `0x…` addresses, Bitcoin base58 (`1…`, `3…`) and bech32 (`bc1…`) addresses, including
`bitcoin:` and `ethereum:` payment links. It does **not** trace transactions or query a blockchain, and a
wallet can't be the seed of an investigation.

## Prompt

> The fake exchange in our case told victims to deposit to `0x0123456789abcdef0123456789abcdef01234567`. Has
> that address shown up anywhere else?

## Tool calls

```json
{ "tool": "get_fact", "arguments": { "value": "0x0123456789abcdef0123456789abcdef01234567" } }
```

You can leave the type out. Context recognises the wallet format and keeps the address's case, which matters
for checksummed addresses.

## What the evidence looks like

```json
{
  "type": "wallet",
  "value": "0x0123456789abcdef0123456789abcdef01234567",
  "first_seen": "2026-07-30T10:02:44Z",
  "last_seen": "2026-10-11T19:45:09Z",
  "observation_count": 3,
  "is_primary_target": false,
  "tags": [],
  "sources": ["investigation"],
  "attrs": {},
  "occurrences": [
    {
      "type": "wallet",
      "value": "0x0123456789abcdef0123456789abcdef01234567",
      "investigation_id": "inv_03EXAMPLE",
      "investigation_title": "trade-desk.example.com",
      "seen_at": "2026-07-30T10:02:44Z"
    }
  ],
  "related": [
    { "type": "domain", "value": "trade-desk.example.com", "verified": false },
    { "type": "domain", "value": "trade-desk-pro.example.net", "verified": false }
  ]
}
```

## Reading it

The same deposit address appearing on two differently named sites is a lead, not proof of common control. The
`verified: false` relationships say exactly that: the address and the sites appeared together in
investigations. Open each investigation from its `investigation_id` and check the capture each sighting came
from.
