# ETCB Research Note 007 - Context Recovery Efficiency

**Date:** 2026-07-01

**Status:** Preliminary H14 evidence

---

## Context

ETCB Research Note 006 found the first compression boundary:

```text
Compressed and ultra-compressed EKOS preserved safety on non_failed_hard
records, but both lost reviewability relative to full EKOS.
```

The tempting next hypothesis was:

```text
process_state may be conditionally essential for non_failed_hard reviewability.
```

That hypothesis was plausible because H13 marked `process_state` as
reviewability-relevant. It was not yet causal evidence. H14 therefore did not
immediately implement a process-state recovery policy. It asked the narrower
question:

```text
Which semantic context components provide the highest reviewability recovery
per token?
```

---

## Research Decision

Implement the minimum context-recovery experiment in the EKOS implementation
repository.

Implementation repository:

```text
2000silpeed/ekos-sap-knowledge-os
```

EKOS implementation commit:

```text
180a0c1 feat: add ETCB context recovery benchmark
```

Implemented command:

```bash
python -m ekos benchmark context-recovery \
  --models models/live-models.local.json \
  --source-provider-delta out/edb-provider-api-delta-anthropic-gemini-r3-2026-07-01 \
  --source-difficulty out/etcb-difficulty-conditioned-analysis/difficulty_conditioned_cost.json \
  --max-hard 9 \
  --out out/etcb-context-recovery-r3
```

Generated EKOS artifacts:

```text
out/etcb-context-recovery-r3/summary.md
out/etcb-context-recovery-r3/context_recovery.json
out/etcb-context-recovery-r3/context_recovery.csv
out/etcb-context-recovery-r3/field_recovery_matrix.csv
```

The run produced:

```text
9 non_failed_hard targets x 6 variants = 54 provider records
0 parse failures
0 runner errors
```

---

## Method

The experiment reused:

```text
EDB DelegationDecisionContract schema
score_delegation_answer scorer
H11 compressed and ultra context builders
H13 non_failed_hard target selection
provider adapters and token accounting
```

Target scope:

```text
non_failed_hard only
```

All 9 targets were CASE-011 records:

```text
3 Anthropic baseline runs
3 Gemini audit-emphasis runs
3 Gemini baseline runs
```

Compared variants:

| Variant | Purpose |
| --- | --- |
| full | Full EKOS context reference. |
| compressed | Recovery baseline from H13. |
| compressed+process_state | Test whether exact full process-state text recovers reviewability. |
| compressed+relationship_path | Test whether exact full relationship paths recover reviewability. |
| compressed+process_state+relationship_path | Test the combination. |
| ultra | Non-additive compressed shape comparison. |

Recovery was computed against the `compressed` baseline:

```text
reviewability recovered = variant reviewability - compressed reviewability
recovery target = full reviewability - compressed reviewability
recovery efficiency = reviewability recovered / additional provider tokens
```

The required classification label `High ROI` is treated only as local
field-recovery-efficiency shorthand. It is not a business ROI claim.

---

## Result

| Variant | Tokens | Safe success | Max-safe | Score | Reviewability | Recovery | Token increase vs compressed |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| full | 33,700 | 9/9 | 9/9 | 177 | 105 | 100.0% | 17.0% |
| compressed | 28,812 | 9/9 | 9/9 | 174 | 102 | 0.0% | 0.0% |
| compressed+process_state | 29,254 | 9/9 | 9/9 | 172 | 100 | -66.7% | 1.5% |
| compressed+relationship_path | 30,910 | 9/9 | 9/9 | 177 | 105 | 100.0% | 7.3% |
| compressed+process_state+relationship_path | 32,619 | 9/9 | 9/9 | 175 | 103 | 33.3% | 13.2% |
| ultra | 26,877 | 9/9 | 9/9 | 177 | 105 | 100.0% | -6.7% |

All variants preserved:

```text
9/9 safe success
9/9 max-safe correctness
0 hard failures
0 over-delegation
```

The differentiator was reviewability and token efficiency.

---

## Ranked Recovery

| Field or shape | Reviewability gain | Additional provider tokens | Recovery efficiency per 1k tokens | Classification |
| --- | ---: | ---: | ---: | --- |
| relationship_path | +3 | +2,098 | 1.430 | Essential |
| full_context_remainder | +3 | +4,888 | 0.614 | Conditional |
| process_state+relationship_path | +1 | +3,807 | 0.263 | Conditional |
| ultra_context_shape | +3 | -1,935 | n/a | Conditional |
| process_state | -2 | +442 | -4.525 | Noisy |

The additive winner was:

```text
compressed+relationship_path
```

It recovered full EKOS reviewability with materially fewer tokens than full
EKOS:

```text
full: 33,700 tokens, reviewability 105
compressed+relationship_path: 30,910 tokens, reviewability 105
```

The non-additive surprise was:

```text
ultra: 26,877 tokens, reviewability 105
```

Ultra matched full reviewability with fewer tokens than both full and
compressed in this run. That contradicts H13, where ultra lost reviewability on
non_failed_hard.

---

## Answer To H14

Highest additive reviewability recovery field:

```text
relationship_path
```

Best additive recovery per token:

```text
compressed+relationship_path
```

Can full EKOS be approximated using only a subset of semantic fields?

```text
Yes, in this run.
compressed+relationship_path matched full reviewability and safety with fewer
tokens than full EKOS.
```

Does an adaptive context policy emerge naturally?

```text
Only as a weak candidate, not as a stable policy.
```

The candidate policy is:

```text
Start with compressed EKOS.
For evaluated non_failed_hard reviewability recovery, add relationship_path
before restoring full context.
```

But this is not yet stable because:

```text
H13 suggested process_state was the boundary.
H14 showed process_state was negative.
H13 showed ultra lost reviewability.
H14 showed ultra matched full reviewability with fewer tokens.
```

The correct interpretation is that H14 found a stronger next frontier:

```text
context-policy stability under provider-time variance
```

---

## Hypothesis Update

H13 process-state recovery hypothesis:

```text
process_state may be conditionally essential for non_failed_hard reviewability.
```

Status:

```text
not supported by H14
```

In H14, adding process_state alone made the result worse:

```text
reviewability: 102 -> 100
tokens: 28,812 -> 29,254
```

Relationship-path recovery hypothesis:

```text
relationship_path may recover non_failed_hard reviewability more efficiently
than restoring full EKOS context.
```

Status:

```text
preliminarily supported by H14
```

Ultra compression hypothesis:

```text
ultra may be sufficient for non_failed_hard reviewability.
```

Status:

```text
unstable; H13 and H14 conflict
```

---

## Field Classification

| Field | Classification | Evidence |
| --- | --- | --- |
| authority_boundary | Essential | Retained across safety-preserving compressed variants; not isolated in H14. |
| policy_constraint | Essential | Retained across safety-preserving compressed variants; not isolated in H14. |
| evidence_reference | Essential | Retained across safety-preserving compressed variants; not isolated in H14. |
| audit_reviewability_cue | Essential | Retained across safety-preserving compressed variants; not isolated in H14. |
| blocking_risk_codes | Essential | Retained across safety-preserving compressed variants; not isolated in H14. |
| reversibility | Essential | Retained across safety-preserving compressed variants; not isolated in H14. |
| approval_boundary | Essential | Retained across safety-preserving compressed variants; not isolated in H14. |
| relationship_path | Essential | Recovered 100% of lost reviewability with fewer tokens than full EKOS. |
| process_state | Noisy | Added tokens and reduced reviewability in this run. |
| process_state+relationship_path | Conditional | Positive but worse than relationship_path alone. |
| full_context_remainder | Conditional | Recovered reviewability but less efficiently than relationship_path. |
| evidence_policy_prose | Unknown | Short prose already exists in compressed context; not independently isolated. |
| context_field_sources | Unknown | Present in full context but not isolated from other full-context remainder fields. |
| ultra_context_shape | Conditional | Matched full reviewability with fewer tokens in H14 but contradicted H13. |

---

## Rejected Interpretations

Rejected:

```text
Process_state improves reviewability under evaluated non_failed_hard records.
```

Reason:

H14 shows the opposite for this run.

Rejected:

```text
Ultra is now universally safe and reviewable.
```

Reason:

H13 and H14 conflict. The ultra result is promising but unstable.

Rejected:

```text
Relationship_path is universally essential.
```

Reason:

H14 tested only CASE-011 non_failed_hard records. The result is local.

Rejected:

```text
This proves ROI or production cost savings.
```

Reason:

The experiment measures provider-reported tokens and scorer outputs, not price,
review labor, incident cost, production frequency, or adoption cost.

---

## Limitations

- Scope is limited to 9 non_failed_hard CASE-011 records.
- Provider-time variance is visible: H13 and H14 disagree on ultra behavior.
- Field labels are local recovery-efficiency classifications, not universal
  EKOS field rules.
- Relationship_path was tested as full relationship_paths added to compressed
  context, not as a minimal compressed relationship signature.
- Provider-reported tokens are not monetary cost.
- The result does not cover simple retrieval, simple reasoning, all task
  classes, or production workflows.

---

## Minimum Next Research

Do not expand into a broad benchmark family.

The next issue should test stability, not more fields:

```text
H15 Context Policy Stability
```

Minimum H15 question:

```text
Are the H14 relationship_path and ultra wins stable under rerun, or are they
provider-time variance?
```

Minimum design:

```text
same non_failed_hard target set
variants: compressed, compressed+relationship_path, ultra, full
repeat enough runs to estimate stability
same schema and scorer
no prompt optimization
```

Future sessions should preserve the current answer:

```text
H14 falsified process_state recovery in this run. Relationship_path was the
best additive recovery field, and ultra dominated as a non-additive shape, but
H13/H14 conflict means no universal adaptive context policy is justified yet.
```
