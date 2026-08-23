---
name: webhook-hmac
description: Create Assess webhook destinations and verify X-Praxicraft-Signature with whsec_ on the raw body. Use for HTTPS listeners, retries, and delivery debugging — not Slack/Teams chat destinations.
---

# Assess webhooks and HMAC

## When to use

- Registering an HTTPS catch-hook (n8n, Make, custom backend)
- Implementing signature verification
- Debugging failed deliveries / 401s
- Retrying a failed delivery

## Create

Body uses **`events`** (not `event_types`):

```bash
curl -sS -X POST https://assess.praxicraft.com/api/v1/public/webhooks/create/ \
  -H "Authorization: Bearer $PRAXICRAFT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com/hooks/praxicraft",
    "events": ["assessment.completed", "candidate.passed", "candidate.failed"]
  }'
```

Store `secret_key` (`whsec_…`) once. Then:

```bash
curl -sS -X POST \
  "https://assess.praxicraft.com/api/v1/public/webhooks/WEBHOOK_ID/test/" \
  -H "Authorization: Bearer $PRAXICRAFT_API_KEY"
```

HTTP 2xx on the test ping sets `is_verified` / `is_active`.

## Signature

```http
X-Praxicraft-Signature: sha256=<hex>
```

HMAC-SHA256 of the **raw** body with `whsec_…`. Prefer SDK helpers:

- Docs: https://docs.praxicraft.com/sdks/webhooks
- Python / Node / Go / PHP / Ruby / Java / .NET clients expose verify helpers

Pseudo-check:

```text
expected = "sha256=" + hex(hmac_sha256(whsec, raw_body))
constant_time_compare(expected, header_value)
```

## Delivery behaviour

- Retries on 5xx/timeout: up to **3** (~1m, 2m, 4m)
- **4xx** = no retry
- Deduplicate with delivery `id` / `event_id`
- Slack / Teams destinations post chat cards — **no HMAC**

## Debug

| Symptom | Fix |
|---------|-----|
| 401 / bad signature | Verify **raw** body; wrong `whsec_`; body mutated by middleware |
| Timeout | Public HTTPS, TLS, firewall |
| Auto-paused | Fix URL, send test ping |
| No events | Wrong Live/Test destination; wrong `events` list |

`GET /webhooks/{id}/deliveries/` includes payload, last_error, response_status_code.

Retry:

```bash
curl -sS -X POST \
  "https://assess.praxicraft.com/api/v1/public/webhooks/WEBHOOK_ID/deliveries/DELIVERY_ID/retry/" \
  -H "Authorization: Bearer $PRAXICRAFT_API_KEY"
```

## Event names (subset)

`assessment.started`, `assessment.completed`, `candidate.passed`, `candidate.failed`, `candidate.violation`, `invitation.created`, `invitation.expired`, `pipeline.enrolled`, `pipeline.advanced`, `pipeline.completed`, `pipeline.rejected`, `pipeline.held`, `pipeline.unheld`, `interview.*`, `webhook.test`.

## Docs

- https://docs.praxicraft.com/webhooks
- https://docs.praxicraft.com/sdks/webhooks
- https://docs.praxicraft.com/integrations/slack
