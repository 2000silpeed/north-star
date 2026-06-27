# AIOS ADR Rules

Version: 1.0

Status: Draft

## Purpose
ADRs preserve decisions that future collaborators may question.

They are not only records of what was chosen. They must explain why alternatives were rejected.

## When to Create an ADR
Create an ADR when a decision affects:

- Repository responsibility
- Architecture direction
- EKOS positioning
- Evaluation philosophy
- Public claims
- Long-term AI behavior
- Cross-repository boundaries
- Implementation constraints

## When Not to Create an ADR
Do not create an ADR for:

- Minor wording edits
- Temporary notes
- Local formatting changes
- Work that does not constrain future decisions

Use a diary entry or research note instead.

## ADR Required Structure
Use this structure:

```text
# ADR-NNNN - Title

Date:
Status:

## Context
## Decision
## Consequences
## Rejected Alternatives
## Open Questions
## Next Actions
```

Open Questions may be omitted only when there are genuinely none.

## Decision Quality Standard
A good ADR answers:

- Why did this decision arise?
- What was chosen?
- What was not chosen?
- What trade-offs were accepted?
- What risks remain?
- What should future AI systems remember?

## Options Rule
If multiple architectural directions exist, document the options before deciding.

Minimum structure:

- Option A
- Option B
- Option C, if relevant
- Trade-offs
- Recommendation
- Why

## Rejection Rule
Rejected alternatives are first-class reasoning.

Do not delete them to make the decision look obvious.

Future AI systems need to understand why a tempting path was not taken.

## Repository Boundary ADRs
Any decision that changes which repository owns a responsibility must be recorded in an ADR.

Default boundary:

- North Star owns research, strategy, ADRs, glossary, evaluation criteria, and public narrative.
- `2000silpeed/ekos-sap-knowledge-os` owns implementation.

## ADR Lifecycle
Statuses:

- Proposed
- Accepted
- Superseded
- Deprecated

If an ADR changes, prefer creating a new ADR that supersedes the old one instead of rewriting history.
