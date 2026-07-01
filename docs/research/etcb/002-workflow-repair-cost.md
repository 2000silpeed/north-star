# ETCB Research Note 002 — Workflow Repair-Cost Measurement

**Date:** 2026-07-01

**Status:** Preliminary H10b evidence

---

## Context

ETCB Research Note 001 showed an uncomfortable first result:

```text
Current EKOS improves quality and reduces hard failures, but increases
first-turn token cost.
```

That result split the cost hypothesis into narrower claims:

```text
H10a: Current EKOS improves quality but increases first-turn token cost.
H10b: Workflow-level repair-cost savings remain unmeasured.
H11: Semantic compression remains open and is now higher priority.
```

This note measures the minimum useful H10b follow-up:

```text
Can a one-turn repair workflow cheaply recover the model-only hard failures
that EKOS avoided?
```

The answer matters because the first ETCB result was negative under simple
single-turn token accounting. If model-only hard failures are easy and cheap to
repair, EKOS's extra context cost may not be justified. If they are not easy to
repair, EKOS's higher first-turn cost may buy real completion value.

---

## Source Artifacts

EKOS implementation repository:

```text
2000silpeed/ekos-sap-knowledge-os
```

Input artifacts:

```text
out/edb-provider-api-delta-anthropic-gemini-r3-2026-07-01/
out/etcb-provider-api-delta-anthropic-gemini-r3-2026-07-01/
```

Repair-cost output artifact:

```text
out/etcb-repair-cost-provider-r3/
```

Generated files:

```text
summary.md
repair_cost_delta.json
repair_cost_delta.csv
raw/
```

---

## Method

The experiment used only the 9 model-only hard-failed records from the Provider
API Delta R3 run.

All 9 hard failures came from:

```text
Gemini: gemini-2.5-flash
```

The repair turn used:

```text
original model-only prompt
+ previous model answer
+ automated review feedback
```

It did not use:

```text
EKOS context
gold label fields
prompt optimization
new benchmark family
human correction time
provider pricing
```

The original first answers were not changed. The repair turn was scored with
the existing EDB scorer and the same DelegationDecisionContract schema.

Missing token metadata was not treated as zero.

---

## Token Metadata Coverage

| Field | Known | Records |
| --- | ---: | ---: |
| Before hard-failed total tokens | 9 | 9 |
| Repair total tokens | 9 | 9 |
| Matching EKOS-after total tokens | 9 | 9 |
| Repair cached tokens | 3 | 9 |
| Repair thoughts tokens | 9 | 9 |

Gemini `totalTokenCount` includes provider-reported `thoughtsTokenCount` when
present, so this remains token-cost evidence, not monetary-cost evidence.

---

## Hard-Failed Subset Result

| Metric | Model-only first + repair | EKOS first answer |
| --- | ---: | ---: |
| Records | 9 | 9 |
| Safe successes | 0 | 9 |
| Repair success rate | 0.000 | 1.000 |
| Total task tokens | 72,107 | 35,827 |
| Tokens per repaired success | n/a | n/a |
| Tokens per safe success | n/a | 3,980.778 |
| Score per 1k task tokens | 2.177 | 4.857 |
| Reviewability per 1k task tokens | 1.179 | 2.847 |

For the hard-failed subset, EKOS dominates this one-repair simulation:

```text
Model-only first + repair used more tokens and still produced 0/9 safe
successes.

EKOS first answers used fewer tokens and produced 9/9 safe successes.
```

This is the strongest H10b signal from the experiment.

---

## Overall R3 Adjusted View

| Metric | Model-only + one repair for hard fails | EKOS first answer |
| --- | ---: | ---: |
| Safe successes | 51 | 60 |
| Hard failures | 9 | 0 |
| Total task tokens | 181,640 | 222,466 |
| Tokens per safe success | 3,561.569 | 3,707.767 |
| Score per 1k task tokens | 6.155 | 5.327 |
| Reviewability per 1k task tokens | 3.512 | 3.169 |
| Additional EKOS tokens vs model-only + one repair | n/a | 40,826 |
| Additional EKOS safe successes | n/a | 9 |
| Incremental EKOS tokens per additional safe success | n/a | 4,536.222 |

This overall view is not success-equivalent.

Model-only plus one repair uses fewer total tokens, but still leaves 9 hard
failures unresolved. EKOS uses more total tokens and achieves 60/60 safe
successes.

Therefore the lower model-only tokens per safe success should not be read as a
completed-task win. It counts only the tasks that already succeeded and leaves
the failed tasks unresolved.

---

## Interpretation

Interpretation label:

```text
mixed_ekos_dominates_failed_subset_but_overall_token_savings_not_proven
```

The H10b result is narrower than a cost-savings claim:

```text
Simple one-turn repair did not cheaply recover the Gemini hard failures that
EKOS avoided.
```

The result supports:

```text
EKOS can buy completion reliability on the evaluated hard-failed subset.
```

It does not support:

```text
EKOS saves money.
EKOS proves ROI.
EKOS is production-cost efficient.
```

The most accurate current statement is:

```text
Current EKOS is higher-token overall in Provider API R3, but the one-turn
repair baseline failed to recover the hard failures. EKOS remains the only
tested path in this run that produced 60/60 safe successes.
```

---

## Hypothesis Status

### H10a — Current EKOS improves quality but increases first-turn token cost.

Status:

```text
supported by ETCB 001
```

### H10b — Workflow-level repair-cost savings remain unmeasured.

Status after this note:

```text
partially measured; favorable on hard-failed subset; not a full workflow-cost
proof
```

The one-turn repair attempt did not recover any hard-failed records. This makes
EKOS favorable on the failed subset, but it still does not measure human review
time, multi-turn repair, escalation, or production workflow cost.

### H11 — Semantic compression.

Status:

```text
not tested
```

This experiment used current EKOS context exactly as-is. It does not test
compact EKOS references, retrieval-on-demand, or prompt-token compression.

---

## Rejected Interpretations

Rejected:

```text
Model-only is cheaper because its adjusted tokens per safe success are lower.
```

Reason:

The comparison is not success-equivalent. Model-only plus one repair still has
only 51/60 safe successes, while EKOS has 60/60.

Rejected:

```text
EKOS has proven task-cost savings.
```

Reason:

EKOS still uses more total tokens across all 60 records, and human correction
cost is unmeasured.

Rejected:

```text
Repair is useless.
```

Reason:

Only one repair prompt design, one model, and one repair turn were tested. The
result shows that this minimal repair loop failed, not that all repair loops
will fail.

---

## Claim Discipline

Allowed:

```text
In the evaluated Provider API R3 hard-failed subset, one model-only repair turn
failed to recover any safe successes, while EKOS first answers produced 9/9
safe successes with fewer tokens than model-only first + repair.
```

Allowed:

```text
Across all 60 Provider API R3 records, EKOS used more total tokens but achieved
9 more safe successes than model-only plus one repair.
```

Not allowed:

```text
EKOS saves money.
EKOS proves ROI.
EKOS is production-ready.
EKOS has proven full workflow-cost reduction.
```

---

## Minimum Next Research

Do not create a broad new benchmark framework.

The minimum useful next step is a decision, not another runner:

```text
Either treat H10b as partially favorable but incomplete and move to H11
semantic compression, or run exactly one stronger repair condition with the
same hard-failed records to test whether a more explicit correction loop can
recover them.
```

If the project goal is cost efficiency, H11 is now higher priority because both
ETCB 001 and 002 show that current EKOS context is expensive in first-turn
tokens.

---

## Future AI Context

Future sessions should preserve the split:

```text
EDB quality evidence: positive.
ETCB 001 first-turn token efficiency: negative.
ETCB 002 one-turn repair-cost evidence: mixed, favorable on hard-failed subset,
but no overall token-savings or ROI claim.
H10a: supported.
H10b: partially measured, still incomplete.
H11: untested and higher priority.
```
