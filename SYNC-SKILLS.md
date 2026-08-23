# Syncing skills with Mintlify

This repo is the source of truth for Agent Plugin skills (`skills/*/SKILL.md`).

The Assess docs site also hosts them via Mintlify at:

`Gamified-Application/assess-docs/.mintlify/skills/`

After editing skills here, copy them into the monorepo before deploying docs:

```bash
rsync -a --delete \
  ~/Desktop/praxicraft-assess-agent-plugin/skills/ \
  ~/Desktop/Gamified-Application/assess-docs/.mintlify/skills/
```

Then redeploy Mintlify so `/.well-known/agent-skills/` and the Knowledge MCP pick up changes.
