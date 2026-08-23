---
name: invitations-external-id
description: Invite Assess candidates with email, name, send_email, and external_id for ATS join keys. Use when creating invites or matching score-back webhooks to Greenhouse/Lever/Ashby records.
---

# Invitations and external_id

## When to use

- Sending take-home invites from code or automation
- Storing an ATS candidate/opportunity id for score-back
- Avoiding duplicate invites on retry

## Invite

```bash
curl -sS -X POST \
  "https://assess.praxicraft.com/api/v1/public/assessments/{slug}/invites/" \
  -H "Authorization: Bearer $PRAXICRAFT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "candidate@example.com",
    "name": "Alex Candidate",
    "send_email": true,
    "external_id": "ATS_CANDIDATE_OR_OPPORTUNITY_ID"
  }'
```

| Field | Purpose |
|-------|---------|
| `email` | Candidate work email |
| `name` | Display name |
| `send_email` | `true` to email the take-home link |
| `external_id` | ATS id (Greenhouse Candidate ID, Lever opportunity id, Ashby candidate id, …) |

Response includes **`invite_token`** — that is the invitation id for later GETs.

Retries with the same `external_id` return the existing invite (`200`) instead of creating a second one.

## Score-back join

On `candidate.passed` / `candidate.failed`, event `data` typically includes:

- `invite_token`
- `external_id`
- `candidate_email`
- `score`, `max_score`, `passed`

Match ATS notes / stage moves with `data.external_id`.

## Scopes

`assessments:read`, `invitations:write` (plus `candidates:read` if you poll results).

## Docs

- https://docs.praxicraft.com/invitations
- https://docs.praxicraft.com/results
- https://docs.praxicraft.com/integrations
