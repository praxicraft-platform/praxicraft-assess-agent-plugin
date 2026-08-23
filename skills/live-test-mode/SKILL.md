---
name: live-test-mode
description: Keep Assess Live and Test modes isolated — keys, webhooks, MCP OAuth, and safe go-live checks. Use when debugging missing webhook deliveries or mixing ct_live_ and ct_test_ keys.
---

# Live and Test mode

## When to use

- Webhooks “not arriving”
- Debugging with fake candidate emails
- Choosing hosted MCP vs stdio MCP
- Preparing to go live

## Rules

1. **Keys set the mode.** `ct_live_…` → live; `ct_test_…` → test.
2. **Destinations are mode-scoped.** Live destinations only receive live events. Create a separate destination while debugging with a `ct_test_` key.
3. **Hosted MCP** at `https://assess.praxicraft.com/mcp` is always **live** (browser OAuth). For test-mode agent work use stdio:

```json
{
  "mcpServers": {
    "praxicraft-assess-api": {
      "command": "npx",
      "args": ["-y", "@praxicraft/assess-mcp"],
      "env": { "PRAXICRAFT_API_KEY": "ct_test_xxxxxxxxxxxxxxxx" }
    }
  }
}
```

4. **IP allowlists** on API keys do **not** apply to hosted MCP OAuth. They do apply to Public API and stdio MCP using that key.

## Go-live checklist

- [ ] Assessment published / active in live
- [ ] Live API key with least-privilege scopes
- [ ] Live webhook destination verified (`POST /webhooks/{id}/test/`)
- [ ] HMAC verified on HTTPS (or Slack/Teams connected)
- [ ] Test with your own email before cohort invites
- [ ] Remove or rotate any keys pasted into chats/logs

## Docs

- https://docs.praxicraft.com/authentication#live-and-test-mode
- https://docs.praxicraft.com/webhooks
- https://docs.praxicraft.com/assess-mcp
