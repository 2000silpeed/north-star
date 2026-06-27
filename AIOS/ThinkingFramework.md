# AIOS Thinking Framework

Version: 1.0

Status: Draft

## Purpose
This framework defines how AI collaborators should think inside North Star.

It is not a coding workflow.

It is a reasoning workflow.

## Core Flow
Use this sequence for research, architecture, strategy, and public narrative work:

```text
Problem
  -> Context
  -> Assumptions
  -> Competing Ideas
  -> Trade-offs
  -> Decision
  -> Evidence
  -> Consequences
  -> Open Questions
  -> Future Work
```

## 1. Problem
State the problem in enterprise terms before naming technologies.

Weak:

> We need GraphRAG.

Stronger:

> Enterprise AI cannot reliably answer operational questions when business objects, evidence, and execution boundaries are implicit.

## 2. Context
Preserve why the problem matters now.

Context should include:

- What triggered the discussion
- What repository or project boundary matters
- What prior decisions constrain the work
- Which documents are relevant

## 3. Assumptions
List the assumptions before choosing an answer.

Common North Star assumptions:

- Enterprise semantics change slower than AI models.
- AI systems should consume enterprise intelligence, not own it.
- North Star is research infrastructure, not implementation infrastructure.
- EKOS implementation belongs in `2000silpeed/ekos-sap-knowledge-os`.

## 4. Competing Ideas
If there is more than one path, document them.

Use:

- Option A
- Option B
- Option C

Do not collapse disagreement too early.

## 5. Trade-offs
For each option, explain:

- Benefits
- Costs
- Risks
- Long-term consequences
- What future AI systems may misunderstand

## 6. Decision
State the decision clearly.

A decision should answer:

- What are we choosing?
- What are we not choosing?
- Why is this direction stronger?
- What evidence or reasoning supports it?

## 7. Evidence
Evidence can include:

- Repository history
- ADRs
- Research notes
- White paper claims
- Glossary definitions
- Prior rejected alternatives
- External references, when verified

Do not treat model confidence as evidence.

## 8. Consequences
Every decision creates constraints.

Document:

- What this enables
- What this prevents
- What future work must respect
- What failure modes remain

## 9. Open Questions
Do not force closure when uncertainty remains.

Open questions preserve research direction for future AI systems.

## 10. Future Work
State the next research action, not only the next task.

Weak:

> Continue.

Stronger:

> Compare the glossary terms against the existing EKOS repository structure before proposing implementation mappings.

## Thinking Standard
North Star outputs should make reasoning inspectable.

If the reasoning is not visible, the document is not finished.
