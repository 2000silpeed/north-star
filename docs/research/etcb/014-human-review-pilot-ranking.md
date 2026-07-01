# ETCB Research Note 014 - H20 Pilot Human Review Ranking Observation

**Date:** 2026-07-01

**Status:** H20 pilot evidence, post-protocol observation

---

## Context

H20 was defined in `docs/research/etcb/013-human-reviewability-protocol.md`
to test whether scorer-defined reviewability survives blinded SAP-domain human
review.

The protocol froze a 0 to 10 reviewability rubric before evaluation:

- authority boundary
- policy grounding
- evidence traceability
- auditability
- execution safety

After the first packet-generation attempt, North Star Issue #1 clarified that
blind evaluation must hide generation metadata, not the business task itself.
The EKOS implementation was then revised so each blinded scenario presents:

```text
Business Context
Enterprise Task
Question Being Answered
Response A
Response B
Response C
Response D
```

while continuing to hide:

- provider
- model
- prompt variant
- context shape
- benchmark labels
- run index
- scorer outputs

This note records the first H20 pilot review observation. It does not modify
the frozen H20 protocol or rubric.

---

## Pilot Scope

Pilot unit:

```text
Scenario 01
```

Reviewer:

```text
SAP domain practitioner
Single blinded evaluator
```

The evaluator reviewed the visible business context, enterprise task, question,
and four blinded responses.

The evaluator did not see generation metadata.

---

## Pilot Ranking

The reviewer produced this ordinal ranking:

| Rank | Response |
| ---: | --- |
| 1 | A |
| 2 | D |
| 3 | C |
| 4 | B |

This ranking is evidence about one blinded scenario only. It is not yet a
general H20 result.

---

## Reviewer Rationale

The reviewer preferred responses with:

- clear authority boundaries
- explicit policy grounding
- strong evidence linkage
- coherent reasoning flow

The reviewer did not treat concision as inherently improving reviewability.

Important qualitative observation:

```text
Conciseness alone did not improve reviewability.
```

This matters because compressed context can sometimes produce shorter answers.
The pilot suggests that shorter output is not automatically more reviewable to
a SAP-domain reviewer unless authority, policy, evidence, and reasoning remain
clear.

---

## Methodological Observation

The reviewer found ordinal ranking significantly easier and more reliable than
assigning absolute rubric scores.

This is a material protocol-design finding:

```text
Ranking may be the more reliable primary human input for the first human study.
```

Absolute rubric scores may still be useful, but the pilot suggests they should
be treated as secondary or derived measures until the scoring process is tested
for reviewer confidence and consistency.

---

## Interpretation

The pilot does not invalidate H20.

It identifies a practical measurement issue:

```text
Human reviewers may be better at comparative reviewability judgment than at
absolute reviewability calibration on a frozen 0 to 10 scale.
```

This does not mean the rubric is wrong. The rubric still describes the
dimensions that matter. The issue is whether the first human study should ask
the reviewer to express judgment primarily as:

```text
ordinal preference among blinded responses
```

rather than:

```text
absolute numeric score per response
```

The strongest current reading is:

```text
Use the rubric dimensions to guide rationale, but collect ranking as the primary
pilot signal.
```

---

## What This Does Not Show

This pilot does not show:

- that Response A is globally best
- that any provider, prompt variant, or context shape is better
- that H19 is supported or falsified
- that the scorer is aligned or misaligned with humans
- that ordinal ranking should permanently replace rubric scoring

The unblind key should not be interpreted until the planned human scoring or
ranking workflow is complete and frozen.

---

## Implication For Future H20 Refinement

Future refinement should consider collecting:

1. ordinal ranking
2. qualitative rationale
3. optional rubric scores as secondary measures

This would preserve the original H20 intent while reducing reviewer burden and
improving reviewer confidence.

Possible future data shape:

```text
scenario_id
blind_id
ordinal_rank
reviewer_rationale
authority_boundary_score_optional
policy_grounding_score_optional
evidence_traceability_score_optional
auditability_score_optional
execution_safety_score_optional
operational_approval_optional
scored_or_ranked_at
```

This is a future protocol-refinement candidate, not a retroactive change to
H20 v1.

---

## Open Questions

1. Should H20 v1 continue collecting absolute scores for all responses, or
   should the first study be explicitly reframed as ordinal-ranking-first?
2. If ranking becomes primary, should pairwise comparisons also be collected,
   or is a full scenario-level ranking enough?
3. How should ordinal ranking be compared against the LLM scorer's
   reviewability values without overclaiming statistical significance?
4. Should reviewer rationale be coded against the existing five rubric
   dimensions after the review, or should the reviewer assign dimension scores
   directly?
5. Does ranking remain easier when the packet includes more scenarios, or does
   fatigue affect ranking as much as scoring?

---

## Context For Future AI Collaborators

Do not treat H20 as completed from this pilot.

Do not modify `013-human-reviewability-protocol.md` retroactively.

Do preserve the methodological lesson:

```text
The first human review suggests that comparative ranking may be a better
primary measurement interface than absolute rubric scoring.
```

Future work should keep H20 falsifiable. A ranking-first design can still
weaken or falsify the scorer-backed H19 finding if humans prefer responses that
do not align with the scorer-supported conditions after unblinding.
