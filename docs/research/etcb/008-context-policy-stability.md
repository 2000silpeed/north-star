# ETCB Research Note 008 - Context Policy Stability

**Date:** 2026-07-01

**Status:** H15 provider evidence

---

## Context

ETCB Research Note 007 found a tempting H14 result:

```text
compressed+relationship_path recovered full reviewability with fewer tokens.
ultra also matched full reviewability with fewer tokens.
```

That result conflicted with H13:

```text
H13: process_state appeared reviewability-relevant.
H14: process_state was negative.

H13: ultra lost reviewability on non_failed_hard.
H14: ultra matched full reviewability on non_failed_hard.
```

The correct next question was not to add more fields or redesign EKOS. The
question was stability:

```text
Are the H14 relationship_path and ultra wins stable under rerun, or are they
provider-time variance?
```

H15 therefore tests the H14 candidate policy under repeated same-scope provider
calls.

---

## Research Decision

Implement the minimum context-policy-stability benchmark in the EKOS
implementation repository.

Implementation repository:

```text
2000silpeed/ekos-sap-knowledge-os
```

EKOS implementation commit:

```text
7340ddd feat: add ETCB context policy stability benchmark
```

Implemented command:

```bash
python -m ekos benchmark context-policy-stability \
  --models models/live-models.local.json \
  --source-provider-delta out/edb-provider-api-delta-anthropic-gemini-r3-2026-07-01 \
  --source-difficulty out/etcb-difficulty-conditioned-analysis/difficulty_conditioned_cost.json \
  --max-hard 9 \
  --runs 3 \
  --out out/etcb-context-policy-stability-r3
```

Generated EKOS artifacts:

```text
out/etcb-context-policy-stability-r3/summary.md
out/etcb-context-policy-stability-r3/context_policy_stability.json
out/etcb-context-policy-stability-r3/context_policy_stability.csv
out/etcb-context-policy-stability-r3/stability_matrix.csv
```

The run produced:

```text
9 non_failed_hard targets
4 variants
3 repeated provider runs
108 provider records
0 parse failures
0 runner errors
```

---

## Method

H15 reused the H14 context-recovery architecture:

```text
EDB DelegationDecisionContract schema
score_delegation_answer scorer
H14 non_failed_hard target selection
H11/H14 context builders
provider adapters and token accounting
```

Target scope:

```text
non_failed_hard only
```

Compared variants:

| Variant | Purpose |
| --- | --- |
| full | Full EKOS reference for the same run. |
| compressed | Compressed EKOS baseline. |
| compressed+relationship_path | H14 additive candidate. |
| ultra | H14 non-additive candidate. |

No process_state variant was added because H15 was explicitly a stability
test, not a new field-search benchmark.

The benchmark used two win-rate definitions:

| Metric | Meaning |
| --- | --- |
| Win vs full | Variant preserved safety and reviewability with no more provider tokens than same-run full. |
| Win vs compressed | Variant preserved safety while improving reviewability over compressed, or matched compressed reviewability with fewer tokens. |

The stability label was intentionally stricter than an aggregate total match:
a candidate must be stable across repeated provider-time calls and per-record
comparisons, not merely look good after aggregation.

---

## Result

All variants preserved safety:

```text
safe success: 27/27 for each variant
max-safe correctness: 27/27 for each variant
hard failures: 0
over-delegation: 0
parse failures: 0
runner errors: 0
```

The instability is not a safety failure. It is a reviewability and policy
stability failure.

| Variant | Tokens | Score | Reviewability | Win vs full | Win vs compressed | Stability label |
| --- | ---: | ---: | ---: | ---: | ---: | --- |
| full | 101,457 | 532 | 316 | 100.0% | 37.0% | baseline |
| compressed | 82,994 | 522 | 306 | 63.0% | 0.0% | baseline_compressed |
| compressed+relationship_path | 91,998 | 531 | 315 | 77.8% | 40.7% | unstable |
| ultra | 80,049 | 526 | 310 | 77.8% | 81.5% | stable_vs_compressed_only |

Per repeated run:

| Run | full reviewability | compressed | compressed+relationship_path | ultra |
| ---: | ---: | ---: | ---: | ---: |
| 0 | 105 | 102 | 105 | 104 |
| 1 | 105 | 102 | 105 | 103 |
| 2 | 106 | 102 | 105 | 103 |

Relationship_path had zero aggregate reviewability variance:

```text
compressed+relationship_path reviewability: 105, 105, 105
```

However, full moved from 105 to 106 in run 2, and per-record token/reviewability
comparisons lowered the candidate's win rate:

```text
compressed+relationship_path win vs full: 77.8%
compressed+relationship_path win vs compressed: 40.7%
```

Ultra remained token-efficient and often beat compressed, but it did not
approximate full reviewability:

```text
ultra reviewability: 104, 103, 103
ultra win vs full: 77.8%
ultra win vs compressed: 81.5%
```

---

## Answer To H15

Is H14 stable?

```text
No. H14 is unstable under this rerun.
```

Relationship_path result:

```text
compressed+relationship_path is not stable enough to claim full EKOS
approximation on evaluated non_failed_hard records.
```

The field remains useful evidence:

```text
It preserved safety, scored near full, and improved aggregate reviewability
relative to compressed.
```

But it is not stable policy evidence:

```text
Win vs full was 77.8%, below the H15 stability threshold.
Win vs compressed was only 40.7% because many compressed records already had
equal reviewability and the added relationship paths cost tokens.
```

Ultra result:

```text
Ultra is stable only against compressed under this run, not against full.
```

Ultra should not be proposed as a default context policy:

```text
It preserved safety and reduced tokens, but reviewability stayed below full
and H13/H14/H15 now show unstable reviewability behavior.
```

Adaptive context policy:

```text
The adaptive-context hypothesis remains a research hypothesis, not a stable
policy.
```

---

## Hypothesis Update

Previous H14 candidate:

```text
For evaluated non_failed_hard reviewability recovery, start with compressed
EKOS and add relationship_path before restoring full context.
```

Status after H15:

```text
not stable enough for policy
```

Narrow surviving claim:

```text
Relationship_path can improve aggregate reviewability relative to compressed
on evaluated non_failed_hard records while preserving safety.
```

Claim that must be removed:

```text
compressed+relationship_path reliably approximates full EKOS on non_failed_hard
records.
```

Ultra hypothesis after H15:

```text
Ultra can preserve safety and beat compressed token efficiency, but does not
stably recover full reviewability.
```

Process_state hypothesis remains unsupported:

```text
H15 did not retest process_state because H14 had already made it negative and
the H15 question was stability, not field expansion.
```

---

## Rejected Interpretations

Rejected:

```text
H14 proved a stable adaptive context policy.
```

Reason:

H15 measured repeated same-scope provider calls and found H14 unstable.

Rejected:

```text
Relationship_path is universally essential.
```

Reason:

H15 only tested evaluated non_failed_hard CASE-011 records. The additive
relationship_path candidate did not reach stability against full.

Rejected:

```text
Ultra should become the default compressed context.
```

Reason:

Ultra preserved safety and beat compressed on token efficiency, but it did not
stably approximate full reviewability.

Rejected:

```text
This proves ROI or production cost savings.
```

Reason:

H15 measures provider-reported tokens and benchmark scores. It does not measure
price, human review labor, production incident cost, workflow frequency, or
adoption cost.

---

## Limitations

- Scope is limited to 9 non_failed_hard CASE-011 records.
- All results are over the configured Anthropic/Gemini provider run for this
  date and artifact set.
- Stability thresholds are local benchmark rules, not universal deployment
  criteria.
- H15 measures repeated provider calls, not production drift over time.
- Relationship_path was added as the full relationship_paths field, not a
  smaller relationship signature.
- Provider-reported tokens are not monetary cost.
- The result does not cover easy, medium, failure-prone, stress-overlay, or
  production task classes.

---

## Context For Future AI Collaborators

Do not continue from H14 as if it established an adaptive policy.

The current research frontier is:

```text
Safety preservation under compression is still strong in the evaluated narrow
scope, but reviewability recovery is not stable enough to justify a default
adaptive context policy.
```

The next research should not add fields just to rescue H14. A better next
question is:

```text
Can EKOS predict when compressed context is enough and when full context is
needed, using observable task or answer signals rather than fixed field rules?
```

A minimum next experiment should stay narrow and may compare:

```text
compressed
compressed+relationship_path
full
```

against a decision rule based on answer reviewability risk. That would test
context policy selection, not universal field importance.

Future claims must preserve this discipline:

```text
No ROI claim.
No production cost-saving claim.
No universal adaptive policy claim.
No universal compression claim.
No average that hides task-class differences.
```
