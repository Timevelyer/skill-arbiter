---
name: example-merged
description: Synthetic golden fixture for deterministic text regression only; not a semantic evaluation result.
---

# Example Merged Skill

## Decision rule

Output must include the test status and failing step IDs.

## Worked example

```text
Test status: not-run.
Failing step IDs: none; execution has not been run.
```

### Coverage check

| Sentence | Rule ID |
|---|---|
| `Test status: not-run.` | `beta:R1`（status 部分） |
| `Failing step IDs: none; execution has not been run.` | `beta:R1`（step-ID 部分） |

### Negative check

No retained rule is violated; no undeclared exception is introduced.

### Minimality check

Removing either sentence removes a distinct required part of `beta:R1`; neither sentence is removable.

## Provenance

| Output section | Source rule ID | Disposition | Reason |
|---|---|---|---|
| Decision rule | `beta:R1` | keep | The superset requirement is stricter and includes both required parts. |
| Decision rule | `alpha:R1` | deduplicate | The subset requirement is represented by `beta:R1`. |

## Unincluded rules

| Source rule ID | Reason | Destination |
|---|---|---|
| `alpha:R1` | Deduplicated because it is a subset of the stricter rule. | Represented by `beta:R1`. |

```yaml
arbiter-summary:
  version: "9.0.0"
  mode: "full"
  skills_in: 2
  groups_out: 1
  verdict: "merged"
  rules: {total: 2, kept: 1, deduped: 1, conditional: 0, deferred: 0, excluded: 0}
  conflicts: {exclusive: 0, shared: 1, entangled: 0, contradictory: 0, dependency: 0, intent: 0}
  severity: {S0: 0, S1: 0, S2: 0, S3: 1}
  degraded_level: "none"
  actionability: "prompt-generated"
  confidence: {certain: 0, high: 1, medium: 0, low: 0}
  open_questions: ["Synthetic fixture; takeover probes not-run"]
  appeal_path: "No strong inconsistency; rerun with new evidence"
```
