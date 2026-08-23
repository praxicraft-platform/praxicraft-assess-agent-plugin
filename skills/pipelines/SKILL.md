---
name: pipelines
description: Design and enroll Assess multi-stage hiring pipelines via dashboard or Public API. Use for funnel enroll, advance, hold, reject, and pipeline webhook events.
---

# Assess pipelines

## When to use

- Multi-stage funnels (assessment → interview → decision)
- Enrolling from ATS / automation instead of a single invite
- Listening for `pipeline.*` webhook events

## Concepts

Pipelines are designed in the dashboard (stages, assessments, interviews). The Public API enrolls and moves candidates.

Typical events: `pipeline.enrolled`, `pipeline.advanced`, `pipeline.completed`, `pipeline.rejected`, `pipeline.held`, `pipeline.unheld`.

## Enroll

```bash
curl -sS -X POST \
  "https://assess.praxicraft.com/api/v1/public/pipelines/{slug}/enroll/" \
  -H "Authorization: Bearer $PRAXICRAFT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "candidate@example.com",
    "name": "Alex Candidate",
    "external_id": "ATS_ID",
    "send_email": true
  }'
```

Prefer `external_id` so later webhooks join back to the ATS.

## Scopes

`pipelines:read`, `pipelines:write`, plus `webhooks:write` if you register destinations.

## Long-tail ATS

If there is no native connector, enroll/invite from n8n/Make when the ATS stage changes, then write back on Assess webhooks — https://docs.praxicraft.com/integrations/via-webhooks

## Docs

- https://docs.praxicraft.com/pipelines
- https://docs.praxicraft.com/webhooks
- https://docs.praxicraft.com/automations
