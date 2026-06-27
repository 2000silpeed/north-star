# AIOS Research Methodology

Version: 1.0

Status: Draft

## Purpose
Research work in North Star exists to preserve reasoning that future AI systems can continue.

The goal is not to produce more text.

The goal is to make thinking durable.

## Research Note Requirement
Create a Research Note when a new insight changes:

- Project hierarchy
- Repository responsibility
- Architecture direction
- Concept meaning
- Evaluation philosophy
- Public narrative
- Future implementation constraints

## Research Note Location
Research notes live in:

```text
docs/research/
```

Recommended filename:

```text
YYYY-MM-DD-short-topic.md
```

## Research Note Structure
A strong Research Note includes:

1. Context
2. Original discussion and reasoning
3. Initial assumptions
4. Competing ideas
5. Turning points
6. Final decision or current hypothesis
7. Rejected alternatives
8. Open questions
9. Context for future AI collaborators
10. Next research

## Research Modes
### Exploratory Research
Use when the direction is uncertain.

Output:

- Options
- Trade-offs
- Hypotheses
- Questions

Do not force a decision.

### Decision Research
Use when a durable decision is needed.

Output:

- ADR or decision note
- Rationale
- Consequences
- Rejected alternatives

### Narrative Research
Use when translating architecture into public explanation.

Output:

- White paper section
- Case-study framing
- Portfolio narrative

Must preserve the reasoning behind the story, not only the story.

### Vocabulary Research
Use when defining core concepts.

Output:

- Glossary term
- Non-example
- Evaluation implication
- Implementation-repository mapping note

No implementation code.

## Handling Uncertainty
If the answer is not clear, document uncertainty directly.

Do not hide uncertainty behind confident language.

Use:

- Current hypothesis
- Open question
- Rejected for now
- Needs validation
- Requires EKOS repository inspection

## Evidence Standard
Research claims should point to:

- A repo artifact
- An ADR
- A research note
- A glossary term
- A white paper claim
- A verified external source when needed

## Boundary Standard
Every research artifact that touches implementation must say where implementation belongs.

Default:

```text
Implementation belongs in 2000silpeed/ekos-sap-knowledge-os.
```

## Completion Standard
Research work is complete when a future AI system can answer:

- Why did this start?
- What was considered?
- What was rejected?
- What was decided?
- What remains uncertain?
- What should happen next?
