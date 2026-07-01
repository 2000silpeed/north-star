# ETCB Research Note 010 - Balanced Context Matrix

**Date:** 2026-07-01

**Status:** H17 provider evidence

---

## Context

H16 found that existing H13-H15 artifacts could not support causal variance
decomposition.

The reason was not lack of data volume alone. The artifacts were not fully
crossed:

```text
provider/model
prompt_variant
task_group
context_shape
```

The strongest H16 descriptive signal was uncomfortable for the Adaptive Context
Policy hypothesis:

```text
provider/model and prompt_variant explained more reviewability variance than
context_shape.
```

Therefore H17 did not ask:

```text
Which compressed context is best?
```

It asked:

```text
After balancing provider, prompt variant, task group, and context shape, does
context_shape still explain enough reviewability and token-efficiency variance
to support an adaptive context-policy hypothesis?
```

---

## Research Decision

Implement a minimum balanced provider rerun in the EKOS implementation
repository:

```text
2000silpeed/ekos-sap-knowledge-os
```

EKOS implementation commit:

```text
012f544 feat: add ETCB balanced context matrix benchmark
```

Implemented command:

```bash
python -m ekos benchmark balanced-context-matrix \
  --models models/live-models.local.json \
  --source-difficulty out/etcb-difficulty-conditioned-analysis/difficulty_conditioned_cost.json \
  --runs 3 \
  --max-targets-per-group 1 \
  --out out/etcb-balanced-context-matrix-r3
```

Generated EKOS artifacts:

```text
out/etcb-balanced-context-matrix-r3/summary.md
out/etcb-balanced-context-matrix-r3/balanced_context_matrix.json
out/etcb-balanced-context-matrix-r3/balanced_context_matrix.csv
out/etcb-balanced-context-matrix-r3/factor_effect_matrix.csv
out/etcb-balanced-context-matrix-r3/interaction_matrix.csv
out/etcb-balanced-context-matrix-r3/noninferiority_matrix.csv
```

The run used the same EDB `DelegationDecisionContract` schema and scorer.

---

## Pre-Registered Rules

H17 pre-registered the following interpretation rules inside the report before
scoring:

| Rule | Threshold |
| --- | --- |
| Safety non-inferiority | no hard failures, no over-delegation, no parse/runner failure |
| Max-safe correctness | candidate must match full for the paired cell |
| Reviewability margin | both `0` and `-1` reported |
| Minimum token reduction | 10% |
| Stable win-rate threshold | 80% |

Interaction terms reported:

```text
provider x context_shape
prompt_variant x context_shape
task_group x context_shape
provider x prompt_variant
```

---

## Matrix

H17 used the minimum balanced matrix:

| Dimension | Levels |
| --- | --- |
| task_group | `failure_prone`, `non_failed_hard` |
| provider | `anthropic`, `gemini` |
| prompt_variant | `baseline`, `audit-emphasis` |
| context_shape | `full`, `compressed`, `compressed+relationship_path`, `ultra` |
| runs | 3 |
| temperature | 0.0 |

Target selection was intentionally narrow:

| task_group | selected case |
| --- | --- |
| `failure_prone` | `CASE-012` |
| `non_failed_hard` | `CASE-011` |

Total provider records:

```text
2 task groups x 2 providers x 2 prompts x 4 contexts x 3 runs = 96
```

The missing Anthropic audit-emphasis cell from H16 was included:

| Provider | Prompt variant | Status |
| --- | --- | --- |
| anthropic | baseline | included |
| anthropic | audit-emphasis | included |
| gemini | baseline | included |
| gemini | audit-emphasis | included |

---

## Result

H17 produced a complete 96/96 balanced matrix.

All evaluated context shapes preserved safety:

```text
safety non-inferiority = 100%
hard failures = 0
over-delegation = 0
```

The factor-effect matrix showed:

| Factor | Reviewability eta^2 | Score eta^2 | Token eta^2 |
| --- | ---: | ---: | ---: |
| prompt_variant | 0.220 | 0.220 | 0.025 |
| provider | 0.079 | 0.079 | 0.412 |
| context_shape | 0.037 | 0.037 | 0.427 |
| task_group | 0.035 | 0.035 | 0.013 |

Interpretation:

```text
context_shape remained token-relevant, but it was weaker than prompt_variant
and provider for reviewability and score.
```

This partially confirms the H16 warning. Context shape matters for token
volume, but this balanced H17 run does not show context_shape as the dominant
quality factor.

---

## Context Shape Comparison Against Full

Overall non-inferiority against full:

| Variant | Safety NI | Review NI 0 | Review NI -1 | Token reduction met | Stable win 0 | Stable win -1 | Mean token reduction |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| compressed | 100.0% | 70.8% | 100.0% | 95.8% | 70.8% | 95.8% | 26.3% |
| compressed+relationship_path | 100.0% | 66.7% | 100.0% | 45.8% | 25.0% | 45.8% | 9.2% |
| ultra | 100.0% | 83.3% | 100.0% | 100.0% | 83.3% | 100.0% | 30.4% |

Important observations:

1. `compressed+relationship_path` did not reproduce the H14 win.
2. `ultra` did meet the strict zero-margin stable win threshold in this matrix.
3. The strongest reviewability factor was still `prompt_variant`, not
   `context_shape`.

This creates a narrow but important distinction:

```text
ultra looked stable as a candidate context shape, but Adaptive Context Policy
did not become supported because the balanced factor analysis still assigns
more quality variance to prompt_variant and provider than to context_shape.
```

---

## Interaction Effects

Interaction residuals:

| Interaction | Reviewability residual eta^2 | Score residual eta^2 | Token residual eta^2 |
| --- | ---: | ---: | ---: |
| provider x context_shape | 0.136 | 0.136 | 0.004 |
| prompt_variant x context_shape | 0.031 | 0.031 | 0.002 |
| task_group x context_shape | 0.048 | 0.048 | 0.023 |
| provider x prompt_variant | 0.020 | 0.020 | 0.005 |

The largest residual was `provider x context_shape`.

This means a simple global context policy is still risky. Context shape may
behave differently by provider, and that interaction appears stronger than the
context_shape main effect for reviewability in this run.

---

## Answers

### Does context_shape remain meaningful after balancing?

Yes, but narrowly.

`context_shape` remains meaningful for token volume:

```text
token eta^2 = 0.427
```

But it is weak for reviewability and score:

```text
reviewability eta^2 = 0.037
score eta^2 = 0.037
```

The strongest quality factor was:

```text
prompt_variant, eta^2 = 0.220
```

### Does compressed+relationship_path remain stable?

No.

It was unstable under both reported reviewability margins:

```text
stable win at margin 0  = 25.0%
stable win at margin -1 = 45.8%
```

H17 falsifies the narrow H14/H15 possibility that
`compressed+relationship_path` is a stable recovery/default candidate in this
balanced scope.

### Does ultra remain stable?

Yes, within this evaluated balanced matrix.

`ultra` met the pre-registered stable win threshold:

```text
stable win at margin 0  = 83.3%
stable win at margin -1 = 100.0%
mean token reduction    = 30.4%
safety NI               = 100.0%
```

This does not make `ultra` a universal default. It only means `ultra` was stable
in this minimum H17 matrix.

### Is Adaptive Context Policy supported?

Still inconclusive.

The reason is subtle:

```text
ultra passed the candidate-level stability rule, but context_shape remained a
weaker quality factor than prompt_variant and provider.
```

A policy that adapts context only by context_shape would ignore larger observed
quality drivers and a nontrivial `provider x context_shape` interaction.

Therefore the evidence supports:

```text
ultra remains a candidate for further provider-conditioned study.
```

It does not support:

```text
Adaptive Context Policy as a general EKOS policy.
```

---

## Falsified Or Weakened Hypotheses

Falsified in this evaluated scope:

```text
compressed+relationship_path is the stable reviewability-recovery candidate.
```

Weakened:

```text
context_shape independently dominates reviewability variance.
```

Still alive:

```text
ultra may be a provider- and prompt-conditioned compression candidate.
```

Still unsupported:

```text
Universal adaptive context policy.
Universal field importance.
Production cost savings.
ROI.
```

---

## Threats To Validity

- The matrix is balanced but minimal: one selected case per task group.
- The task scope is still limited to evaluated authority/policy/delegation
  records.
- Reviewability is a discrete scorer component with possible ceiling effects.
- Provider token accounting differs by provider.
- `provider x context_shape` interaction suggests a single global context
  policy may be the wrong model.
- The result does not test new context fields and does not redesign EKOS.

---

## Next Research

The immediate frontier is no longer:

```text
Which context shape wins?
```

The better question is:

```text
Can context policy be provider-conditioned without becoming prompt-tuning or
overfitting?
```

Minimum next work should not expand the benchmark family.

Possible H18:

```text
Provider-Conditioned Compression Stability
```

But a new issue should only be opened after deciding whether the `provider x
context_shape` interaction is strong enough to justify another provider-call
experiment.
