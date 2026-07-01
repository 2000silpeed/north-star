# ETCB Research Note 005 - Signal Density

**Date:** 2026-07-01

**Status:** Preliminary H12 evidence

---

## Context

ETCB Research Note 004 produced the first positive H11 semantic-compression
result on the evaluated failure-prone subset:

```text
Full EKOS: 35,827 tokens, 9/9 safe success
Compressed EKOS: 26,245 tokens, 9/9 safe success
Ultra-compressed EKOS: 24,554 tokens, 9/9 safe success
```

The important surprise was not only token reduction. Score and reviewability
also improved:

```text
Score: 174 -> 180
Reviewability: 102 -> 108
```

That raised H12:

```text
How much enterprise meaning per token does EKOS provide, and which context
fields carry the most signal?
```

---

## Research Decision

Implement H12 as a narrow post-hoc analysis over the existing H11 artifact:

```text
out/etcb-semantic-compression-r3/
```

Do not create a broad new benchmark family. Do not make new provider calls. Do
not optimize prompts to make EKOS look cheaper.

Implementation repository:

```text
2000silpeed/ekos-sap-knowledge-os
```

Implemented command:

```bash
python -m ekos benchmark signal-density \
  --source out/etcb-semantic-compression-r3 \
  --out out/etcb-signal-density-r3
```

Generated EKOS artifacts:

```text
out/etcb-signal-density-r3/summary.md
out/etcb-signal-density-r3/signal_density.json
out/etcb-signal-density-r3/signal_density.csv
```

No field-level provider variants were added in this sprint. The reason is
methodological: H11 already provides the minimum full/compressed/ultra contrast
needed to answer the H12 density question, while prior broad ablation evidence
did not show clear single-field causality. Adding new field variants now would
expand the experiment before the signal-density baseline is recorded.

---

## Method

The analysis uses the same 9 H11 records:

```text
Provider/model: Gemini gemini-2.5-flash
Cases: CASE-012 and CASE-014
Task class: failure-prone authority/policy/delegation boundary records
```

Metrics computed per variant:

```text
score per 1k tokens
reviewability per 1k tokens
safe success per 1k tokens
max-safe correctness per 1k tokens
authority-boundary preservation per 1k tokens
policy-constraint preservation per 1k tokens
evidence-reference preservation per 1k tokens
tokens per safe success
```

Authority, policy, and evidence preservation are computed from existing EDB
rubric points per 1k provider-reported total tokens. Perfect-record densities
are also exported in JSON/CSV for auditability.

---

## Variant Result

| Variant | Tokens | Safe success | Score/1k | Reviewability/1k | Safe success/1k | Max-safe/1k | Authority pts/1k | Policy pts/1k | Evidence pts/1k | Tokens/safe |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| full | 35,827 | 9/9 | 4.857 | 2.847 | 0.251 | 0.251 | 2.010 | 0.502 | 0.502 | 3,980.778 |
| compressed | 26,245 | 9/9 | 6.858 | 4.115 | 0.343 | 0.343 | 2.743 | 0.686 | 0.686 | 2,916.111 |
| ultra | 24,554 | 9/9 | 7.331 | 4.398 | 0.367 | 0.367 | 2.932 | 0.733 | 0.733 | 2,728.222 |

Token reduction versus full EKOS:

| Variant | Total-token reduction |
| --- | ---: |
| compressed | 9,582 tokens, 26.7% |
| ultra | 11,273 tokens, 31.5% |

---

## Answer To H12

For the evaluated 9 Gemini failure-prone authority/policy/delegation records:

```text
Compression improved signal density, not merely token count.
```

The evidence:

- compressed and ultra preserved 9/9 safe success
- compressed and ultra preserved 9/9 max-safe correctness
- compressed and ultra produced 0 hard failures
- compressed and ultra produced 0 over-delegations
- score per 1k tokens increased from 4.857 to 6.858 and 7.331
- reviewability per 1k tokens increased from 2.847 to 4.115 and 4.398
- authority, policy, and evidence preservation per 1k tokens all increased

This supports H11a/H12 in a narrow form:

```text
Semantic compression can improve enterprise signal density by removing context
noise while preserving authority, policy, evidence, max-safe, and audit signals
on the evaluated failure-prone boundary records.
```

---

## Field Classification

| Field | Classification | Evidence | Caveat |
| --- | --- | --- | --- |
| authority boundary | essential | Retained in compressed and ultra contexts; authority-boundary rubric points stayed perfect. | Preservation-supported, not independent causal ablation. |
| policy constraint | essential | Policy references were retained; policy-to-authority coverage stayed perfect. | Prior broad ablation did not isolate a large policy-only drop. |
| evidence reference | essential | Evidence references were retained; evidence-to-authority coverage stayed perfect. | Verbose evidence prose is compressible, but exact evidence anchoring was not removed. |
| audit/reviewability cue | essential | Short audit signal survived compression; audit-note usefulness became perfect in compressed/ultra. | Long audit wording is compressible. |
| process state | compressible | Expanded process-state narrative was shortened or omitted while safety was preserved. | The base case packet still contains process context. |
| relationship path | removable | Expanded relationship-path objects were absent from compressed/ultra while safety and reviewability were preserved. | Removable only under these records where boundary/evidence/policy refs remain explicit. |
| context field source labels | removable | Long provenance labels were absent while safety was preserved. | This applies to prompt text labels, not durable EKOS audit provenance. |
| verbose context bundle | harmful/noisy | Full context had lower score and reviewability density than compressed variants. | Bundle-level inference; no single harmful field was isolated. |
| max-safe decision hint | unknown | Max-safe correctness stayed perfect, but exact `maximum_safe_delegation_level` was intentionally excluded by anti-leakage rules. | The tested signal is authority-boundary context, not an explicit answer hint. |

---

## Interpretation

H12 strengthens H11 because the compressed variants did not merely reduce token
count. They produced more scored enterprise meaning per token.

The likely mechanism is:

```text
full EKOS context = enterprise signal + useful context + excess context noise
compressed EKOS context = enterprise signal + essential boundary/evidence/policy cues
```

This mechanism is plausible, not fully proven. H12 does not identify one
specific harmful field. It shows that the full context bundle is less dense
than the compressed bundle for these records.

---

## Claim Discipline

Allowed:

```text
In the evaluated 9 Gemini failure-prone authority/policy/delegation records,
compressed and ultra-compressed EKOS improved signal density while preserving
9/9 safe success and 9/9 max-safe correctness.
```

Allowed:

```text
Authority boundary, policy refs, evidence refs, and audit cues appear essential
for the compressed context shape in this run.
```

Not allowed:

```text
EKOS proves ROI.
EKOS saves production cost.
Compressed EKOS works for all models.
Compressed EKOS works for all enterprise task classes.
Relationship paths are always useless.
Process state is always removable.
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

H10c:

```text
EKOS is economically justified primarily for difficult or failure-prone task
classes.
```

Status:

```text
still supported only in narrowed form for evaluated failure-prone
policy/authority/delegation boundary records
```

H11:

```text
EKOS can reduce context tokens while preserving the failure-prone safety benefit.
```

Status:

```text
supported preliminarily for the evaluated 9 Gemini failure-prone records
```

H12:

```text
Semantic compression can improve enterprise signal density, not merely reduce
tokens.
```

Status:

```text
supported preliminarily for the same evaluated 9 records
```

---

## Limitations

- Only 9 records were analyzed.
- All 9 records are Gemini `gemini-2.5-flash`.
- Only CASE-012 and CASE-014 are represented.
- The result is post-hoc analysis over H11 artifacts, not a new provider run.
- Field classification is inferred from preserved and omitted fields across
  H11 variants, not independent causal ablation.
- Provider-reported tokens are not monetary cost.
- Human review time, escalation cost, production incident cost, and adoption
  cost remain unmeasured.
- This does not test simple retrieval, simple reasoning, already-successful
  routine records, Anthropic failure-prone records, or true conflicting-evidence
  tasks.

---

## Minimum Next Research

Do not expand into a broad benchmark family.

The next useful step is one of:

1. Repeat H11/H12 with a same-batch full rerun to reduce provider-time drift.
2. Test compressed EKOS on non-failed hard records to find where compression
   starts losing reviewability or safety.
3. Run a small targeted field test only for the uncertain boundary:
   relationship/process-state removal when evidence conflicts or process state
   is under-specified.

Future sessions should preserve the current answer:

```text
H12 is positive but narrow. Compressed EKOS improved enterprise signal density
on evaluated failure-prone boundary tasks. It does not prove ROI, production
cost savings, universal model coverage, or universal task-class coverage.
```
