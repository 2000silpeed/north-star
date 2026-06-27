# AIOS AI Continuity

Version: 1.0

Status: Draft

Based on: `docs/methodology/AI-CONTINUITY.md`

## Purpose
North Star is not written only for humans.

It is written so future AI systems can continue the research without losing reasoning context.

## Core Principle
Chats are temporary.

Git is persistent.

Reasoning must be preserved, not only conclusions.

Every major discussion must create a Research Note preserving the full reasoning process, not only the conclusion.

North Star is a collective research memory, not a documentation repository.

## Continuity Requirements
Every important research document should contain:

1. Context
2. Original discussion and reasoning
3. Competing ideas
4. Turning points
5. Final decision or current hypothesis
6. Rejected alternatives
7. Open questions
8. Context for future AI collaborators
9. Next actions

## Future AI Reader Test
A document passes the continuity test when a future AI system can answer:

- Why does this document exist?
- What changed because of it?
- What alternatives were considered?
- What should not be repeated?
- What remains unresolved?
- Where should work continue?

## Memory Layers
Use the correct memory layer:

- `AIOS/` for operating rules
- `docs/research/` for emerging insights and hypotheses
- `docs/adr/` for durable decisions
- `docs/diary/` for session continuity
- `docs/glossary/` for stable vocabulary
- `docs/whitepaper/` for public thesis and architecture narrative

## Anti-Pattern: Summary Without Reasoning
Do not reduce important reasoning to a short summary when future collaborators need the path.

Preserve:

- Why the issue mattered
- What was tempting but rejected
- Which assumption changed
- What future mistakes are likely

## Anti-Pattern: Chat Memory Dependency
If a future AI system needs the previous chat to understand the decision, the repository has failed.

Move the reasoning into Git.

## Continuity Output Rule
When finishing research work, state:

- Files changed
- Decisions recorded
- Open questions
- Next research action
- Whether implementation was avoided or redirected

## Goal
Create a human and AI collective research memory that survives changes in AI models and enables future collaboration across different AI systems.
