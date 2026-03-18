---
name: autoresearch:fix
description: Autonomous fix loop — iteratively repairs errors until zero remain. One fix per iteration, atomic, auto-reverted on failure.
argument-hint: "[--target <cmd>] [--guard <cmd>] [--scope <glob>] [--category <type>] [--from-debug]"
---

EXECUTE IMMEDIATELY — do not ask clarifying questions before reading the protocol.

1. Read the fix workflow: `.claude/skills/autoresearch/references/fix-workflow.md` — this is the FULL protocol
2. Parse any flags from the user's arguments: $ARGUMENTS
3. If target and scope are missing — use `AskUserQuestion` with batched questions per fix-workflow.md
4. Execute the 8-phase fix loop as defined in `fix-workflow.md`

Follow the protocol exactly. ONE fix per iteration. Never suppress errors. Auto-revert on regression. Stream all output live — never run this in background.
