# ETCB Research Note 003 - Difficulty-Conditioned ETCB

**Date:** 2026-07-01

**Status:** Preliminary H10c evidence

---

## Context

ETCB began by asking whether EKOS reduces enterprise task completion cost.

ETCB Research Note 001 produced a negative first-turn cost result:

```text
Current EKOS improves quality and reduces hard failures, but increases
first-turn token cost.
```

ETCB Research Note 002 then measured one-turn repair cost for the 9
model-only hard failures. That result was mixed:

```text
Hard-failed subset:
model-only first answer + one repair = 72,107 tokens, 0/9 safe success
EKOS first answer = 35,827 tokens, 9/9 safe success

Full 60-record set:
model-only + one repair = 181,640 tokens, 51/60 safe success
EKOS first answer = 222,466 tokens, 60/60 safe success
```

That evidence corrected the cost question again. The relevant question is not
whether EKOS is always cheaper. It is:

```text
For which enterprise task classes is EKOS economically justified?
```

---

## Research Decision

Start from the existing Provider API R3, ETCB token-cost, and repair-cost
artifacts. Do not create a broad benchmark family and do not optimize prompts
to make EKOS cheaper.

Issue:

```text
2000silpeed/ekos-sap-knowledge-os#14
Difficulty-Conditioned ETCB: identify when EKOS becomes economically preferable
```

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
out/etcb-repair-cost-provider-r3/
```

Difficulty-conditioned output artifact:

```text
out/etcb-difficulty-conditioned-analysis/
```

Generated files:

```text
summary.md
difficulty_conditioned_cost.json
difficulty_conditioned_cost.csv
difficulty_conditioned_records.csv
```

The analysis reuses existing Provider API Delta R3, first-turn ETCB, and
repair-cost artifacts. It does not introduce a new benchmark family, change the
scorer, or optimize prompts.

---

## H10c - Difficulty-Conditioned Cost Hypothesis

H10c:

```text
EKOS is economically justified primarily for difficult or failure-prone
enterprise task classes.
```

Expected pattern before measurement:

```text
Easy tasks: model-only may be cheaper.
Medium tasks: mixed or threshold-dependent.
Hard or failure-prone tasks: EKOS may become cheaper per safe success.
```

This is narrower than the original H10. It does not claim that EKOS is cheaper
for all tasks. It asks where the extra context cost is justified by safer,
more complete task completion.

---

## Difficulty Proxies

Difficulty is derived from model-only behavior and existing consistency data.
It is not treated as an inherent label.

| Bucket | Rule |
| --- | --- |
| failure_prone | model-only hard failure or repair required |
| hard | incorrect max-safe delegation or model-only score <= 17, excluding failure-prone records |
| medium | low reviewability <= 9, inconsistency, policy/authority ambiguity, or stale/incomplete evidence, excluding failure-prone and hard records |
| easy | none of the above proxies |

The proxies used in the 60-record set were:

| Proxy | Records |
| --- | ---: |
| hard_failure | 9 |
| repair_required | 9 |
| low_model_only_score | 18 |
| low_reviewability | 18 |
| inconsistency | 15 |
| policy_authority_ambiguity | 36 |
| stale_or_incomplete_evidence | 12 |

---

## Enterprise Task Classes

The analysis uses reproducible case-level task-class labels for existing
CASE-011 through CASE-015.

| Task class | Cases covered | Records |
| --- | --- | ---: |
| simple_retrieval | none | 0 |
| simple_reasoning | none | 0 |
| structured_reasoning | CASE-011, CASE-012, CASE-013, CASE-014, CASE-015 | 60 |
| authority_reasoning | CASE-011, CASE-012, CASE-014 | 36 |
| policy_reasoning | CASE-011, CASE-012, CASE-014 | 36 |
| conflicting_evidence | none | 0 |
| delegation | CASE-011, CASE-012, CASE-013, CASE-014, CASE-015 | 60 |
| auditability | CASE-011, CASE-012, CASE-013, CASE-014, CASE-015 | 60 |
| compliance_reversibility | CASE-011, CASE-012, CASE-013, CASE-014, CASE-015 | 60 |

This matters because the current evidence cannot answer simple retrieval,
simple reasoning, or true conflicting-evidence economics. Those classes are not
covered by the existing R3 artifact.

---

## Overall Result

| Path | Tokens | Safe successes | Hard failures | Tokens per safe success | Score per 1k tokens | Reviewability per 1k tokens |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Model-only first answer | 132,631 | 51/60 | 9 | 2,600.608 | 8.362 | 4.742 |
| Model-only + one repair for hard failures | 181,640 | 51/60 | 9 | 3,561.569 | 6.155 | 3.512 |
| EKOS first answer | 222,466 | 60/60 | 0 | 3,707.767 | 5.327 | 3.169 |

Overall interpretation:

```text
more_expensive_but_safer
```

Across all 60 records, EKOS is not lower-token. It uses 40,826 more tokens than
model-only plus one repair. However, the comparison is not success-equivalent:
model-only plus repair still leaves 9 hard failures, while EKOS reaches 60/60
safe success.

---

## Difficulty-Conditioned Result

| Bucket | Records | Interpretation | Model+repair tokens | EKOS tokens | Model+repair safe | EKOS safe | Model+repair tokens/safe | EKOS tokens/safe |
| --- | ---: | --- | ---: | ---: | ---: | ---: | ---: | ---: |
| easy | 21 | quality_positive_but_cost_negative | 42,087 | 76,901 | 21/21 | 21/21 | 2,004.143 | 3,661.952 |
| medium | 21 | quality_positive_but_cost_negative | 42,836 | 75,650 | 21/21 | 21/21 | 2,039.810 | 3,602.381 |
| hard | 9 | quality_positive_but_cost_negative | 24,610 | 34,088 | 9/9 | 9/9 | 2,734.444 | 3,787.556 |
| failure_prone | 9 | cheaper | 72,107 | 35,827 | 0/9 | 9/9 | n/a | 3,980.778 |

The strongest signal is the failure-prone bucket:

```text
Model-only first answer + one repair used more tokens and still produced
0/9 safe successes.

EKOS first answers used fewer tokens and produced 9/9 safe successes.
```

For easy, medium, and non-failed hard records, EKOS remains cost-negative under
current token accounting. Those records already succeed without EKOS, so EKOS
mostly adds context cost rather than additional safe-success count.

---

## Task-Class Result

Primary task-class view:

| Primary task class | Records | Interpretation | Model+repair tokens | EKOS tokens | Model+repair safe | EKOS safe | Model+repair tokens/safe | EKOS tokens/safe |
| --- | ---: | --- | ---: | ---: | ---: | ---: | ---: | ---: |
| authority_reasoning | 24 | cheaper | 102,636 | 90,202 | 15/24 | 24/24 | 6,842.400 | 3,758.417 |
| policy_reasoning | 12 | quality_positive_but_cost_negative | 30,100 | 43,435 | 12/12 | 12/12 | 2,508.333 | 3,619.583 |
| delegation | 12 | quality_positive_but_cost_negative | 24,398 | 45,622 | 12/12 | 12/12 | 2,033.167 | 3,801.833 |
| compliance_reversibility | 12 | quality_positive_but_cost_negative | 24,506 | 43,207 | 12/12 | 12/12 | 2,042.167 | 3,600.583 |

Multi-label task-class view:

| Task class | Records | Interpretation | Model+repair tokens | EKOS tokens | Model+repair safe | EKOS safe | Model+repair tokens/safe | EKOS tokens/safe |
| --- | ---: | --- | ---: | ---: | ---: | ---: | ---: | ---: |
| authority_reasoning | 36 | more_expensive_but_safer | 132,736 | 133,637 | 27/36 | 36/36 | 4,916.148 | 3,712.139 |
| policy_reasoning | 36 | more_expensive_but_safer | 132,736 | 133,637 | 27/36 | 36/36 | 4,916.148 | 3,712.139 |
| delegation | 60 | more_expensive_but_safer | 181,640 | 222,466 | 51/60 | 60/60 | 3,561.569 | 3,707.767 |

These rows should not be averaged into a broad delegation claim. The
multi-label delegation row covers all 60 records, including easy tasks where
EKOS is token-expensive. The more useful signal is the intersection:

```text
failure-prone authority/policy/delegation boundary tasks
```

That is where model-only behavior created hard failures and the one-turn repair
path failed to recover safe completion.

---

## Answer To The Research Question

For the evaluated CASE-011 through CASE-015 evidence, EKOS is economically
justified in this narrow condition:

```text
Failure-prone policy/authority/delegation tasks where model-only behavior
produces hard failures or unrecovered repair loops.
```

EKOS is not economically justified by token cost alone for easy, medium, or
already-successful routine records in the current context design.

The stronger interpretation is not "EKOS is cheaper." It is:

```text
EKOS can be economically preferable when the alternative is an unsafe or
unrecovered enterprise decision failure, especially around authority, policy,
delegation, and reversibility boundaries.
```

---

## Relationship To H11 Semantic Compression

H11 remains important because the difficulty-conditioned result separates two
problems:

```text
Failure-prone boundary tasks: EKOS has a strong safety and completion signal.
Already-successful tasks: current EKOS context is too token-heavy.
```

Semantic compression should therefore target the expensive context volume while
preserving the failure-prone safety benefit. Do not compress EKOS blindly.
Compress the cases where EKOS is valuable but token-heavy.

---

## Hypothesis Update

H10a:

```text
Current EKOS improves quality but increases first-turn token cost.
```

Status:

```text
supported
```

H10b:

```text
Workflow-level repair-cost savings remain partially measured.
```

Status:

```text
mixed; favorable on hard-failed subset, not proven overall
```

H10c:

```text
EKOS is economically justified primarily for difficult or failure-prone
enterprise task classes.
```

Status:

```text
supported in narrowed form for evaluated failure-prone
policy/authority/delegation records
```

Difficulty alone is not the full explanation. The non-failed hard bucket still
remains token-negative for EKOS. Task class alone is also not enough because
broad delegation includes many already-successful records. The useful axis is:

```text
difficulty signal x enterprise authority/policy/delegation boundary
```

---

## Rejected Interpretations

Rejected:

```text
EKOS is always cheaper.
```

Reason:

Easy, medium, and non-failed hard records are cost-negative under current
token accounting.

Rejected:

```text
Model-only is cheaper overall, so EKOS loses.
```

Reason:

The overall comparison is not success-equivalent. Model-only plus one repair
uses fewer tokens but leaves 9/60 records unsafe. EKOS uses more tokens but
produces 60/60 safe successes.

Rejected:

```text
EKOS proves ROI.
```

Reason:

This analysis measures provider-reported token cost. It does not measure
provider pricing, human review time, incident cost, escalation cost, or
production workflow cost.

Rejected:

```text
EKOS is proven for all enterprise task classes.
```

Reason:

The current cases do not cover simple retrieval, simple reasoning, or true
conflicting-evidence tasks.

---

## Limitations

- The analysis reuses only CASE-011 through CASE-015.
- Task-class labels are reproducible case-level annotations, not independently
  validated human labels.
- Difficulty buckets are derived from model-only behavior, not inherent task
  difficulty.
- Repair-cost data exists only for the 9 model-only hard-failed Gemini records
  and only for one repair prompt design.
- Multi-label task-class rows are non-additive because one record can belong to
  multiple enterprise task classes.
- Token accounting is not monetary cost accounting.
- Human correction cost remains unmeasured.

---

## Minimum Next Research

Do not add a broad new benchmark family.

The minimum useful next experiment is either:

```text
1. Test one stronger repair condition on the same 9 hard-failed records.
2. Start H11 semantic compression to reduce EKOS context tokens while preserving
   the failure-prone safety benefit.
```

Given the current evidence, H11 is the higher-leverage direction. Current EKOS
already has a strong failure-prone safety signal; the economic weakness is
context volume on tasks that do not need the full EKOS context.

---

## Future AI Context

Future sessions should preserve this split:

```text
EDB quality/reviewability/safety evidence: positive.
ETCB first-turn token efficiency: negative.
ETCB one-turn repair-cost evidence: mixed.
ETCB difficulty-conditioned evidence: favorable only for failure-prone
authority/policy/delegation boundaries.

Do not claim ROI.
Do not claim universal cost savings.
Do not average away task-class differences.
```
