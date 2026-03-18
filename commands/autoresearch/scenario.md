---
name: autoresearch:scenario
description: Scenario-driven use case generator — explores situations, edge cases, and derivative scenarios from a seed scenario using autonomous iteration.
argument-hint: "[scenario description] [--scope <glob>] [--depth shallow|standard|deep] [--domain <type>] [--iterations N]"
---

EXECUTE IMMEDIATELY — do not ask clarifying questions before reading the protocol.

1. Read the scenario workflow: `.claude/skills/autoresearch/references/scenario-workflow.md` — this is the FULL protocol
2. Parse any flags from the user's arguments: $ARGUMENTS
3. If scenario or domain is missing — use `AskUserQuestion` with adaptive questions per scenario-workflow.md
4. Execute the 7-phase scenario loop as defined in `scenario-workflow.md`

Follow the protocol exactly. Every scenario requires concrete context (who, what, when, where). Stream all output live — never run this in background.
