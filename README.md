# Praxicraft Assess Agent Plugin

Official [Agent Plugins 1.0](https://agent-plugins.org/specification) bundle for **[Praxicraft Assess](https://assess.praxicraft.com)**.

One install wires **two MCP servers** and **eight hiring skills** into Claude Code, Codex CLI, Cursor, VS Code / GitHub Copilot, Kiro, OpenCode, and Gemini CLI (MCP only) — so agents can call the Assess API and follow correct invite, webhook, and ATS patterns.

```bash
claude plugins marketplace add praxicraft-platform/praxicraft-assess-agent-plugin
claude plugins install praxicraft-assess@praxicraft
```

Product docs: [Build with agents](https://docs.praxicraft.com/build-with-agents) · [MCP](https://docs.praxicraft.com/assess-mcp) · [Using docs with LLMs](https://docs.praxicraft.com/using-llms)

## Table of Contents

- [What you get](#what-you-get)
- [Install](#install)
  - [Claude Code](#claude-code)
  - [Codex CLI](#codex-cli)
  - [Cursor](#cursor)
  - [VS Code / GitHub Copilot](#vs-code--github-copilot)
  - [Gemini CLI](#gemini-cli-mcp-only)
  - [Skills only](#skills-only)
  - [MCP only (manual)](#mcp-only-manual)
- [Try this prompt](#try-this-prompt)
- [Authentication](#authentication)
- [Live vs Test mode](#live-vs-test-mode)
- [Requirements & support](#requirements--support)
- [License](#license)

---

## What you get

| Primitive | Purpose | Auth |
|-----------|---------|------|
| **`praxicraft-assess-api`** | Live Public API tools (invites, results, webhooks, pipelines, …) | Browser OAuth at `https://assess.praxicraft.com/mcp` |
| **`praxicraft-assess-knowledge`** | Search and read Assess documentation | None — `https://docs.praxicraft.com/mcp` |
| **Skills** | Procedural cheat sheets your agent loads on demand | — |

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

Knowledge MCP cannot invite candidates. API MCP does not replace reading docs for payload shapes — use both.

---

## Install

### Claude Code

```bash
claude plugins marketplace add praxicraft-platform/praxicraft-assess-agent-plugin
claude plugins install praxicraft-assess@praxicraft
```

The API MCP server uses browser OAuth by default — no keys required at install time. The first time your agent calls an Assess tool, you will be prompted to sign in.

### Codex CLI

```bash
codex plugin marketplace add praxicraft-platform/praxicraft-assess-agent-plugin
```

Then open Codex and run `/plugins` → **Praxicraft** marketplace → install **praxicraft-assess**.

Refresh if needed:

```bash
codex plugin marketplace upgrade praxicraft
```

### Cursor

```bash
git clone https://github.com/praxicraft-platform/praxicraft-assess-agent-plugin.git \
  ~/.cursor/plugins/local/praxicraft-assess-agent-plugin
```

Restart Cursor. Skills load from `skills/`; MCP servers from `.mcp.json`.

### VS Code / GitHub Copilot

Clone this repository, then in Chat → **Plugins** add the folder (or set `chat.pluginLocations`). Skills load from `skills/`; MCP from `.mcp.json`.

### Gemini CLI (MCP only)

Gemini has no agent-skill primitive — you get the two MCP servers only:

```bash
git clone https://github.com/praxicraft-platform/praxicraft-assess-agent-plugin.git \
  ~/.gemini/extensions/praxicraft-assess
```

Restart Gemini CLI.

### Skills only

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

Stdio API (Test mode / CI) via [`@praxicraft/assess-mcp`](https://github.com/praxicraft-platform/praxicraft-assess-mcp):

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

---

## Try this prompt

```
Invite a Greenhouse candidate to our active Assess assessment with external_id set to their Candidate ID, then add a webhook handler that verifies X-Praxicraft-Signature and posts a note on candidate.passed.
```

Your agent should load the invite / webhook skills, use Knowledge MCP for current field names, and call the API MCP (or write SDK code) accordingly.

---

## Authentication

- **Hosted API MCP** — browser OAuth; owners, admins, and [developers](https://docs.praxicraft.com/roles) can consent. Always **live** mode.
- **Knowledge MCP** — no credentials.
- **Stdio API MCP** — `PRAXICRAFT_API_KEY` from **Assess → Developer → API Keys**.

Never commit `ct_live_`, `ct_test_`, or `whsec_` secrets. Prefer OAuth in IDEs; use `ct_test_` keys while developing automations.

Scopes: [Authentication](https://docs.praxicraft.com/authentication) · [Scopes](https://docs.praxicraft.com/scopes)

---

## Live vs Test mode

| | Live | Test |
|--|------|------|
| Hosted OAuth | Yes (only mode) | Use stdio + `ct_test_` |
| Webhook destinations | Live only | Test only |

Details: [Live and Test mode](https://docs.praxicraft.com/authentication#live-and-test-mode)

---

## Requirements & support

- An Assess organisation with API / MCP access (see [plan limits](https://docs.praxicraft.com/plan-limits))
- Product docs: [docs.praxicraft.com](https://docs.praxicraft.com)
- API MCP package: [@praxicraft/assess-mcp](https://github.com/praxicraft-platform/praxicraft-assess-mcp)
- Issues: [GitHub Issues](https://github.com/praxicraft-platform/praxicraft-assess-agent-plugin/issues)
- Email: [support@praxicraft.com](mailto:support@praxicraft.com)

Review agent-generated webhook handlers for **raw-body** HMAC verification (`X-Praxicraft-Signature`).

---

## License

[MIT](LICENSE)
