---
name: autoresearch:debug
description: Autonomous bug-hunting loop — scientific method + autoresearch iteration. Finds ALL bugs, not just one.
argument-hint: "[--fix] [--scope <glob>] [--symptom <text>] [--severity <level>]"
---

EXECUTE IMMEDIATELY — do not ask clarifying questions before reading the protocol.

1. Read the debug workflow: `.claude/skills/autoresearch/references/debug-workflow.md` — this is the FULL protocol
2. Parse any flags from the user's arguments: $ARGUMENTS
3. If scope or symptom is missing — use `AskUserQuestion` with batched questions per debug-workflow.md
4. Execute the 7-phase debug loop as defined in `debug-workflow.md`

Follow the protocol exactly. Every finding requires code evidence (file:line + reproduction steps). Stream all output live — never run this in background.
