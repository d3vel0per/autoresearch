---
name: autoresearch:predict
description: Multi-persona swarm prediction — pre-analyze code from multiple expert perspectives using file-based knowledge representation. Zero external dependencies.
argument-hint: "[goal/focus] [--scope <glob>] [--chain debug|security|fix|ship|scenario (comma-separated for multi)] [--depth shallow|standard|deep] [--personas N] [--rounds N] [--adversarial] [--budget <dollars>] [--fail-on <severity>]"
---

EXECUTE IMMEDIATELY — do not ask clarifying questions before reading the protocol.

1. Read the predict workflow: `.claude/skills/autoresearch/references/predict-workflow.md` — this is the FULL protocol
2. Parse any flags from the user's arguments: $ARGUMENTS
3. If scope or goal is missing — use `AskUserQuestion` with batched questions per predict-workflow.md
4. Execute the 8-phase predict workflow as defined in `predict-workflow.md`

Follow the protocol exactly. Build file-based knowledge representation, generate expert personas, run structured debate with Devil's Advocate, produce consensus findings. Stream all output live — never run this in background.
