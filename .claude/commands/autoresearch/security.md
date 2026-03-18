---
name: autoresearch:security
description: Autonomous security audit — STRIDE threat model + OWASP Top 10 + red-team with 4 adversarial personas
argument-hint: "[--diff] [--fix] [--fail-on <severity>]"
---

EXECUTE IMMEDIATELY — do not ask clarifying questions before reading the protocol.

1. Read the security workflow: `.claude/skills/autoresearch/references/security-workflow.md` — this is the FULL protocol
2. Parse any flags from the user's arguments: $ARGUMENTS
3. If scope is missing — use `AskUserQuestion` with batched questions per security-workflow.md
4. Execute the 7-step security audit as defined in `security-workflow.md`

Follow the protocol exactly. Every finding requires code evidence (file:line + attack scenario). Stream all output live — never run this in background.
