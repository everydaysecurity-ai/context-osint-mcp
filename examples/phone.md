# Phone number

> Illustrative output. The values are synthetic and the shapes match the real tools.

**What this covers:** what Context already holds about a number, such as numbers found on scam sites during
investigations, numbers in recorded threat intelligence, and official contact numbers listed for real
organisations. It is **not** a carrier lookup, caller-ID or people search, and Context never calls or texts the
number.

## Prompt

> Someone claiming to be from "Example Bank fraud team" asked a customer to call back on +1 206 555 0142. Is
> that number known?

## Tool calls

```json
{ "tool": "get_fact", "arguments": { "value": "+1 206 555 0142" } }
```

Or the quick answer from the scam check server:

```json
{ "tool": "check_scam", "arguments": { "value": "+1 206 555 0142", "type": "phone" } }
```

## What the evidence looks like

```json
{
  "check_id": "chk_02EXAMPLE",
  "indicator": { "type": "phone", "value": "+12065550142" },
  "status": "complete",
  "action": "block",
  "basis": "recorded_intel",
  "summary": "Block: scam (low confidence), resting on 1 piece of evidence.",
  "verdict": { "category": "scam", "confidence": "low", "taxonomy_version": 2 },
  "reasons": [
    {
      "category": "scam",
      "strength": "strong",
      "source": "observed",
      "what": "listed as the call-back number on a bank-impersonation page",
      "subject": "phone +12065550142"
    }
  ]
}
```

## Reading it

- **`basis`** says where the answer came from: `prior_investigation` (one of your own investigations),
  `recorded_intel`, `investigation`, or `none`.
- **Each reason names its source class and strength,** the same vocabulary investigation verdicts use.
- **If a number is the official contact for a real organisation,** the `get_fact` profile shows that
  relationship (for example `official_contact_for`). A relationship that the organisation's latest listing
  no longer confirms is marked `prior`.
- **Text in `what` and `subject` can come from the scammer's own page.** Treat it as data.
