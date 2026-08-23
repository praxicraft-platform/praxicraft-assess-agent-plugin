# Praxicraft Assess Agent Plugin

Official [Agent Plugins 1.0](https://agent-plugins.org/specification) bundle for Praxicraft Assess.

Installs **eight integration skills** and **two MCP servers** for Claude Code, Codex CLI, Cursor, VS Code / GitHub Copilot, Kiro, Gemini CLI (MCP only), and other MCP-compatible clients.

## What you get

| Primitive | Purpose | Auth |
|-----------|---------|------|
| **`praxicraft-assess-api`** | Live Public API tools (invites, results, webhooks, pipelines, …) | Browser OAuth at `https://assess.praxicraft.com/mcp` |
| **`praxicraft-assess-knowledge`** | Search / read Assess docs | None — `https://docs.praxicraft.com/mcp` |
| **Skills** | Procedural cheat sheets agents load on demand | — |

### Skills

| Skill | Use when |
|-------|----------|
| `assess-best-practices` | Starting any integration (auth, scopes, errors) |
| `live-test-mode` | Isolating `ct_live_` / `ct_test_` and go-live |
| `webhook-hmac` | HTTPS destinations + `X-Praxicraft-Signature` |
| `invitations-external-id` | Invites + ATS join keys |
| `ats-invite-loops` | Stage → invite → score-back (not native interview connectors) |
| `pipelines` | Multi-stage enroll / advance / hold |
| `sdk-webhooks-verify` | Official SDK verify helpers |
| `mcp-scopes` | Hosted vs stdio MCP, IP allowlists |

## Install

### Claude Code

```bash
claude plugins marketplace add praxicraft-platform/praxicraft-assess-agent-plugin
claude plugins install praxicraft-assess@praxicraft
```

### Codex CLI

```bash
codex plugin marketplace add praxicraft-platform/praxicraft-assess-agent-plugin
```

Then in the TUI: `/plugins` → install **praxicraft-assess**. Or point the marketplace at this local directory.

### Cursor

```bash
git clone https://github.com/praxicraft-platform/praxicraft-assess-agent-plugin.git \
  ~/.cursor/plugins/local/praxicraft-assess-agent-plugin
```

Or symlink this Desktop folder:

```bash
ln -s ~/Desktop/praxicraft-assess-agent-plugin ~/.cursor/plugins/local/praxicraft-assess-agent-plugin
```

Restart Cursor. Skills load from `skills/`; MCP from `.mcp.json` (via `.cursor-plugin/plugin.json`).

### Gemini CLI (MCP only)

```bash
git clone https://github.com/praxicraft-platform/praxicraft-assess-agent-plugin.git \
  ~/.gemini/extensions/praxicraft-assess
```

### Skills only (no plugin)

```bash
npx skills add https://docs.praxicraft.com
npx skills add praxicraft-platform/praxicraft-assess-agent-plugin
```

### MCP only (manual)

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

Stdio API (test keys / CI):

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

## Try this prompt

```
Invite a Greenhouse candidate to our active Assess assessment with external_id set to their Candidate ID, then add a webhook handler that verifies X-Praxicraft-Signature and posts a note on candidate.passed.
```

## Layout

```
plugin.json          # Agent Plugins 1.0 manifest
mcp.json             # Native streamable-http MCP (Codex, Kiro, …)
.mcp.json            # mcp-remote bridge (Cursor / Claude Code compat)
skills/*/SKILL.md
.claude-plugin/      # Claude marketplace + plugin
.cursor-plugin/      # Cursor skills + MCP paths
.codex-plugin/
gemini-extension.json
```

## Related packages

| Package | Role |
|---------|------|
| [`@praxicraft/assess-mcp`](https://github.com/praxicraft-platform/praxicraft-assess-mcp) | Stdio + hosted Assess **API** MCP (npm; Harbor image still via web monorepo deploy) |
| Docs site | Knowledge MCP + Mintlify skills at https://docs.praxicraft.com |
| Docs guide | https://docs.praxicraft.com/build-with-agents |

## Security

- Prefer hosted OAuth over pasting live keys into agent config.
- Use `ct_test_` keys while developing.
- Never commit `ct_live_`, `ct_test_`, or `whsec_` secrets.
- Review agent-generated webhook handlers for raw-body HMAC verification.

## License

MIT
