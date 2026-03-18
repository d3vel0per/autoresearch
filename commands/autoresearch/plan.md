---
name: autoresearch:plan
description: Interactive wizard to build Scope, Metric, Direction & Verify from a Goal
argument-hint: "[goal description]"
---

EXECUTE IMMEDIATELY — do not ask clarifying questions before reading the protocol.

1. Read the plan workflow: `.claude/skills/autoresearch/references/plan-workflow.md` — this is the FULL protocol
2. Accept the user's goal from arguments: $ARGUMENTS
3. Execute the 7-step planning wizard as defined in `plan-workflow.md`

Follow the protocol exactly. All gates (mechanical metric, dry-run, scope resolution) must pass before accepting. Stream all output live — never run this in background.
