# ETCB Research Note 006 - Compression Boundary

**Date:** 2026-07-01

**Status:** Preliminary H13 evidence

---

## Context

ETCB Research Note 004 found that semantic compression preserved the EKOS
safety benefit on the evaluated 9 Gemini failure-prone authority/policy/
delegation records:

```text
Full EKOS: 35,827 tokens, 9/9 safe success
Compressed EKOS: 26,245 tokens, 9/9 safe success
Ultra-compressed EKOS: 24,554 tokens, 9/9 safe success
```

ETCB Research Note 005 then found that the compressed variants improved signal
density on that same narrow set. The unresolved question was where compression
stops being safe or reviewable.

H13 asks:

```text
At what boundary does compressed EKOS stop being safe or reviewable?
```

This note records the minimum H13 result. It does not introduce a broad new
benchmark family and does not claim ROI, production cost savings, or universal
compression safety.

---

## Research Decision

Implement H13 in the EKOS implementation repository and preserve the result in
North Star.

Implementation repository:

```text
2000silpeed/ekos-sap-knowledge-os
```

EKOS implementation commit:

```text
0dc630e feat: add ETCB compression boundary benchmark
```

Implemented command:

```bash
python -m ekos benchmark compression-boundary \
  --models models/live-models.local.json \
  --source-provider-delta out/edb-provider-api-delta-anthropic-gemini-r3-2026-07-01 \
  --source-difficulty out/etcb-difficulty-conditioned-analysis/difficulty_conditioned_cost.json \
  --max-failure-prone 9 \
  --max-hard 9 \
  --stress-runs 1 \
  --out out/etcb-compression-boundary-r3
```

Generated EKOS artifacts:

```text
out/etcb-compression-boundary-r3/summary.md
out/etcb-compression-boundary-r3/compression_boundary.json
out/etcb-compression-boundary-r3/compression_boundary.csv
out/etcb-compression-boundary-r3/field_boundary_matrix.csv
```

The run produced 66 provider records:

```text
22 targets x 3 variants = 66 records
```

The report recorded zero parse failures and zero runner errors.

---

## Method

H13 reused the existing EDB `DelegationDecisionContract` schema, scorer,
provider adapters, and H11 full/compressed/ultra context shapes.

The experiment compared:

```text
full
compressed
ultra
```

Target groups:

| Group | Records | Purpose |
| --- | ---: | --- |
| failure_prone_same_batch | 9 | Same-batch rerun of the H11 failure-prone target set. |
| non_failed_hard | 9 | Hard records where model-only and EKOS both previously succeeded. |
| stress:conflicting-evidence | 1 | Capability evidence conflicts with authority/policy constraint. |
| stress:stale-vs-current-evidence | 1 | Current status conflicts with stale external evidence. |
| stress:process-state-transition | 1 | Process state permits preparation but not execution. |
| stress:relationship-path-disambiguation | 1 | Relationship path disambiguates capability from approval authority. |

The stress cases reused existing CASE-011 through CASE-014 delegation packets
with targeted stress overlays. This makes them boundary probes, not a new broad
benchmark family.

Metrics:

```text
safe success
max-safe correctness
hard failures
over-delegation
reviewability
score per 1k tokens
tokens per safe success
field-boundary status
```

---

## Boundary Result

| Group | Variant | Tokens | Safe success | Max-safe | Hard fails | Over-delegation | Score | Reviewability | Status vs full |
| --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| failure_prone_same_batch | full | 36,608 | 9/9 | 9/9 | 0 | 0 | 177 | 105 | baseline |
| failure_prone_same_batch | compressed | 26,617 | 9/9 | 9/9 | 0 | 0 | 180 | 108 | wins |
| failure_prone_same_batch | ultra | 23,643 | 9/9 | 9/9 | 0 | 0 | 178 | 106 | wins |
| non_failed_hard | full | 35,525 | 9/9 | 9/9 | 0 | 0 | 178 | 106 | baseline |
| non_failed_hard | compressed | 31,733 | 9/9 | 9/9 | 0 | 0 | 173 | 101 | loses reviewability |
| non_failed_hard | ultra | 28,038 | 9/9 | 9/9 | 0 | 0 | 174 | 102 | loses reviewability |

Stress overlays:

| Stress group | Compressed status | Ultra status | Interpretation |
| --- | --- | --- | --- |
| conflicting evidence | wins | wins | Safety and reviewability preserved while reducing tokens. |
| stale vs current evidence | wins | wins | Safety and reviewability preserved while reducing tokens. |
| process-state transition | wins | wins | Safety and reviewability preserved while reducing tokens. |
| relationship-path disambiguation | wins | wins | Safety and reviewability preserved while reducing tokens. |

Token reduction versus full:

| Group | Variant | Reduction |
| --- | --- | ---: |
| failure_prone_same_batch | compressed | 9,991 tokens, 27.3% |
| failure_prone_same_batch | ultra | 12,965 tokens, 35.4% |
| non_failed_hard | compressed | 3,792 tokens, 10.7% |
| non_failed_hard | ultra | 7,487 tokens, 21.1% |

---

## Answer To H13

H13 found a compression boundary:

```text
Compressed and ultra-compressed EKOS preserved safety on all evaluated groups,
but both lost reviewability on the non-failed hard group.
```

The boundary was not a safety failure in this run:

```text
safe success stayed 9/9
max-safe correctness stayed 9/9
hard failures stayed 0
over-delegation stayed 0
```

The boundary was reviewability:

```text
non_failed_hard reviewability:
full = 106
compressed = 101
ultra = 102
```

For the same-batch failure-prone records, H11 is strengthened:

```text
full = 36,608 tokens, 9/9 safe success
compressed = 26,617 tokens, 9/9 safe success
ultra = 23,643 tokens, 9/9 safe success
```

For the targeted stress overlays, H13 did not find a boundary:

```text
all compressed and ultra stress variants preserved safety and reviewability
in this one-run targeted stress set
```

This does not prove that compression is safe for all stress cases. It only
shows that these four targeted probes did not break the compressed variants.

---

## Field Boundary Interpretation

No field became essential under the targeted stress overlays in this run
because the stress overlays did not expose safety or reviewability loss.

The field that did become boundary-relevant was:

| Field | Boundary status | Evidence |
| --- | --- | --- |
| process_state | essential_for_reviewability_in_group | Ultra omitted expanded process-state detail and lost reviewability on non_failed_hard. |

Retained core signals across winning compressed groups:

```text
authority_boundary
policy_constraint
evidence_reference
audit_reviewability_cue
blocking_risk_codes
reversibility
approval_boundary
```

Fields that appeared compressible or removable in the evaluated stress probes:

```text
relationship_path
process_state
context_field_sources
verbose evidence/policy prose
```

The phrase "compressible or removable" remains conditional. It means the field
was not necessary in this specific group when authority, policy, evidence,
approval, reversibility, and audit signals remained explicit. It does not mean
the field is globally unnecessary.

---

## Hypothesis Update

H11:

```text
EKOS can reduce context tokens while preserving the failure-prone safety benefit.
```

Status:

```text
strengthened for the same-batch 9 Gemini failure-prone records
```

H12:

```text
Semantic compression can improve enterprise signal density, not merely reduce
tokens.
```

Status:

```text
still supported for the failure-prone set, but not sufficient as a universal
default-context argument
```

H13:

```text
Compressed EKOS stops being cleanly favorable when reviewability depends on
process-state detail that the compressed shape removes or weakens.
```

Status:

```text
preliminarily supported by the non_failed_hard reviewability loss
```

The important correction to H12 is:

```text
process_state is not simply compressible.
It appears conditionally compressible for failure-prone and targeted stress
records, but reviewability-relevant for non-failed hard records.
```

---

## Rejected Interpretations

Rejected:

```text
Compressed EKOS is universally better than full EKOS.
```

Reason:

Compressed and ultra variants lost reviewability on non-failed hard records.

Rejected:

```text
Ultra compression is safe by default.
```

Reason:

Ultra preserved safety in this run, but the reviewability loss shows that
smaller context can remove useful enterprise explanation.

Rejected:

```text
Relationship paths and process state are always removable.
```

Reason:

Relationship paths were compressible in the targeted probes, but that is not
global causal proof. Process state became reviewability-relevant on the
non-failed hard group.

Rejected:

```text
This proves ROI or production cost savings.
```

Reason:

The experiment measures provider-reported tokens and scorer outputs. It does
not measure provider pricing, human review time, escalation cost, incident
avoidance, production frequency, or adoption cost.

---

## Limitations

- The failure-prone group remains Gemini `gemini-2.5-flash` only.
- The targeted stress overlays are one-run probes, not a broad stress suite.
- The non-failed hard group is small and concentrated in existing CASE-011
  records.
- Field-boundary labels are inferred from full/compressed/ultra context shapes,
  not independent causal ablations for every field.
- Provider-reported token counts are not monetary cost.
- The result does not cover simple retrieval, simple reasoning, all enterprise
  task classes, or production workflows.

---

## Claim Discipline

Allowed:

```text
In the evaluated same-batch 9 Gemini failure-prone authority/policy/delegation
records, compressed and ultra-compressed EKOS preserved 9/9 safe success while
reducing provider-reported tokens by 27.3% and 35.4%.
```

Allowed:

```text
H13 found a reviewability boundary on non-failed hard records: compressed and
ultra variants preserved safety but lost reviewability relative to full EKOS.
```

Allowed:

```text
Process-state detail appears conditionally essential for reviewability in the
non-failed hard group.
```

Not allowed:

```text
EKOS proves ROI.
EKOS saves production cost.
Compressed EKOS works for all models.
Compressed EKOS works for all task classes.
Ultra compression should become the default.
Process state is always removable.
```

---

## Minimum Next Research

Do not expand ETCB into a broad benchmark family.

The next minimal experiment should test only the boundary H13 found:

```text
Can a compressed-plus-process-state EKOS context recover non_failed_hard
reviewability while keeping most of the token reduction?
```

That follow-up should remain limited to:

```text
non_failed_hard records
process-state detail
full vs compressed vs compressed+process-state
same scorer and schema
```

Future sessions should preserve the current answer:

```text
H13 supports compressed EKOS for the evaluated failure-prone boundary tasks,
but finds a reviewability boundary on non-failed hard records. Compression
should be conditional, not universal.
```
