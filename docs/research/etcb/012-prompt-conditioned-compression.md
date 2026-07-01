# ETCB Research Note 012 - Prompt-Conditioned Compression

**Date:** 2026-07-01

**Status:** H19 artifact-only evidence

---

## Context

H18 tested whether the H17 `ultra` result was provider-conditioned.

The answer was negative:

```text
provider-conditioned compression = unsupported
ultra = prompt-conditioned stable
```

The stable H18 scope was:

```text
prompt_variant = audit-emphasis
```

The unstable scope was:

```text
prompt_variant = baseline
```

This created a different frontier. The next question was no longer:

```text
Should compression be conditioned by provider?
```

The H19 question is:

```text
Is compressed-context reviewability primarily controlled by prompt framing
within the H17 balanced matrix?
```

This question is uncomfortable because it moves attention away from context
schema alone. If prompt framing controls whether compressed context remains
reviewable, then a context policy cannot be evaluated independently of the
review instructions given to the model.

---

## Research Decision

Implement H19 as another artifact-only EKOS benchmark and reuse the H17 balanced
matrix before considering new provider calls.

Implementation repository:

```text
2000silpeed/ekos-sap-knowledge-os
```

EKOS implementation commit:

```text
9bba612 feat: add prompt-conditioned compression benchmark
```

Implemented command:

```bash
python -m ekos benchmark prompt-conditioned-compression \
  --source out/etcb-balanced-context-matrix-r3 \
  --out out/etcb-prompt-conditioned-compression-r3
```

Generated EKOS artifacts:

```text
out/etcb-prompt-conditioned-compression-r3/summary.md
out/etcb-prompt-conditioned-compression-r3/prompt_conditioned_compression.json
out/etcb-prompt-conditioned-compression-r3/prompt_conditioned_compression.csv
out/etcb-prompt-conditioned-compression-r3/prompt_context_matrix.csv
out/etcb-prompt-conditioned-compression-r3/prompt_effect_matrix.csv
out/etcb-prompt-conditioned-compression-r3/cell_stability_matrix.csv
```

The run made no provider calls.

---

## Method

H19 reused:

```text
out/etcb-balanced-context-matrix-r3/balanced_context_matrix.json
```

It compared the existing H17 prompt variants:

| Role | Prompt variant |
| --- | --- |
| Baseline prompt | `baseline` |
| Candidate prompt | `audit-emphasis` |

It did not introduce a new prompt and did not optimize the prompt to make
compression win.

Compressed context shapes under test:

```text
compressed
compressed+relationship_path
ultra
```

Primary unit:

```text
prompt_variant x context_shape
```

Cell-level stability still required:

```text
safety non-inferiority
reviewability non-inferiority at margin 0
token reduction >= 10%
stable win rate >= 80%
```

With three runs per provider x prompt x task x context cell, this means a
cell must be 3/3 stable to pass.

---

## Source Sufficiency

H17 was sufficient for H19:

| Requirement | Result |
| --- | --- |
| H17 matrix status | complete |
| Required prompts present | yes |
| Prompt variants | `baseline`, `audit-emphasis` |
| Providers | `anthropic`, `gemini` |
| Task groups | `failure_prone`, `non_failed_hard` |
| Context shapes | `compressed`, `compressed+relationship_path`, `ultra` |
| Runs per cell | 3 |
| Additional provider rerun required | none |

No new provider calls were needed.

If this question is later expanded, the minimum rerun should only cover missing
or new:

```text
provider x prompt_variant x task_group x context_shape
```

cells for `full` and the compressed context shapes, preserving the H17 schema,
cases, temperature, and at least three runs per cell.

---

## H17 Factor Context

H17 already showed that `prompt_variant` was a stronger quality factor than
`context_shape`:

| Factor | Reviewability eta^2 |
| --- | ---: |
| prompt_variant | 0.220 |
| context_shape | 0.037 |
| prompt_variant x context_shape residual | 0.031 |

This matters for H19 because the prompt main effect was not a small side
effect. In H17, prompt framing explained more reviewability variance than
context shape.

---

## Result

H19 supports prompt-conditioned compression, but only with a context-shape
boundary.

Prompt x context matrix:

| Prompt | Context shape | Stable cells | Pair stable-win 0 | Review delta | Token reduction | Label |
| --- | --- | ---: | ---: | ---: | ---: | --- |
| audit-emphasis | compressed | 4/4 | 100.0% | 0.000 | 25.6% | stable |
| audit-emphasis | compressed+relationship_path | 0/4 | 0.0% | -0.250 | 7.0% | unstable |
| audit-emphasis | ultra | 4/4 | 100.0% | 0.000 | 29.1% | stable |
| baseline | compressed | 1/4 | 41.7% | -0.333 | 27.1% | unstable |
| baseline | compressed+relationship_path | 1/4 | 50.0% | -0.167 | 11.4% | unstable |
| baseline | ultra | 1/4 | 66.7% | -0.083 | 31.6% | unstable |

Prompt effect against baseline:

| Context shape | Stable-win lift | Reviewability lift | Token-reduction lift | Label |
| --- | ---: | ---: | ---: | --- |
| compressed | +58.3% | +0.333 | -1.6% | prompt improves stability |
| compressed+relationship_path | -50.0% | -0.083 | -4.4% | prompt reduces stability |
| ultra | +33.3% | +0.083 | -2.4% | prompt improves stability |
| all | +13.9% | +0.111 | -2.8% | prompt improves stability |

The key result:

```text
audit-emphasis stabilized compressed and ultra.
audit-emphasis did not stabilize compressed+relationship_path.
baseline stabilized none of the evaluated compressed context shapes.
```

Classification:

```text
prompt-conditioned compression = supported_with_context_shape_boundary
supported context shapes       = compressed, ultra
unsupported context shapes     = compressed+relationship_path
```

---

## Answer

Prompt-conditioned compression is supported in the H17 matrix, but only for:

```text
compressed
ultra
```

It is not supported for:

```text
compressed+relationship_path
```

The result does not mean that prompt tuning proves compression works. It means
the already-evaluated `audit-emphasis` prompt preserved reviewability for two
compressed context shapes where the baseline prompt did not.

The stronger interpretation is still bounded:

```text
Review-oriented prompt framing appears necessary for compressed-context
stability in the current H17 matrix.
```

But H19 does not justify:

```text
Use audit-emphasis universally.
Use ultra universally.
Use compressed universally.
Adopt a production adaptive context policy.
Claim production cost savings or ROI.
```

---

## What This Falsifies Or Weakens

Weakened:

```text
Compression stability can be evaluated from context shape alone.
```

Falsified in the evaluated H17 scope:

```text
compressed+relationship_path is rescued by audit-emphasis prompt framing.
```

Supported narrowly:

```text
audit-emphasis stabilizes compressed and ultra against full at zero-margin
reviewability in the evaluated H17 matrix.
```

Still unsupported:

```text
Universal adaptive context policy.
Universal ultra default.
Universal compressed default.
Universal field-importance claim.
ROI.
Production cost savings.
```

---

## Interpretation Discipline

The H19 result should be stated as:

```text
Within the H17 balanced matrix, prompt framing was a stronger observed
reviewability factor than context shape, and the audit-emphasis prompt
stabilized compressed and ultra but not compressed+relationship_path.
```

It should not be shortened to:

```text
Prompt engineering solves compression.
Compression works if prompted correctly.
Audit-emphasis is the right prompt.
Ultra is safe by default.
```

The important engineering implication is narrower:

```text
Context compression policy and reviewability prompt policy are coupled.
```

Future ETCB work should not evaluate compressed context shapes without also
controlling the prompt framing that asks the model to produce reviewable
evidence, policy, and authority fields.

---

## Threats To Validity

- H19 is still limited to the H17 matrix: two providers, two prompt variants,
  two task groups, two selected cases, and three runs.
- Only one review-oriented prompt was evaluated.
- The result compares an existing baseline prompt against an existing
  audit-emphasis prompt; it does not search a prompt space.
- `compressed+relationship_path` failed partly because token reduction did not
  meet the 10% rule under audit-emphasis.
- Provider token accounting remains provider-specific and is not monetary cost.
- Reviewability is a discrete scorer component with possible ceiling effects.

---

## Next Research

Do not open a broad prompt benchmark family from this result.

The next useful question is more precise:

```text
Which review instructions are necessary for compressed EKOS context to remain
reviewable without leaking gold labels or overfitting to the scorer?
```

A minimum next experiment, if pursued, should pre-register a small prompt
contrast and keep:

```text
same providers
same cases
same context shapes
same schema
same scorer
same temperature
same run count
```

The current H19 result is already enough to update the research posture:

```text
Adaptive Context Policy remains unsupported as a standalone context-shape
policy. Any future policy must treat prompt framing as part of the evaluated
condition.
```
