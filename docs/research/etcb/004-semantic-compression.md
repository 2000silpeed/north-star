# ETCB Research Note 004 — Semantic Compression

**Date:** 2026-07-01

**Status:** Preliminary H11 evidence

---

## Context

ETCB first-turn token-cost evidence showed that current EKOS improves quality and
safety but increases first-turn token cost.

ETCB workflow repair-cost evidence then showed a narrow favorable condition:
for the 9 Gemini model-only hard-failed records, model-only plus one repair used
more tokens and still failed, while EKOS completed all 9 safely.

Difficulty-conditioned ETCB narrowed the economic claim further:

```text
EKOS is economically favorable only for evaluated failure-prone
policy/authority/delegation boundary tasks.
```

H11 asks whether EKOS can keep that failure-prone safety benefit while reducing
context tokens.

---

## Research Decision

Start H11 Semantic Compression from the highest-signal subset rather than the
whole workload.

Target subset:

```text
9 Gemini failure-prone authority/policy/delegation records
```

Reason:

```text
This is the subset where EKOS already dominates model-only plus one repair.
```

Implementation artifacts were produced in the EKOS implementation repository:

```text
out/etcb-semantic-compression-r3/
```

Generated files:

```text
summary.md
semantic_compression_delta.json
semantic_compression_delta.csv
```

---

## H11 — Semantic Compression

H11:

```text
EKOS can preserve its failure-prone safety benefit while reducing context tokens.
```

This is not a universal cost-savings hypothesis. It is a targeted compression
hypothesis over the evaluated failure-prone boundary records.

---

## Method

Compare three EKOS context variants on the 9 failure-prone records:

```text
1. Full EKOS context
2. Compressed EKOS context
3. Ultra-compressed EKOS context
```

Compression must preserve:

```text
authority boundary
policy constraint
evidence references
max-safe delegation decision
auditability / reviewability signal
```

The target is not merely shorter text. The target is to preserve enterprise
meaning while removing unnecessary context volume.

---

## Result

| Variant | Tokens | Safe success | Token delta vs full | Token reduction | Max-safe correctness | Hard failures | Over-delegation |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Full EKOS | 35,827 | 9/9 | — | — | 9/9 | 0 | 0 |
| Compressed EKOS | 26,245 | 9/9 | -9,582 | -26.7% | 9/9 | 0 | 0 |
| Ultra-compressed EKOS | 24,554 | 9/9 | -11,273 | -31.5% | 9/9 | 0 | 0 |

Score and reviewability also improved in this run:

| Variant | Score | Reviewability |
| --- | ---: | ---: |
| Full EKOS | 174 | 102 |
| Compressed / Ultra result | 180 | 108 |

The important result is:

```text
Quality and safety were preserved while tokens decreased.
```

This is the first ETCB result where EKOS shows a favorable economic direction on
the targeted failure-prone subset:

```text
quality/safety maintained or improved
context tokens decreased
```

---

## Interpretation

H11 preliminary interpretation:

```text
positive for the evaluated 9 Gemini failure-prone authority/policy/delegation
records.
```

This result does not support:

```text
ROI
production cost savings
universal EKOS cost savings
all-model generalization
all-task generalization
```

It supports a narrower claim:

```text
For the evaluated failure-prone boundary records, semantic compression reduced
EKOS context cost while preserving safe completion.
```

---

## Hypothesis Update — H11a Signal Density

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

The goal is not to minimize tokens blindly. The goal is to maximize the amount of
correct, reviewable, authority-aware enterprise meaning delivered per token.

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

## Minimum Next Research

Open a follow-up research track for signal density.

Next question:

```text
How much enterprise meaning per token does EKOS provide, and which context fields
carry the most signal?
```

Minimum experiment:

```text
1. Decompose compressed EKOS context into authority, policy, evidence,
   relationship-path, process-state, and audit fields.
2. Measure marginal contribution per token.
3. Identify fields that are essential, compressible, or removable.
4. Produce an Enterprise Signal Density report.
```

The next research target is not just smaller prompts. It is better enterprise
signal engineering.
