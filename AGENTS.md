# Project North Star Instructions

## Role
You are working on Project North Star, the operating repository for building an Enterprise AI engineering portfolio around EKOS.

Treat this repository as a project headquarters, not a scratchpad.

## AIOS Boot Protocol
Before strategic, architectural, or documentation work, read AIOS first:

1. `AIOS/Constitution.md`
2. `AIOS/MasterPrompt.md`
3. `AIOS/OperatingRules.md`

AIOS is the research operating system for North Star. `AGENTS.md` is only the bootloader.

## Repository Boundary
Do not recreate EKOS inside North Star.

North Star is the research memory, strategy, white paper, and portfolio headquarters for EKOS. The actual EKOS implementation remains in `2000silpeed/ekos-sap-knowledge-os`:

- `https://github.com/2000silpeed/ekos-sap-knowledge-os`

When writing implementation sections, reference the existing EKOS repository instead of designing a separate replacement system in this repository. North Star may define architecture, glossary terms, evaluation criteria, public narrative, ADRs, and portfolio strategy. Executable code, prototype data, implementation structure, and runnable demos belong in the EKOS implementation repository unless the user explicitly says otherwise.

## Required Reading
Before making strategic, architectural, or documentation changes, read:

1. `AIOS/Constitution.md`
2. `AIOS/MasterPrompt.md`
3. `AIOS/OperatingRules.md`
4. `README.md`
5. `docs/diary/day-001.md`
6. `docs/methodology/AI-CONTINUITY.md`
7. The latest relevant ADR in `docs/adr/`

## Operating Principle
Chat is temporary. Git is persistent.

Do not leave important reasoning only in conversation. If a decision affects EKOS, North Star, portfolio strategy, public positioning, or future implementation, record it in the repository.

Every major discussion must create a Research Note preserving the full reasoning process, not only the conclusion. North Star is a collective research memory, not a documentation repository.

## Source of Truth
- AI research operating system: `AIOS/`
- Vision and positioning: `README.md`, `docs/principles/`, `docs/research/`
- Architecture decisions: `docs/adr/`
- EKOS design review notes: `docs/design-review/`
- White paper drafts: `docs/whitepaper/`
- Session history and working memory: `docs/diary/`
- Portfolio and GitHub reviews: `reviews/`
- Roadmap and cross-repository execution planning: `roadmap/`
- Terms and concept definitions: `docs/glossary/`
- EKOS implementation: `2000silpeed/ekos-sap-knowledge-os`

## Decision Rules
- Every major claim must answer "Why?"
- Do not optimize for agreement. Optimize for truth.
- If evidence contradicts the user preference, current EKOS narrative, or prior North Star documents, say so directly.
- Start from enterprise problems, not technology trends.
- Do not present ontology, GraphRAG, MCP, or agents as the product. They are implementation mechanisms.
- Present EKOS as enterprise intelligence infrastructure.
- Do not invent private company data, customer names, metrics, or production claims.
- Separate facts, hypotheses, and future plans clearly.

## Documentation Rules
- Prefer concise, durable documents over chat-style notes.
- Preserve reasoning, rejected alternatives, and open questions.
- Create Research Notes for major discussions before reducing them to conclusions.
- Use ADRs for decisions that future contributors may question.
- Use research notes for working hypotheses.
- Use diary entries for session summaries and next actions.

## Current Next Task
Continue from `docs/adr/ADR-0001-why-ekos-exists.md`.

The immediate project priority is to turn EKOS from an idea into a coherent public architecture:

1. Use AIOS as the operating layer for future research work.
2. Review Phase 1 glossary at `docs/glossary/enterprise-concept-glossary-v1.md`.
3. Inspect `2000silpeed/ekos-sap-knowledge-os` before proposing implementation mappings.
4. Connect all existing projects to the EKOS narrative or archive them.
