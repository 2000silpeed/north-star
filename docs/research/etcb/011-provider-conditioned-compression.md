# ETCB Research Note 011 - Provider-Conditioned Compression

**Date:** 2026-07-01

**Status:** H18 artifact-only evidence

---

## Context

H17 completed a 96/96 balanced matrix across:

```text
provider
prompt_variant
task_group
context_shape
run
```

The H17 aggregate result looked favorable to `ultra`:

```text
stable win at reviewability margin 0  = 83.3%
stable win at reviewability margin -1 = 100.0%
safety non-inferiority                = 100.0%
mean token reduction                  = 30.4%
```

But H17 also showed that:

```text
provider x context_shape interaction > context_shape main effect
```

Therefore H18 did not ask whether `ultra` was globally good. It asked a
narrower falsification question:

```text
Is ultra compression stable within specific provider x prompt x task cells,
or was the H17 ultra result an artifact of aggregation?
```

---

## Research Decision

Implement H18 as an artifact-only benchmark in the EKOS implementation
repository and reuse the H17 output first.

Implementation repository:

```text
2000silpeed/ekos-sap-knowledge-os
```

GitHub issue:

```text
#19 H18 Provider-Conditioned Compression Stability
```

EKOS implementation commit:

```text
3d9260b feat: add provider-conditioned compression benchmark
```

Implemented command:

```bash
python -m ekos benchmark provider-conditioned-compression \
  --source out/etcb-balanced-context-matrix-r3 \
  --out out/etcb-provider-conditioned-compression-r3
```

Generated EKOS artifacts:

```text
out/etcb-provider-conditioned-compression-r3/summary.md
out/etcb-provider-conditioned-compression-r3/provider_conditioned_compression.json
out/etcb-provider-conditioned-compression-r3/provider_conditioned_compression.csv
out/etcb-provider-conditioned-compression-r3/provider_context_matrix.csv
out/etcb-provider-conditioned-compression-r3/cell_stability_matrix.csv
```

The run made no provider calls.

---

## Method

H18 loaded:

```text
out/etcb-balanced-context-matrix-r3/balanced_context_matrix.json
```

It reused the H17 full-paired comparison rows instead of regenerating model
outputs.

Primary comparison:

```text
ultra vs full
```

Reference baselines:

```text
compressed vs full
compressed+relationship_path vs full
```

Cell dimensions:

```text
provider x prompt_variant x task_group
```

Run dimension:

```text
run_index
```

Per-cell metrics:

- safety non-inferiority
- max-safe correctness
- reviewability non-inferiority at margin 0
- reviewability non-inferiority at margin -1
- token reduction
- stable win rate
- reviewability delta vs full
- score delta vs full
- token delta vs full

H18 used the H17 stable-win threshold:

```text
safety non-inferior
reviewability non-inferior
token reduction >= 10%
stable win rate >= 80%
```

With three runs per cell, a cell must be 3/3 stable to pass the 80% threshold.

---

## Source Sufficiency

H17 data was sufficient for the H18 artifact-only question:

| Requirement | Result |
| --- | --- |
| H17 matrix status | complete |
| H17 records | 96 |
| Full-paired comparisons | 72 |
| Ultra provider x prompt x task cells | 8 |
| Runs per ultra cell | 3 |
| Additional provider rerun required | none |

No new provider calls were needed.

If this question is later expanded beyond the H17 scope, the minimum additional
rerun would be:

```text
Only the missing or new provider x prompt_variant x task_group x context_shape
cells for full and ultra, with the same H17 prompt, schema, target cases,
temperature, and at least three runs per cell.
```

`compressed` and `compressed+relationship_path` would remain useful reference
baselines but are not required to answer the narrow ultra-vs-full question.

---

## Result

H18 found that the H17 aggregate `ultra` win does not mean global cell-level
stability.

Cell-level result for `ultra`:

| Provider | Prompt | Task group | Stable win 0 | Review delta | Token delta | Label |
| --- | --- | --- | ---: | ---: | ---: | --- |
| anthropic | audit-emphasis | failure_prone | 100.0% | 0.000 | -1326.333 | stable |
| anthropic | audit-emphasis | non_failed_hard | 100.0% | 0.000 | -1047.333 | stable |
| anthropic | baseline | failure_prone | 66.7% | -0.333 | -1315.000 | unstable |
| anthropic | baseline | non_failed_hard | 33.3% | -0.667 | -1016.333 | unstable |
| gemini | audit-emphasis | failure_prone | 100.0% | 0.000 | -1131.000 | stable |
| gemini | audit-emphasis | non_failed_hard | 100.0% | 0.000 | -705.000 | stable |
| gemini | baseline | failure_prone | 100.0% | 1.000 | -1413.000 | stable |
| gemini | baseline | non_failed_hard | 66.7% | -0.333 | -676.667 | unstable |

Overall cell stability:

```text
stable cells at margin 0 = 5 / 8 = 62.5%
```

This differs from the H17 aggregate pair-level stable-win rate:

```text
stable pairs at margin 0 = 20 / 24 = 83.3%
```

That distinction is the core H18 result. The H17 aggregate passed the stable-win
threshold because pair-level aggregation smoothed over unstable cells.

---

## Conditioning Summary

| Scope | Value | Stable cells | Stable-cell rate | Pair stable-win 0 | Label |
| --- | --- | ---: | ---: | ---: | --- |
| overall | overall | 5/8 | 62.5% | 83.3% | unstable |
| provider | anthropic | 2/4 | 50.0% | 75.0% | unstable |
| provider | gemini | 3/4 | 75.0% | 91.7% | unstable |
| prompt_variant | audit-emphasis | 4/4 | 100.0% | 100.0% | stable |
| prompt_variant | baseline | 1/4 | 25.0% | 66.7% | unstable |
| task_group | failure_prone | 3/4 | 75.0% | 91.7% | unstable |
| task_group | non_failed_hard | 2/4 | 50.0% | 75.0% | unstable |

Classification:

```text
ultra = prompt-conditioned stable
provider-conditioned compression = unsupported
```

The stable conditioning scope was:

```text
prompt_variant = audit-emphasis
```

It was not:

```text
provider = anthropic
provider = gemini
task_group = failure_prone
task_group = non_failed_hard
```

---

## Answer

Provider-conditioned compression is not supported by H17.

The observed result is:

```text
ultra stability appears prompt-conditioned, not provider-conditioned.
```

This weakens the interpretation that the H17 `provider x context_shape`
interaction directly implies a provider-conditioned compression policy.

It does not mean provider is irrelevant. H17 still showed a meaningful provider
interaction and a strong provider token association. H18 only shows that, for
the current H17 matrix, provider alone does not produce a stable rule for
`ultra`.

---

## What This Falsifies Or Weakens

Falsified in the H17/H18 evaluated scope:

```text
ultra is globally stable across provider x prompt x task cells.
```

Unsupported:

```text
Provider-conditioned compression policy.
```

Supported narrowly:

```text
ultra is stable under audit-emphasis across the evaluated providers and task
groups.
```

Still unsupported:

```text
Universal adaptive context policy.
Universal ultra default.
Universal field-importance claim.
Production cost-saving claim.
ROI claim.
```

---

## Interpretation Discipline

The strongest allowed positive statement is:

```text
Within the H17 minimum balanced matrix, ultra was stable for all audit-emphasis
cells and unstable as a global cell-level rule.
```

The result must not be converted into:

```text
Use ultra by default.
Use ultra for Gemini.
Use ultra for Anthropic.
Compression saves production cost.
Adaptive context policy is now supported.
Prompt tuning proves compression works.
```

The safer interpretation is:

```text
Prompt framing may dominate whether compressed semantic context remains
reviewable.
```

That is uncomfortable because it means the compression-policy problem may be
less about context shape and more about the interaction between context shape
and review instruction.

---

## Threats To Validity

- The matrix is still minimal: two providers, two prompt variants, two task
  groups, two selected cases, and three runs.
- Cell-level stability with three runs is strict: one miss makes a cell fail the
  80% threshold.
- Reviewability is a discrete scorer component and may have ceiling effects.
- `audit-emphasis` stability may reflect the prompt making reviewability cues
  more explicit, not compression robustness by itself.
- Provider token accounting remains provider-specific and is not monetary cost.
- The result does not test new context fields or a redesigned EKOS context
  schema.

---

## Next Research

Do not open a broad benchmark family from this result.

The next research question should shift away from provider-conditioned
compression:

```text
Is compressed-context reviewability primarily controlled by prompt framing?
```

A minimum next step, if pursued, should preserve H17 discipline:

```text
same providers
same cases
same schema
same context shapes
same temperature
pre-registered prompt contrast
at least three runs per provider x prompt x task x context cell
```

But the immediate conclusion is enough for Issue #19:

```text
Provider-conditioned compression is unsupported by the current H17 evidence.
No additional provider rerun is required to answer the H18 artifact-only
question.
```
