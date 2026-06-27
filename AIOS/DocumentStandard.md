# AIOS Document Standard

Version: 1.0

Status: Draft

## Purpose
This standard defines document types used by North Star.

Documents are not deliverables for their own sake. They are memory structures for future reasoning.

## Universal Requirements
Every durable document should make clear:

- Purpose
- Scope
- Status
- Context
- Decision or hypothesis
- Reasoning
- Open questions
- Next action

## Research Note
Use for emerging insights, conceptual shifts, exploratory reasoning, and early architecture ideas.

Location:

```text
docs/research/
```

Required sections:

- Context
- Original discussion and reasoning
- Initial assumptions
- Competing ideas
- Turning points
- Final decision or current hypothesis
- Rejected alternatives
- Open questions
- Context for future AI collaborators
- Next research

## ADR
Use when a decision should constrain future work.

Location:

```text
docs/adr/
```

Required sections:

- Context
- Decision
- Consequences
- Rejected alternatives
- Open questions, when relevant
- Next actions

## White Paper
Use for public-facing thesis and architecture explanation.

Location:

```text
docs/whitepaper/
```

Standard:

- Reader-facing sections should be clear and durable.
- Supporting reasoning should remain in section drafts, ADRs, and research notes.
- Claims must not imply production readiness without evidence.
- Implementation references must point to `2000silpeed/ekos-sap-knowledge-os`.

## Glossary
Use for stable concept definitions.

Location:

```text
docs/glossary/
```

Required fields:

- Definition
- Why it matters
- Example
- Non-example
- Implementation repository use

Glossary entries must not include implementation code.

## Evaluation Criteria
Use for defining how EKOS claims will be judged.

Location:

```text
docs/whitepaper/
docs/research/
```

Required fields:

- Claim being evaluated
- Baseline
- Evaluation dimension
- Scoring criteria
- Failure modes
- Limitations

Runnable evaluation harnesses belong in `2000silpeed/ekos-sap-knowledge-os`.

## Diary
Use for session continuity.

Location:

```text
docs/diary/
```

Should include:

- What changed
- Why it changed
- New artifacts
- Current next task
- Important repository boundary reminders

## Meeting or Collaboration Note
Use when multiple agents or humans contribute to a direction.

Recommended sections:

- Participants or sources
- Context
- Key discussion points
- Decisions
- Action items
- Open questions

## AIOS Document
Use for operating rules that govern future AI behavior.

Location:

```text
AIOS/
```

AIOS documents should be short enough to be read before work begins, but explicit enough to prevent predictable failure modes.
