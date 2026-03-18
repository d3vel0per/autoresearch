---
name: autoresearch:ship
description: Universal shipping workflow — ship code, content, marketing, sales, research, or anything through structured 8-phase workflow
argument-hint: "[--dry-run] [--auto] [--force] [--rollback] [--monitor N] [--type <type>] [--checklist-only]"
---

EXECUTE IMMEDIATELY — do not ask clarifying questions before reading the protocol.

1. Read the ship workflow: `.claude/skills/autoresearch/references/ship-workflow.md` — this is the FULL protocol
2. Parse any flags from the user's arguments: $ARGUMENTS
3. If ship type is unclear — use `AskUserQuestion` with batched questions per ship-workflow.md
4. Execute the 8-phase ship workflow as defined in `ship-workflow.md`

Follow the protocol exactly. All phases, checklists, and verification steps must be executed. Stream all output live — never run this in background.
