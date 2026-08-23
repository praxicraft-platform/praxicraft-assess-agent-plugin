---
name: sdk-webhooks-verify
description: Verify Assess webhook signatures with official SDKs (Python, Node, Go, PHP, Ruby, Java, .NET) and the CLI. Use when implementing or fixing HMAC verification in application code.
---

# SDK webhook verification

## When to use

- Adding signature checks in a backend
- Fixing false 401s after framework body parsing
- Preferring maintained helpers over hand-rolled HMAC

## Rules

1. Pass the **raw** request body bytes to the verifier.
2. Use header `X-Praxicraft-Signature` (`sha256=<hex>`).
3. Secret is `whsec_…` from create-webhook response (shown once).
4. Slack/Teams chat destinations do **not** use HMAC.

## Clients

| Language | Package | Docs |
|----------|---------|------|
| Python | `praxicraft` | https://docs.praxicraft.com/sdks/python |
| Node | `@praxicraft/assess` | https://docs.praxicraft.com/sdks/node |
| Go | module docs | https://docs.praxicraft.com/sdks/go |
| PHP | `praxicraft/assess` | https://docs.praxicraft.com/sdks/php |
| Ruby | `praxicraft` gem | https://docs.praxicraft.com/sdks/ruby |
| Java | Maven/Gradle | https://docs.praxicraft.com/sdks/java |
| .NET | NuGet | https://docs.praxicraft.com/sdks/dotnet |
| CLI | `praxicraft-assess` | https://docs.praxicraft.com/sdks/cli |

Shared verify guide: https://docs.praxicraft.com/sdks/webhooks

Install CLI:

```bash
curl -fsSL https://praxicraft.com/install.sh | sh
```

## Env

```bash
export PRAXICRAFT_API_KEY=ct_test_xxxxxxxxxxxxxxxx
# optional
export PRAXICRAFT_API_BASE_URL=https://assess.praxicraft.com
```

## Docs

- https://docs.praxicraft.com/sdks/introduction
- https://docs.praxicraft.com/sdks/webhooks
- https://docs.praxicraft.com/webhooks
