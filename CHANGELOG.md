# Changelog

All notable changes to this project are documented here. This project follows
[Semantic Versioning](https://semver.org/) and the [Keep a Changelog](https://keepachangelog.com/) structure.

## [9.0.1] - 2026-11-23

### Still open

- The 37 semantic cases were **not** executed. `tests/case-results.md` remains `not-run` and `case_runs.skipped_by_owner` is `true`; do not cite them as passing.
- Host installation and explicit invocation must be verified per target Agent before claiming support. This repository was not installed into local OpenCode, Codex, or ZCode by request.

### Fixed

- Corrected the "all 21 core files carry boundary and progress markers" claim in `README.md`, `README.en.md`, and `LAUNCH.md`; `LICENSE` is exempt by D22, so the accurate figure is 21 files with 20 marker-bearing.
- `README.en.md` architecture diagram now has the same node labels and the machine-checked JSON edge list as the Chinese version; without it the English page was the only diagram not covered by the graph/edge consistency check.
- Six install guides no longer claim `agent_created: true` is retained; `install/claude-code.md` no longer claims `allowed-tools` is absent. Both statements were stale after the frontmatter change.
- `OPERATIONS.md` repository visibility and release state updated; `decision-log.md` no longer reports the tag and release as uncreated, and its summary block no longer lists the executed probes as `not-run`.

### Changed

- `SKILL.md` frontmatter now uses only portable, schema-checked fields (`name`, `description`, `category`, `license`, `allowed-tools`); the non-standard `agent_created` field was removed so the skill can be bundled into third-party collections without frontmatter errors.
- `LICENSE` now contains the MIT text only, so GitHub detects the license; the appended metadata block was removed (D22). Consequently 20 of the 21 core files carry `FILE END` / `PROGRESS` markers, and the claims in `README.md`, `README.en.md`, and `LAUNCH.md` were corrected to say so.
- Added `README.en.md` and status badges; the Chinese README links to the English edition. The English architecture diagram now carries the same node labels and JSON edge list as the Chinese one.
- Six install guides updated: they no longer state that `agent_created` is retained (D23); `install/workbuddy.md` documents re-adding it locally if a given WorkBuddy version needs it.
- `OPERATIONS.md` and `decision-log.md` corrected: repository is public, `v9.0.0` tag and release exist, and the decision log no longer lists executed probes as `not-run`.

## [9.0.0] - 2026-11-23

### Added

- Offline eight-layer workflow for pasted `SKILL.md` inputs: parse and atomize, pair matrix, circuit breaker, tri/n-ary checks, global resolution, merge with worked example, adversarial review, and output assembly.
- Six relation types (`exclusive`, `shared`, `entangled`, `contradictory`, `dependency-conflict`, `intent-conflict`) with S0–S3 severity and three hard invariants.
- Templates for rejection, merge, router, and incremental reports, each carrying `arbiter-summary` metadata.
- 37 acceptance scenarios, a five-dimension human rubric, and a SkillClash pair index.
- Installation guidance for Claude Code, Codex, OpenCode, ZCode, DeepSeek Harness, and WorkBuddy.
- Structural CI for frontmatter, rubric record integrity, and deterministic golden regression.

### Notes

- This is a documentation and specification toolkit, not a claimed semantic compiler.
- SkillClash figures are upstream-reported and were not independently reproduced by this project.
- See `LAUNCH.md` for the human gates that remain open before publishing.
