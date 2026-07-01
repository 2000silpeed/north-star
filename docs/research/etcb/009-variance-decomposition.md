# ETCB Research Note 009 - Variance Decomposition

**Date:** 2026-07-01

**Status:** H16 artifact-only evidence

---

## Context

H15 ended the first adaptive-context-policy attempt:

```text
Adaptive Context Policy is not supported yet.
```

The reason was not safety. Safety remained stable across evaluated variants.
The reason was reviewability and policy instability:

```text
H13: process_state appeared reviewability-relevant.
H14: process_state became negative; relationship_path and ultra looked strong.
H15: relationship_path and ultra were not stable enough to justify a policy.
```

The next scientific question is therefore not:

```text
Which context shape wins?
```

The correct H16 question is:

```text
Which observable factors contribute to the benchmark variance, and can the
existing artifacts separate context effects from provider, prompt, task, run,
and scorer effects?
```

---

## Research Decision

Implement the first H16 pass as an artifact-only retrospective analysis in the
EKOS implementation repository.

Implementation repository:

```text
2000silpeed/ekos-sap-knowledge-os
```

GitHub issue:

```text
#18 H16 Variance Decomposition: explain EKOS context-policy instability
```

EKOS implementation commit:

```text
d403cf0 feat: add ETCB variance decomposition analysis
```

Implemented command:

```bash
python -m ekos benchmark variance-decomposition \
  --source-h13 out/etcb-compression-boundary-r3 \
  --source-h14 out/etcb-context-recovery-r3 \
  --source-h15 out/etcb-context-policy-stability-r3 \
  --out out/etcb-variance-decomposition-r3
```

Generated EKOS artifacts:

```text
out/etcb-variance-decomposition-r3/summary.md
out/etcb-variance-decomposition-r3/variance_decomposition.json
out/etcb-variance-decomposition-r3/variance_decomposition.csv
out/etcb-variance-decomposition-r3/factor_variance_matrix.csv
out/etcb-variance-decomposition-r3/context_contrast_matrix.csv
out/etcb-variance-decomposition-r3/scorer_sensitivity.csv
```

The run made no provider calls.

---

## Method

The analysis loaded existing H13-H15 artifacts:

| Source | Artifact | Records |
| --- | --- | ---: |
| H13 | `out/etcb-compression-boundary-r3/compression_boundary.json` | 66 |
| H14 | `out/etcb-context-recovery-r3/context_recovery.json` | 54 |
| H15 | `out/etcb-context-policy-stability-r3/context_policy_stability.json` | 108 |

Total normalized records:

```text
228
```

The runner normalized:

```text
experiment
task_group
group_id
provider
model
prompt_variant
case_id
context_shape
run_index
source_run_index
stability_run_index
score
reviewability
provider tokens
safety flags
parse/runner errors
```

The analysis produced:

1. Marginal variance estimates by observable factor.
2. Context contrasts against `full` and `compressed` where paired rows exist.
3. Scorer sensitivity under reviewability non-inferiority margins:

```text
0 points
-1 point
-2 points
```

The marginal variance estimates are intentionally descriptive. They are not
causal estimates because the existing H13-H15 artifacts are not fully crossed.

---

## Result

H16 found that existing artifacts cannot support causal variance decomposition.

The direct answer:

```text
Existing H13-H15 artifacts support descriptive variance triage, but provider,
prompt, task, and context factors are not fully crossed. Causal attribution is
not supported.
```

Marginal variance matrix:

| Factor | Reviewability eta^2 | Score eta^2 | Token eta^2 | Interpretation |
| --- | ---: | ---: | ---: | --- |
| model | 0.361 | 0.361 | 0.547 | Moderate descriptive association. |
| provider | 0.361 | 0.361 | 0.547 | Candidate variance driver; needs balanced rerun. |
| prompt_variant | 0.293 | 0.293 | 0.249 | Candidate variance driver; needs balanced rerun. |
| context_shape | 0.063 | 0.063 | 0.233 | Small association for score/reviewability; stronger for tokens. |
| group_id | 0.053 | 0.053 | 0.012 | Small descriptive association. |
| task_group | 0.036 | 0.036 | 0.007 | Small in this retrospective artifact set. |
| case_id | 0.030 | 0.030 | 0.002 | Small descriptive association. |
| experiment | 0.008 | 0.008 | 0.004 | Small descriptive association. |

Important interpretation:

```text
Provider/model and prompt_variant explain more observed reviewability and score
variance than context_shape in this retrospective H13-H15 artifact set.
```

This does not prove context shape is unimportant. It means the current artifacts
do not isolate context shape cleanly enough to make a causal claim.

---

## Context Contrast Findings

The H15 contrast against full is the most relevant to adaptive context:

| Variant | Reviewability delta mean vs full | Token delta mean vs full | Quality non-inferior | Fewer tokens |
| --- | ---: | ---: | ---: | ---: |
| compressed | -0.370 | -683.815 | 63.0% | 100.0% |
| compressed+relationship_path | -0.037 | -350.333 | 96.3% | 81.5% |
| ultra | -0.222 | -792.889 | 77.8% | 100.0% |

This explains why H15 felt close but did not justify policy:

```text
compressed+relationship_path was almost reviewability-noninferior on average,
but the zero-margin token-efficient non-inferiority rate was only 77.8%.
```

The observed effect is near a scoring threshold. It may be real, but the current
evidence is too margin-sensitive to justify a default policy.

---

## Scorer Sensitivity

H15 scorer sensitivity against full:

| Variant | Margin 0 | Margin -1 | Margin -2 |
| --- | ---: | ---: | ---: |
| compressed | 63.0% token-efficient non-inferior | 100.0% | 100.0% |
| compressed+relationship_path | 77.8% token-efficient non-inferior | 81.5% | 81.5% |
| ultra | 77.8% token-efficient non-inferior | 100.0% | 100.0% |

This matters because the policy conclusion changes depending on how strictly
reviewability must match full EKOS:

```text
At strict zero-margin reviewability, no candidate reaches a stable policy bar.
At a -1 reviewability margin, compressed and ultra look much stronger.
```

That means H16 exposes a missing research decision:

```text
Adaptive context policy cannot be evaluated until the acceptable reviewability
non-inferiority margin is pre-registered.
```

Without that margin, the analysis can be made to look favorable or unfavorable
by changing the scorer tolerance after the fact.

---

## Confounding Assessment

Provider x prompt crossing:

| Experiment | Status | Observed cells |
| --- | --- | --- |
| H13 | Confounded | Anthropic baseline, Gemini baseline, Gemini audit-emphasis |
| H14 | Confounded | Anthropic baseline, Gemini baseline, Gemini audit-emphasis |
| H15 | Confounded | Anthropic baseline, Gemini baseline, Gemini audit-emphasis |

The missing cell is:

```text
Anthropic audit-emphasis
```

Task group x context crossing is also incomplete:

```text
H13 includes failure_prone, non_failed_hard, and stress groups.
H14 and H15 focus on non_failed_hard.
```

Therefore:

```text
Existing artifacts cannot tell whether instability comes from context shape,
provider/model, prompt variant, task group, scorer threshold, or interactions.
```

They can only identify which factors are plausible variance drivers.

---

## Answer To H16

Can benchmark variance be decomposed from existing artifacts?

```text
Only descriptively, not causally.
```

Largest descriptive reviewability factor:

```text
provider/model
```

Largest descriptive score factor:

```text
provider/model
```

Does this revive Adaptive Context Policy?

```text
No.
```

Adaptive Context Policy remains unsupported because:

```text
1. H15 already showed context-policy wins were not stable enough.
2. H16 shows existing artifacts are confounded.
3. Context shape does not dominate observed reviewability variance descriptively.
4. Policy conclusions are sensitive to reviewability margin choices.
```

---

## Hypothesis Update

H16a:

```text
Safety preservation under compression is more stable than reviewability
preservation.
```

Status:

```text
still supported descriptively
```

H16b:

```text
Reviewability variance is driven by provider x prompt x context interactions,
not context alone.
```

Status:

```text
plausible, not causally proven
```

H16c:

```text
relationship_path may help only in specific provider/prompt/task cells.
```

Status:

```text
plausible; requires balanced rerun
```

H16d:

```text
ultra may be token-efficient but insufficient for full reviewability in some
cells.
```

Status:

```text
still supported
```

H16e:

```text
Some apparent wins are scorer-threshold effects.
```

Status:

```text
supported by sensitivity analysis
```

---

## Rejected Interpretations

Rejected:

```text
Context shape explains the instability by itself.
```

Reason:

Existing artifacts are confounded and context_shape has smaller descriptive
association with reviewability than provider/model or prompt_variant.

Rejected:

```text
Provider/model explains everything.
```

Reason:

Provider/model has the largest descriptive association, but current artifacts
are not balanced enough for causal attribution.

Rejected:

```text
A -1 reviewability margin is acceptable because it makes compressed variants
look stable.
```

Reason:

The margin must be pre-registered and justified by review risk, not selected to
preserve a hypothesis.

Rejected:

```text
H16 proves production cost savings.
```

Reason:

H16 uses provider-reported tokens and benchmark scores. It does not measure
price, reviewer labor, incident risk, or production frequency.

---

## Required Next Experiment

The next experiment must be balanced. It should not add new fields or new task
families.

Minimum balanced matrix:

```text
task_group:
  failure_prone
  non_failed_hard

provider:
  Anthropic
  Gemini

prompt_variant:
  baseline
  audit-emphasis

context_shape:
  full
  compressed
  compressed+relationship_path
  ultra

runs:
  at least 3, preferably 5

temperature:
  0.0
```

The missing Anthropic audit-emphasis cell must be included.

Before running it, pre-register:

```text
reviewability non-inferiority margin
safety non-inferiority rule
minimum token reduction
required win-rate threshold
interaction terms to report
```

The balanced run should answer:

```text
After provider, prompt, task group, and run variance are separated, does
context_shape still explain enough reviewability and token variance to support
an adaptive context policy?
```

---

## Context For Future AI Collaborators

Do not treat H16 as a policy result.

Treat it as a methodological correction:

```text
The current artifact history is useful but confounded.
```

Future work should not add fields to rescue relationship_path or ultra. It
should first make the experimental design balanced enough to separate:

```text
provider effect
prompt effect
task-group effect
context-shape effect
scorer-margin effect
interaction effects
```

Claim discipline remains:

```text
No ROI claim.
No production cost-saving claim.
No universal adaptive policy claim.
No universal field-importance claim.
No average that hides task-class differences.
```
