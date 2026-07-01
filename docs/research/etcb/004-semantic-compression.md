# ETCB Research Note 004 - Semantic Compression

**Date:** 2026-07-01

**Status:** Preliminary H11 evidence

---

## Context

ETCB Research Notes 001 through 003 established the current cost boundary:

```text
EDB quality/reviewability/safety evidence: positive.
ETCB first-turn token efficiency: negative.
ETCB one-turn repair-cost evidence: mixed.
Difficulty-conditioned ETCB supports H10c only in a narrow form:
EKOS is economically favorable for evaluated failure-prone
policy/authority/delegation boundary tasks.
```

The strongest favorable cost signal before this note was the 9-record
failure-prone subset:

| Path | Tokens | Safe success |
| --- | ---: | ---: |
| Model-only first answer + one repair | 72,107 | 0/9 |
| EKOS first answer | 35,827 | 9/9 |

That result did not prove ROI or universal cost savings. It showed that current
EKOS can buy safety and completion reliability on the evaluated failure-prone
authority/policy/delegation records. The remaining problem was context volume.

H11 asks:

```text
Can EKOS preserve its failure-prone safety benefit while reducing context tokens?
```

---

## Research Decision

Implement the minimum H11 semantic compression experiment.

Do not create a broad new benchmark family. Do not optimize prompts to make
EKOS look cheaper. Reuse the existing EDB schema, scorer, provider artifact, and
difficulty-conditioned target selection.

Implementation repository:

```text
2000silpeed/ekos-sap-knowledge-os
```

Implemented command:

```bash
python -m ekos benchmark semantic-compression \
  --models models/live-models.local.json \
  --source-provider-delta out/edb-provider-api-delta-anthropic-gemini-r3-2026-07-01 \
  --source-difficulty out/etcb-difficulty-conditioned-analysis/difficulty_conditioned_cost.json \
  --variants full,compressed,ultra \
  --out out/etcb-semantic-compression-r3
```

Generated EKOS artifacts:

```text
out/etcb-semantic-compression-r3/summary.md
out/etcb-semantic-compression-r3/semantic_compression_delta.json
out/etcb-semantic-compression-r3/semantic_compression_delta.csv
```

---

## H11 - Semantic Compression

H11:

```text
EKOS can preserve its failure-prone safety benefit while reducing context tokens.
```

This is not a universal cost-savings hypothesis. It is a targeted compression
hypothesis over the evaluated failure-prone boundary records.

---

## Method

Target selection used the existing difficulty-conditioned ETCB artifact:

```text
difficulty_bucket == failure_prone
model-only first answer hard-failed
model-only plus one repair did not produce safe success
EKOS first answer produced safe success
```

This selected 9 records:

```text
Provider/model: Gemini gemini-2.5-flash
Cases: CASE-012 and CASE-014
CASE-012 records: 6
CASE-014 records: 3
```

The experiment compared three EKOS context variants:

| Variant | Source |
| --- | --- |
| `full` | Existing Provider API Delta R3 EKOS-after records |
| `compressed` | New provider calls with shorter EKOS context |
| `ultra` | New provider calls with the shortest tested EKOS context |

The full rows were reused from the existing R3 artifact instead of being rerun.
The compressed and ultra rows were new Gemini provider API calls on the same
case, prompt variant, and run-index keys.

All variants reused:

```text
DelegationDecisionContract schema
score_delegation_answer scorer
same CASE-012 / CASE-014 case packets
same prompt variants from the selected pair keys
```

Compression preserved:

```text
authority boundary
policy constraint
evidence references
approval role and condition
blocking risk codes
reversibility classification
auditability signal
```

Compression did not include:

```text
gold_label
mandatory_evidence_ids
mandatory_policy_ids
maximum_safe_delegation_level
completed DelegationDecisionContract
```

The target was not merely shorter text. The target was to preserve enterprise
meaning while removing unnecessary context volume.

---

## Result

| Variant | Records | Input tokens | Output tokens | Total tokens | Safe success | Score | Reviewability | Max-safe correct | Hard failures | Over-delegation | Score/1k | Reviewability/1k | Tokens/safe |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| full | 9 | 26,508 | 2,614 | 35,827 | 9/9 | 174 | 102 | 9/9 | 0 | 0 | 4.857 | 2.847 | 3,980.778 |
| compressed | 9 | 17,034 | 2,404 | 26,245 | 9/9 | 180 | 108 | 9/9 | 0 | 0 | 6.858 | 4.115 | 2,916.111 |
| ultra | 9 | 14,982 | 2,679 | 24,554 | 9/9 | 180 | 108 | 9/9 | 0 | 0 | 7.331 | 4.398 | 2,728.222 |

Token reduction versus full EKOS:

| Variant | Total-token reduction | Input-token reduction |
| --- | ---: | ---: |
| compressed | 9,582 tokens, 26.7% | 9,474 tokens |
| ultra | 11,273 tokens, 31.5% | 11,526 tokens |

---

## Answer To H11

For the evaluated 9 failure-prone Gemini records:

```text
Yes. EKOS reduced context tokens while preserving the observed safety benefit.
```

Both compressed variants preserved:

```text
9/9 safe success
9/9 max-safe correctness
0 hard failures
0 over-delegation
```

Both compressed variants also improved score and reviewability relative to the
reused full-context R3 rows:

```text
Score: 174 -> 180
Reviewability: 102 -> 108
```

This is a positive H11 result, but it is narrow. It applies to the evaluated
failure-prone authority/policy/delegation boundary records, not to all
enterprise tasks.

---

## Field Assessment

Essential fields in this experiment:

- authority boundary / prepare-only versus execution authority
- approval role and approval condition
- policy reference IDs
- evidence reference IDs
- blocking risk codes
- reversibility classification
- auditability signal requiring evidence-policy-boundary linkage

Compressible fields in this experiment:

- verbose evidence and policy prose
- expanded process-state narrative
- relationship path objects when boundary references are retained
- context field source/provenance labels
- long audit requirement wording

The strongest design lesson is:

```text
EKOS does not need to copy every semantic explanation into the prompt when the
authority boundary, policy/evidence references, risk code, reversibility, and
audit signal remain explicit.
```

---

## Interpretation

H11 is supported in a preliminary, narrow form:

```text
Semantic compression can preserve EKOS's failure-prone safety benefit while
reducing provider-reported total tokens on the evaluated authority/policy/
delegation boundary records.
```

This result strengthens the ETCB path because it attacks the actual weakness
found in Notes 001 through 003: context volume.

It does not reverse the earlier result that current full EKOS context is
token-expensive for easy, medium, and already-successful records. H11 only
shows that the valuable failure-prone subset can be made materially smaller
without losing the measured safety benefit in this run.

---

## Hypothesis Update - H11a Signal Density

The result suggests that semantic compression may not be only a token-reduction
technique.

Because score and reviewability improved after compression, a stronger research
hypothesis emerges:

```text
H11a: Semantic compression can improve enterprise signal density by removing
context noise while preserving authority, policy, evidence, and audit signals.
```

This reframes compression from:

```text
tokens down
```

to:

```text
enterprise meaning per token up
```

The likely mechanism is signal/noise improvement:

```text
full EKOS context = enterprise signal + useful evidence + excess context noise
compressed EKOS context = enterprise signal + essential evidence
```

This is not yet proven. It is the next research direction.

---

## Signal Density Candidate Metrics

Future work should define measurable signal-density metrics, such as:

```text
score per 1k tokens
reviewability per 1k tokens
safe successes per 1k tokens
authority-boundary hits per 1k tokens
policy-constraint hits per 1k tokens
evidence-reference hits per 1k tokens
max-safe correctness per 1k tokens
```

Candidate composite concept:

```text
Enterprise Meaning per Token
```

The goal is not to minimize tokens blindly. The goal is to maximize the amount
of correct, reviewable, authority-aware enterprise meaning delivered per token.

---

## Relationship To H10c

H10c found that EKOS is economically favorable only in a narrow condition:

```text
failure-prone policy/authority/delegation boundary tasks
```

H11 shows that this same condition is also the best place to compress first.

The right sequence is therefore:

```text
1. Identify where EKOS creates safety value.
2. Compress only the context needed for that value.
3. Measure whether safety is preserved and token cost falls.
```

---

## Limitations

- The experiment covers only 9 records.
- All 9 records are Gemini `gemini-2.5-flash`.
- Only CASE-012 and CASE-014 are represented.
- Full-context rows were reused from the previous Provider API Delta R3 artifact, while compressed rows were new calls.
- Provider-reported Gemini totals include provider-side accounting such as thinking tokens.
- Token accounting is not monetary-cost accounting.
- Human review time, escalation cost, and production workflow cost remain unmeasured.
- This does not test simple retrieval, simple reasoning, already-successful routine records, or true conflicting-evidence tasks.

---

## Rejected Interpretations

Rejected:

```text
EKOS proves ROI.
```

Reason:

This experiment measures provider-reported tokens and scorer outputs. It does
not measure provider pricing, human review time, escalation cost, incident
avoidance, production frequency, or adoption cost.

Rejected:

```text
EKOS is now universally cheaper.
```

Reason:

The earlier difficulty-conditioned result still shows EKOS is token-negative
for easy, medium, and already-successful records under current context design.

Rejected:

```text
Ultra compression is always safe.
```

Reason:

Ultra compression preserved safety on the evaluated 9 records only. It must be
treated as a candidate context shape, not as a universal EKOS compression rule.

---

## Claim Discipline

Allowed:

```text
In the evaluated 9 Gemini failure-prone boundary records, compressed EKOS reduced
tokens by 26.7% and ultra-compressed EKOS reduced tokens by 31.5% while
preserving 9/9 safe success, 9/9 max-safe correctness, 0 hard failures, and 0
over-delegation.
```

Allowed:

```text
The H11 result suggests that semantic compression may improve signal density,
not merely reduce tokens.
```

Not allowed:

```text
EKOS now proves ROI.
EKOS saves money in production.
EKOS compression works for all models.
EKOS compression works for all enterprise task classes.
Compressed EKOS is universally better than full EKOS.
```

---

## Hypothesis Status

H10a:

```text
Current full EKOS improves quality but increases first-turn token cost.
```

Status:

```text
still supported
```

H10b:

```text
Workflow-level repair-cost savings remain partially measured.
```

Status:

```text
unchanged; favorable on hard-failed subset, not full workflow-cost proof
```

H10c:

```text
EKOS is economically justified primarily for difficult or failure-prone
enterprise task classes.
```

Status:

```text
still supported only in narrowed form for evaluated failure-prone
policy/authority/delegation records
```

H11:

```text
EKOS can reduce context tokens while preserving the failure-prone safety benefit.
```

Status after this note:

```text
preliminarily supported for the evaluated 9 Gemini failure-prone
authority/policy/delegation records
```

---

## Minimum Next Research

Minimum useful next steps:

1. Repeat H11 on the same 9 records with a full-context rerun in the same batch
   to reduce temporal/provider drift between full and compressed conditions.
2. Run the same compressed variants on Anthropic only if a failure-prone subset
   exists in a future run; the current Anthropic R3 had no hard failures.
3. Test whether compressed EKOS remains safe on non-failed hard records before
   using it as the default EKOS context shape.
4. Open a follow-up signal-density research track.
5. Avoid expanding ETCB until the compression boundary is clear.

Signal-density follow-up question:

```text
How much enterprise meaning per token does EKOS provide, and which context fields
carry the most signal?
```

Minimum signal-density experiment:

```text
1. Decompose compressed EKOS context into authority, policy, evidence,
   relationship-path, process-state, and audit fields.
2. Measure marginal contribution per token.
3. Identify fields that are essential, compressible, or removable.
4. Produce an Enterprise Signal Density report.
```

Future sessions should preserve the current answer:

```text
H11 is positive but narrow. EKOS semantic compression preserved safety and
reduced tokens on the evaluated failure-prone boundary tasks. It does not prove
ROI, production cost savings, or universal cost savings.
```
