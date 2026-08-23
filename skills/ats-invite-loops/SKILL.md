---
name: ats-invite-loops
description: Automate ATS stage → Assess assessment invite → score-back note. Use for Greenhouse, Lever, Ashby, Workday, BambooHR, Zoho, Breezy take-home loops — not native AI interview connectors.
---

# ATS invite loops (take-home)

## Critical distinction

| Path | Creates |
|------|---------|
| **Native ATS connectors** (Growth+) | AI **interview room** + unstructured note |
| **Zapier / n8n / Make / Public API** | **Assessment invitation** or pipeline enroll |

Native connectors do **not** send take-home invites. This skill is the invite loop.

## Pattern

```text
ATS stage X → Invite Candidate (send_email=true, external_id=ATS id)
  → candidate completes
  → candidate.passed / failed
  → note or stage move in ATS using data.external_id
```

## Field mapping (Assess side)

| Assess | Value |
|--------|--------|
| Assessment | Active assessment slug / id |
| Email | Candidate email |
| Name | Full name |
| External ID | Provider candidate/opportunity id |
| Send email | `true` |

## Provider notes

- **Greenhouse** — Stage e.g. `Take-home`; External ID = Candidate ID; score-back via Harvest note
- **Lever** — Stage e.g. `Assessment`; External ID = Opportunity ID
- **Ashby** — Stage e.g. `Take home`; External ID = Candidate ID
- **Workday / BambooHR / Zoho / Breezy** — same invite pattern; stage filters differ on **native** interview webhooks only
- **Dover / Binary / TalentHR / Indeed** — no native connector; use https://docs.praxicraft.com/integrations/via-webhooks

## Prefer apps over raw HTTP

- Zapier: https://docs.praxicraft.com/zapier
- n8n: https://docs.praxicraft.com/n8n
- Make: https://docs.praxicraft.com/make

Examples repo: https://github.com/praxicraft-platform/praxicraft-assess-examples

## Docs

- https://docs.praxicraft.com/integrations
- https://docs.praxicraft.com/integrations/greenhouse
- https://docs.praxicraft.com/integrations/lever
- https://docs.praxicraft.com/integrations/ashby
