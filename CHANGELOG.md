# Changelog

All notable changes to this project are documented here. This project follows
[Semantic Versioning](https://semver.org/) and the [Keep a Changelog](https://keepachangelog.com/) structure.

## [Unreleased]

### Pending before release

- `tests/rubric-score.yml` is signed off (`review_status: approved`, `reviewer: Timevelyer`, `reviewed_by_role: human`). The `signoff_basis` field records that the sign-off follows the owner's instruction and rests on the automated first-pass review; the owner did not re-read each dimension independently.
- The 37 semantic cases were **not** executed. `tests/case-results.md` remains `not-run` and `case_runs.skipped_by_owner` is `true`; do not cite them as passing.
- Host installation and explicit invocation must be verified per target Agent before claiming support. This repository was not installed into local OpenCode, Codex, or ZCode by request.

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
