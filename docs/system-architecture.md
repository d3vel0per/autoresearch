# System Architecture

## Overview

Autoresearch v2.1.0 is a modular, markdown-driven autonomous iteration framework. The core architectural shift from v2.0.x is the **thin SKILL.md + self-contained command files** pattern: the skill file is a 41-line routing table; all protocol is embedded in 12 self-contained command files. Only the invoked command file loads per invocation, reducing token cost by ~95%.

Multi-platform: Claude Code, OpenCode, and Codex are all supported via a single `scripts/transform.sh` that produces platform-specific distributions from the canonical `.claude/` source.

## Component Diagram

```mermaid
graph TB
    subgraph "Claude Code Runtime"
        CC[Claude Code CLI]
        PS[Plugin System]
    end

    subgraph "Other Platforms"
        OC[OpenCode]
        CX[Codex]
    end

    subgraph "Transform Layer"
        TX[scripts/transform.sh]
    end

    subgraph "Canonical Source"
        SKILL[.claude/skills/autoresearch/SKILL.md\nthin routing table — 41 lines]
        CMD[.claude/commands/autoresearch.md]
        CMDS[.claude/commands/autoresearch/*.md\n12 self-contained command files]
        REF[.claude/skills/autoresearch/references/\n3 focused reference files]
    end

    subgraph "Platform Distributions"
        OCD[.opencode/commands/ + .opencode/skills/]
        CXD[plugins/autoresearch/ + .agents/skills/]
    end

    CC --> PS --> SKILL
    CC --> CMD & CMDS
    SKILL -.routing only.-> CMDS
    CMDS --> REF
    TX --> OCD
    TX --> CXD
    OC --> OCD
    CX --> CXD
```

## Data Flow — Core Autoresearch Loop

```mermaid
flowchart TD
    A[User invokes /autoresearch] --> B{Config complete?}
    B -- No --> C[AskUserQuestion batched setup]
    C --> D[Establish Baseline — Iteration 0]
    B -- Yes --> D
    D --> E[Write TSV header + metric_direction comment]
    E --> F[Read git log + last TSV rows as memory]
    F --> G[Make ONE focused change]
    G --> H[git commit — experiment: description]
    H --> I[Run Verify command → extract number]
    I --> J{Metric improved?}
    J -- Yes --> K{Guard passes?}
    K -- Yes --> L[keep — commit stays]
    K -- No --> M[rework up to 2x]
    M -- Still fails --> N[discard — git revert]
    J -- No --> N
    I -- Crash --> O[fix up to 3x]
    O -- Fixed --> I
    O -- Unfixable --> N
    N --> P[Log row to TSV]
    L --> P
    P --> Q{Eval checkpoint?}
    Q -- Yes --> R[Print 5-line checkpoint]
    Q -- No --> S{More iterations?}
    R --> S
    S -- Yes --> F
    S -- No --> T[Print summary + write handoff.json]
    T --> U{--chain?}
    U -- Yes --> V[Invoke next command]
    U -- No --> W[Done]
```

## Directory Structure

```
.claude/
├── commands/
│   ├── autoresearch.md                    # Core loop command — self-contained, 110 lines
│   └── autoresearch/
│       ├── debug.md                       # Hypothesis iteration loop
│       ├── evals.md                       # One-shot TSV analysis (NEW in v2.1.0)
│       ├── fix.md                         # Error-count reduction loop
│       ├── learn.md                       # Doc generation loop
│       ├── plan.md                        # Goal-to-config wizard
│       ├── predict.md                     # 5-persona one-shot debate
│       ├── probe.md                       # Requirement interrogation loop
│       ├── reason.md                      # Adversarial refinement loop
│       ├── scenario.md                    # 12-dimension edge case loop
│       ├── security.md                    # STRIDE + OWASP loop
│       └── ship.md                        # 8-phase ship pipeline
└── skills/autoresearch/
    ├── SKILL.md                           # Routing table only — 41 lines
    └── references/
        ├── predict-personas.md            # 5 default expert personas
        ├── reason-judge-protocol.md       # Blind judge scoring protocol
        └── security-checklist.md          # STRIDE + OWASP checklist

.opencode/                                 # OpenCode distribution (underscore naming)
plugins/autoresearch/                      # Codex distribution
.agents/skills/autoresearch/              # Codex agents distribution
scripts/
├── transform.sh                          # Single multi-platform transform script
└── install.sh                            # Guided installer

claude-plugin/
└── .claude-plugin/plugin.json            # Claude Code metadata — v2.1.0
plugins/autoresearch/
└── .codex-plugin/plugin.json             # Codex metadata — v2.1.0-codex.0
```

## Key Architectural Decisions

| Decision | Rationale |
|----------|-----------|
| Thin SKILL.md routing table (41 lines) | ~95% token reduction vs monolith v2.0.x SKILL.md (813 lines) |
| Self-contained command files | Each file embeds full protocol — no reference file loading unless needed |
| 3 focused reference files (not 13) | Only truly shared content warrants a reference: personas, judge protocol, security checklist |
| No autoresearch-command-spec.json | JSON spec removed; command contracts live in individual command files |
| scripts/transform.sh replaces sync-opencode.sh + sync-codex.sh | Single script generates all platform distributions |
| TSV with `# metric_direction` comment | Enables evals command to auto-detect direction without user prompt |
| 8 TSV status values | baseline, keep, discard, crash, no-op, hook-blocked, metric-error, keep (reworked) |
| handoff.json for chain integration | Structured handoff between subcommands; evals reads `*-results.tsv` directly |

## Integration Points

- **Claude Code Plugin System** — commands in `.claude/commands/`, skill in `.claude/skills/`
- **OpenCode** — `.opencode/commands/` + `.opencode/skills/` (underscore naming convention)
- **Codex** — `plugins/autoresearch/` + `.agents/skills/autoresearch/`
- **Git** — memory, rollback, staleness detection, changelog generation
- **Shell** — verify and guard commands are user-defined shell expressions
- **MCP servers** — any MCP server configured in the host environment is available during loops

See also: [Project Overview](project-overview-pdr.md) | [Codebase Summary](codebase-summary.md) | [Code Standards](code-standards.md)
