# Project Changelog

All notable changes to the autoresearch project are documented here.

## v2.1.0 — 2026-05-22

### Summary
Modular rebuild. Thin SKILL.md routing table replaces the 813-line monolith. Twelve self-contained command files replace the old minimal-registration + 13-reference-file pattern. Net result: ~95% token reduction per invocation (~5–8K tokens vs ~100K). New `/autoresearch:evals` subcommand added.

### Added
- `/autoresearch:evals` — one-shot analysis of any `*-results.tsv`: trends, plateaus, regressions, file hotspots, technique effectiveness, recommendations
- `--evals` flag on all looping commands — adaptive mid-loop checkpoints + final evals-summary.md
- `--evals-interval N` — override checkpoint frequency
- `# metric_direction: higher_is_better|lower_is_better` comment on TSV line 1 — enables evals auto-detection
- 3 new TSV status values: `keep (reworked)`, `hook-blocked`, `metric-error` (total: 8)
- `scripts/transform.sh` — single script generates OpenCode and Codex distributions from `.claude/` source
- `handoff.json` written by all subcommands for chain integration; `evals` reads `*-results.tsv` directly

### Changed
- `SKILL.md` reduced from 813 lines to 41 lines — routing table only, no protocol
- All 12 command files are now self-contained (94–120 lines each) — full protocol embedded, no reference file loading required for standard use
- Reference files reduced from 13 to 3: `predict-personas.md`, `reason-judge-protocol.md`, `security-checklist.md`
- Subcommand count: 11 → 12 (added evals)

### Removed
- `plugins/autoresearch/resources/autoresearch-command-spec.json` — command contracts now live in individual command files
- `scripts/sync-opencode.sh` and `scripts/sync-codex.sh` — replaced by `scripts/transform.sh`
- `plugins/autoresearch/scripts/autoresearch_cli.py` — Python wrapper CLI no longer needed
- 10 per-command workflow reference files (plan, debug, fix, security, ship, scenario, predict, learn, reason, probe workflows)

### Technical Details
- Per-invocation token cost: ~5–8K (down from ~100K in v2.0.x)
- All platform distributions generated from `.claude/` canonical source
- Codex plugin version: `2.1.0-codex.0`
- Backward compat: evals reads v2.0.03 TSV files (handles missing `timestamp` column)

## v2.0.0 — 2026-04-28

### Summary
Multi-platform GA release. Claude Code, OpenCode, and Codex all fully supported with strict YAML compliance, security-hardened scripts, and complete command metadata.

### Breaking
- Version jump from 1.10.0 to 2.0.0 — reflects multi-platform support, 11 subcommands, and maturity since v1.x

### Fixed
- SKILL.md YAML frontmatter uses folded block scalars — strict parsers (Codex CLI, PyYAML) no longer reject the description field (#69, #71)
- `scripts/sync-codex.sh` and `scripts/sync-opencode.sh` pass paths via `sys.argv` instead of shell string interpolation — eliminates script corruption when paths contain quotes (#77)
- `scripts/install.sh` `sync_dir()` validates destination path depth before `rm -rf` — rejects empty, root, or shallow paths (#78)

### Added
- `name` field to `.opencode/agents/docs-manager.md` for explicit agent registration (#74)
- `name` field to all 11 `.opencode/commands/*.md` files for schema compliance (#75)
- `allowed-tools` declaration to all Claude Code and claude-plugin command files (#76)

### Changed
- All version references unified to 2.0.0 across SKILL.md ×4, plugin.json, marketplace.json, README badge

### Contributors
- @xiaolai — NLPM audit (#79): 3 bugs + 2 security findings with PRs #74–#78
- @georgelichen — initial YAML frontmatter issue report (#69)
- @ch0udry — Codex loading error report (#71)
- @rexplx — YAML colon escape fix (#80)
- @haosenwang1018 — comprehensive YAML + sync script fix (#81)

## v1.10.0 — 2026-04-16

### Added
- `/autoresearch:probe` — adversarial multi-persona requirement interrogation engine
- probe-workflow.md reference (449 lines) — 10-phase protocol
- 8 personas: Skeptic, Edge-Case Hunter, Scope Sentinel, Ambiguity Detective, Contradiction Finder, Prior-Art Investigator, Success-Criteria Auditor, Constraint Excavator
- Mechanical saturation termination: net-new constraints below threshold for K consecutive rounds
- Output bundle: probe-spec.md, constraints.tsv, questions-asked.tsv, contradictions.md, hidden-assumptions.md, autoresearch-config.yml, summary.md, handoff.json

## v1.8.0 — 2026-03-21

### Added
- `/autoresearch:learn` — autonomous codebase documentation engine
- 4 modes: init, update, check, summarize
- Diff-based targeting for update mode: maps git changes to affected docs
- Validation-fix loop with mechanical verification (max 3 retries)
- Composite metric: `learn_score = validation%*0.5 + coverage%*0.3 + size_compliance%*0.2`

## v1.7.0 — 2026-03-18

### Added
- `/autoresearch:predict` — multi-persona swarm prediction
- 5 default personas: Architecture Reviewer, Security Analyst, Performance Engineer, Reliability Engineer, Devil's Advocate
- Adversarial debate mode (Red/Blue teams), anti-herd detection, budget enforcement
- Zero external dependencies — file-based knowledge representation

## v1.6.2 — 2026-03-10

### Added
- Expanded EXAMPLES.md, GUIDE.md, and CONTRIBUTING.md

## Previous Versions

See `docs/changelog.md` and git history for v1.3.0–v1.6.1 details.

See also: [Development Roadmap](development-roadmap.md) | [Project Overview](project-overview-pdr.md)
