---
name: autoresearch
description: Autonomous Goal-directed Iteration. Modify, verify, keep/discard, repeat. Apply to ANY task with a measurable metric.
argument-hint: "[Goal: <text>] [Scope: <glob>] [Metric: <text>] [Verify: <cmd>]"
---

EXECUTE IMMEDIATELY — do not ask clarifying questions before reading the protocol files.

1. Read the autonomous loop protocol: `.claude/skills/autoresearch/references/autonomous-loop-protocol.md`
2. Read the results logging format: `.claude/skills/autoresearch/references/results-logging.md`
3. Parse any inline config from the user's arguments: $ARGUMENTS
4. If Goal, Scope, Metric, and Verify are all provided — proceed to setup step 5 in SKILL.md
5. If any critical field is missing — use `AskUserQuestion` with batched questions as defined in SKILL.md "Interactive Setup" section
6. Execute the autonomous loop: Modify → Verify → Keep/Discard → Repeat

IMPORTANT: Start executing immediately. Stream all output live — never run this in background.
