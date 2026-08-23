---
name: assess-best-practices
description: Assess Public API setup — auth keys, Live vs Test mode, scopes, error codes, and the canonical invite → webhook architecture. Use when starting any Assess integration.
---

# Praxicraft Assess — best practices

## When to use

- Creating API keys or choosing scopes
- Deciding Live vs Test mode
- Branching on API errors
- Choosing dashboard vs Public API vs MCP vs Zapier/n8n/Make

## Canonical facts

| Item | Value |
|------|--------|
| Base URL | `https://assess.praxicraft.com` |
| API prefix | `/api/v1/public/` |
| Auth | `Authorization: Bearer ct_live_…` or `ct_test_…` |
| Env | `PRAXICRAFT_API_KEY` |
| Webhook secret | `whsec_…` |
| Signature | `X-Praxicraft-Signature: sha256=<hex>` |
| Invite id field | `invite_token` |
| Docs | https://docs.praxicraft.com |
| OpenAPI | https://docs.praxicraft.com/openapi.json |

Public API responses are **flat JSON** (not `{ status, data }`). Errors are:

```json
{ "error": { "code": "SOME_CODE", "message": "…", "details": {} } }
```

Always branch on `error.code`.

## Live vs Test

| | Live (`ct_live_`) | Test (`ct_test_`) |
|--|-------------------|-------------------|
| Data | Production org | Isolated test data |
| Webhooks | Only live destinations | Only test destinations |
| Hosted MCP OAuth | Always **live** | Use stdio + `ct_test_` instead |

Never mix: a test `candidate.passed` never hits a live Slack/HTTPS destination.

## Scopes

Prefer least privilege. Common sets:

| Flow | Scopes |
|------|--------|
| Invite + notify | `assessments:read`, `invitations:write`, `candidates:read`, `webhooks:write` |
| Pipeline | `pipelines:read`, `pipelines:write`, `webhooks:write` |
| Read-only | `assessments:read`, `candidates:read`, `organisation:read` |

## Architecture (default)

1. Create assessment (dashboard or API).
2. Invite with `email`, `name`, `send_email`, optional `external_id` (ATS id).
3. Register HTTPS / Slack / Teams destination for `candidate.passed` / `failed` / `assessment.completed`.
4. Verify HMAC on HTTPS; Slack/Teams get chat cards (no HMAC).
5. On pass/fail, write a note or move stage in the ATS using `data.external_id`.

Native ATS connectors (Growth+) create **AI interview rooms**, not assessment invites. For take-homes use Zapier, n8n, Make, or the Public API.

## Do not

- Invent endpoints or fields not in OpenAPI
- Commit real `ct_live_` / `ct_test_` / `whsec_` values
- Expect native Greenhouse/Lever connectors to send take-home invites
- Verify signatures on a **parsed** JSON body — use the **raw** request bytes

## Docs to open

- https://docs.praxicraft.com/authentication
- https://docs.praxicraft.com/scopes
- https://docs.praxicraft.com/errors
- https://docs.praxicraft.com/plan-limits
