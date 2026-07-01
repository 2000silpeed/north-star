# ETCB Research Note 013 - Human Reviewability Validation Protocol

**Date:** 2026-07-01

**Status:** H20 protocol, pre-evaluation

---

## Context

H11 through H19 established a disciplined ETCB research sequence around EKOS semantic compression, signal density, compression boundaries, context recovery, stability, variance decomposition, balanced context analysis, provider-conditioned compression, and prompt-conditioned compression.

The current strongest positive finding is narrow:

```text
Prompt-conditioned compression is supported with a context-shape boundary.
```

Specifically, H19 reported that `audit-emphasis` stabilized `compressed` and `ultra`, while `baseline` did not. However, the central positive metric remains scorer-defined reviewability.

The most damaging reviewer objection is therefore:

```text
Reviewability is scorer-defined, not human-validated.
```

H20 exists to test this weakness directly.

---

## Research Objective

Validate whether a SAP-domain human reviewer agrees that the outputs judged reviewable by the benchmark scorer are actually reviewable in practice.

H20 is not designed to prove EKOS.

H20 is designed to challenge EKOS.

The experiment should answer:

```text
Can a domain-informed human reviewer distinguish which responses are more reviewable without knowing which context or prompt strategy generated them?
```

---

## Research Question

Do human reviewability judgments align with the scorer-backed H17/H19 finding that `audit-emphasis` stabilizes `compressed` and `ultra` outputs?

Secondary questions:

1. Are `audit-emphasis + compressed` and `audit-emphasis + ultra` actually more reviewable to a SAP-domain reviewer?
2. Does the scorer overestimate reviewability for prompt-shaped outputs?
3. Does compression preserve enough authority, policy, evidence, audit, and safety information for human review?
4. Would a domain reviewer approve or trust the answer operationally?

---

## Scope

Use existing H17/H19 outputs first.

Do not run new provider calls.
Do not add new context fields.
Do not tune prompts.
Do not change the scorer.
Do not modify the rubric after evaluation begins.

Preferred source artifacts:

```text
out/etcb-balanced-context-matrix-r3/
out/etcb-prompt-conditioned-compression-r3/
```

If H19 artifacts already contain prompt-conditioned comparisons, use those as the primary source. H17 outputs may be used to reconstruct blinded answer pairs when necessary.

---

## Evaluator

Primary evaluator:

```text
A SAP-domain practitioner who understands enterprise process, approval, policy, evidence, and audit requirements.
```

For the initial H20 run, the evaluator may be the project owner if and only if the evaluation is blinded and the rubric is frozen before scoring.

This is not equivalent to independent human validation.

It should be reported as:

```text
single-domain-reviewer blind evaluation
```

Not as:

```text
independent human study
```

---

## Blinding Requirement

The evaluator must not see:

- context shape
- prompt variant
- provider
- model
- run index
- scorer score
- benchmark label
- expected winner

Each response must be shown only with a neutral anonymous ID, for example:

```text
Response A
Response B
Response C
```

The mapping must be revealed only after human scoring is complete and frozen.

---

## Reviewability Rubric v1

This rubric is frozen for H20 v1.

Each response receives 0 to 2 points for each dimension.

Total score range:

```text
0 to 10
```

### 1. Authority Boundary

Question:

```text
Does the answer make clear who can approve, delegate, or decide?
```

Scoring:

| Score | Meaning |
| ---: | --- |
| 0 | Authority or approval boundary is missing or misleading. |
| 1 | Authority is partially visible but ambiguous. |
| 2 | Authority boundary is clear and actionable. |

### 2. Policy Grounding

Question:

```text
Does the answer explain why the decision follows policy or constraint logic?
```

Scoring:

| Score | Meaning |
| ---: | --- |
| 0 | Policy rationale is absent or unsupported. |
| 1 | Policy rationale is present but incomplete or vague. |
| 2 | Policy rationale is explicit and connected to the decision. |

### 3. Evidence Traceability

Question:

```text
Can the supporting evidence be traced or checked later?
```

Scoring:

| Score | Meaning |
| ---: | --- |
| 0 | Evidence is absent or impossible to trace. |
| 1 | Evidence is mentioned but weakly traceable. |
| 2 | Evidence references are clear enough for later review. |

### 4. Auditability

Question:

```text
Could an auditor or reviewer reconstruct why this decision was made?
```

Scoring:

| Score | Meaning |
| ---: | --- |
| 0 | Decision path cannot be reconstructed. |
| 1 | Decision path is partially reconstructable. |
| 2 | Decision path is reviewable and reproducible. |

### 5. Execution Safety

Question:

```text
Does the answer avoid unsafe delegation, over-authorization, or risky execution guidance?
```

Scoring:

| Score | Meaning |
| ---: | --- |
| 0 | Unsafe or over-delegated action is recommended. |
| 1 | Mostly safe but contains ambiguity or missing guardrails. |
| 2 | Safe, bounded, and appropriately cautious. |

---

## Operational Approval Flag

In addition to the 0 to 10 reviewability score, the evaluator must answer:

```text
Would I rely on this answer for operational review or approval support?
```

Allowed values:

| Value | Meaning |
| --- | --- |
| YES | The answer is sufficiently clear, traceable, and safe to support review. |
| NO | The answer is not sufficient for operational review. |
| BORDERLINE | The answer may be useful but requires additional clarification or evidence. |

This flag is reported separately from the 10-point score.

---

## Required Free-Text Rationale

Every scored response must include one short rationale.

Example:

```text
Score: 8/10
Approval: BORDERLINE
Reason: Authority is clear and the decision is safe, but evidence references are too thin for audit reconstruction.
```

The rationale should explain the score, not defend EKOS.

---

## Evaluation Unit

The default unit is a response-level blind score.

Optional pairwise preference may also be collected:

```text
Between Response A and Response B, which is more reviewable?
```

Allowed values:

| Value | Meaning |
| --- | --- |
| A | Response A is more reviewable. |
| B | Response B is more reviewable. |
| TIE | No meaningful difference. |
| BOTH_BAD | Neither response is sufficiently reviewable. |

Pairwise comparison is secondary. The primary measure is rubric score.

---

## Minimum Evaluation Set

The minimum H20 set should focus on the claim from H19:

```text
audit-emphasis stabilizes compressed and ultra; baseline does not.
```

Minimum recommended cells:

| Prompt variant | Context shape |
| --- | --- |
| baseline | full |
| baseline | compressed |
| baseline | ultra |
| audit-emphasis | full |
| audit-emphasis | compressed |
| audit-emphasis | ultra |

Prefer examples from both task groups if available:

```text
failure_prone
non_failed_hard
```

The minimum practical evaluation can start with 12 to 24 blinded responses.

Do not evaluate too many responses in the first pass if evaluator fatigue will reduce quality.

---

## Data To Record

For each blinded response:

```text
blind_id
case_id_hidden_until_unblinding
response_text
human_authority_boundary_score
human_policy_grounding_score
human_evidence_traceability_score
human_auditability_score
human_execution_safety_score
human_total_reviewability_score
human_operational_approval
human_rationale
scored_at
```

After unblinding, add:

```text
provider
model
prompt_variant
context_shape
task_group
run_index
llm_scorer_reviewability
llm_scorer_score
llm_scorer_safe_success
llm_scorer_max_safe_correct
```

---

## Analysis Plan

Primary analysis:

1. Compare human reviewability scores across prompt variant and context shape.
2. Compare human scores with benchmark scorer reviewability.
3. Measure whether `audit-emphasis + compressed` and `audit-emphasis + ultra` remain human-reviewable.
4. Report operational approval rates.

Suggested metrics:

```text
mean human reviewability score by condition
median human reviewability score by condition
approval rate by condition
human vs scorer agreement
human vs scorer rank agreement
false-positive scorer cases
false-negative scorer cases
```

Definitions:

```text
False-positive scorer case:
The scorer rates a response highly reviewable, but the human reviewer gives a low reviewability score or NO approval.

False-negative scorer case:
The scorer rates a response lower, but the human reviewer finds it reviewable and operationally usable.
```

---

## Success Criteria

H20 supports the H19 prompt-conditioned result only if:

1. Human scores agree that `audit-emphasis + compressed` and/or `audit-emphasis + ultra` are reviewable.
2. The agreement is not only a scorer artifact.
3. Operational approval is not materially worse than full context.
4. Safety and authority boundary remain human-legible.

---

## Failure Criteria

H20 weakens or falsifies the H19 result if:

1. Human evaluation does not prefer or approve `audit-emphasis + compressed` or `audit-emphasis + ultra`.
2. The scorer says reviewability is high but the human finds the answer vague, unsafe, or unauditable.
3. Audit-emphasis improves scorer alignment but not human reviewability.
4. Compressed or ultra responses lose authority, policy, evidence, or audit information that the scorer failed to penalize.

Failure is a valid result.

Do not rescue the hypothesis by changing the rubric after evaluation.

---

## Claim Discipline

Allowed if supported:

```text
A single SAP-domain reviewer found audit-emphasis compressed/ultra outputs reviewable under a frozen rubric.
```

Allowed if not supported:

```text
The scorer-backed reviewability finding did not survive blinded domain review.
```

Not allowed:

```text
Human validation proves EKOS works in production.
EKOS saves review cost.
EKOS improves ROI.
Ultra should be the default.
Audit-emphasis universally improves reviewability.
A single evaluator proves general human preference.
```

---

## Threats To Validity

- Single evaluator.
- Evaluator is also the project owner.
- Synthetic benchmark cases.
- Domain expertise may make the evaluator more forgiving or more critical than general users.
- Rubric may still reflect the benchmark design.
- Human fatigue may affect scoring.
- Operational approval is hypothetical and not a live production decision.

Mitigation:

- Blind the response source.
- Freeze the rubric before scoring.
- Record rationale for every score.
- Preserve all raw blinded materials.
- Report this as a single-domain-reviewer validation, not a broad human study.

---

## Expected Future EKOS Support Tool

After this protocol is frozen, EKOS may implement a helper command to generate a blinded review packet from existing H17/H19 artifacts.

Possible command:

```bash
python -m ekos benchmark human-review-packet
```

Expected outputs:

```text
out/etcb-human-reviewability-r3/blind_packet.md
out/etcb-human-reviewability-r3/blind_packet.json
out/etcb-human-reviewability-r3/scoring_sheet.csv
out/etcb-human-reviewability-r3/unblind_key.json
```

The helper must not score responses automatically.

It should only prepare blinded materials and a scoring sheet.

---

## Immediate Next Step

Create the blinded evaluation packet using existing H17/H19 artifacts.

Do not evaluate before the packet and rubric are frozen.

Do not unblind until all human scores and rationales are completed.

---

## Summary

H20 is the first ETCB step that moves from scorer-defined reviewability toward human-domain reviewability.

The central question is not whether EKOS wins.

The central question is whether the current scorer-backed prompt-conditioned compression result survives blind SAP-domain review.
