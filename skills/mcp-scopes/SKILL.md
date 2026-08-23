---
name: mcp-scopes
description: Connect Assess API MCP and docs Knowledge MCP safely — hosted OAuth vs stdio keys, scopes, and IP allowlists. Use when configuring Cursor, Claude, Codex, or debugging MCP auth.
---

# Assess MCP scopes and setup

## Two MCP servers

| Server | URL | Auth | Purpose |
|--------|-----|------|---------|
| **API** | `https://assess.praxicraft.com/mcp` | Browser OAuth (live) | Call Public API tools |
| **Knowledge** | `https://docs.praxicraft.com/mcp` | None | Search / read Assess docs |

Do not confuse them. Knowledge MCP cannot invite candidates. API MCP does not replace reading docs for payload shapes.

## Hosted API MCP (recommended for IDEs)

```json
{
  "mcpServers": {
    "praxicraft-assess-api": {
      "url": "https://assess.praxicraft.com/mcp"
    },
    "praxicraft-assess-knowledge": {
      "url": "https://docs.praxicraft.com/mcp"
    }
  }
}
```

- Owners, admins, and **developers** can consent.
- Hosted OAuth is always **live** mode.
- IP allowlists on API keys do **not** apply to hosted OAuth.

## Stdio API MCP (CI / test keys)

Package: `@praxicraft/assess-mcp` (npm) — https://github.com/praxicraft-platform/praxicraft-assess-mcp

```json
{
  "mcpServers": {
    "praxicraft-assess-api": {
      "command": "npx",
      "args": ["-y", "@praxicraft/assess-mcp"],
      "env": {
        "PRAXICRAFT_API_KEY": "ct_test_xxxxxxxxxxxxxxxx"
      }
    }
  }
}
```

Use a dedicated key with least-privilege scopes. If other keys use CIDR allowlists, leave this key’s allowlist empty (stdio clients have no stable IP).

## What agents can do (API)

Tools map to the Public API: assessments, invites, results, org/squads/audit (Starter+), cases, webhooks, interviews, pipelines, integrations.

Remind agents to respect plan limits and branch on `error.code`.

## Skills install (without full plugin)

```bash
npx skills add https://docs.praxicraft.com
# or, once published:
npx skills add praxicraft-platform/praxicraft-assess-agent-plugin
```

## Docs

- https://docs.praxicraft.com/assess-mcp
- https://docs.praxicraft.com/build-with-agents
- https://docs.praxicraft.com/using-llms
- https://docs.praxicraft.com/scopes
- https://docs.praxicraft.com/roles
