# ADR-0002 - North Star and EKOS Repository Boundary

Date: 2026-06-27

Status: Accepted

## Context
North Star was created as the operating repository for EKOS strategy, research memory, white paper work, portfolio positioning, and durable AI collaboration context.

During white paper drafting, the implementation roadmap began to read as if North Star itself should contain the EKOS prototype, data, semantic model, retrieval baseline, evaluation harness, and demo application.

That direction is wrong.

The actual EKOS implementation already has a dedicated repository:

- `2000silpeed/ekos-sap-knowledge-os`
- `https://github.com/2000silpeed/ekos-sap-knowledge-os`

North Star should not become a second implementation repository or a replacement for the existing EKOS codebase.

## Decision
North Star is the research memory, strategy, white paper, and portfolio headquarters for EKOS.

The EKOS implementation remains in `2000silpeed/ekos-sap-knowledge-os`.

When North Star documents discuss implementation, they must reference the existing EKOS repository and avoid designing a separate replacement system inside North Star.

## Repository Responsibilities
North Star owns:

- EKOS thesis and positioning
- White paper drafts
- ADRs and reasoning history
- Glossary and concept definitions
- Evaluation criteria and public narrative
- Portfolio strategy and project reviews
- Cross-repository coordination notes

The EKOS implementation repository owns:

- Executable EKOS code
- Prototype data and runnable examples
- Semantic model implementation
- Context assembly logic
- Retrieval baseline implementation
- Evaluation harness code
- Demos, notebooks, CLIs, or app surfaces
- Implementation issues, pull requests, and release artifacts

## Consequences
Accepted consequences:

- North Star implementation sections should be coordination plans, not local build plans.
- White paper roadmaps should say which work belongs in the EKOS implementation repository.
- North Star may define glossary and evaluation specs, but code and runnable assets should be implemented in `2000silpeed/ekos-sap-knowledge-os`.
- Future AI sessions must not create `prototype/`, app, data, or runnable EKOS implementation trees inside North Star unless explicitly requested.

Tradeoffs:

- Some implementation planning will be less self-contained inside the white paper.
- Cross-repository links and status tracking become more important.
- Before giving concrete file paths for implementation work, contributors should inspect the existing EKOS repository structure.

## Rejected Alternative
Rejected: Build a separate EKOS prototype inside North Star.

Reason:

- It duplicates the existing implementation repository.
- It splits code and architecture context across two competing sources.
- It risks turning North Star from a strategy and research HQ into a second implementation workspace.

## Next Actions
1. Update the white paper implementation roadmap to reference `2000silpeed/ekos-sap-knowledge-os`.
2. Keep Phase 1 glossary work in North Star as a specification artifact.
3. When implementation work begins, inspect the EKOS repository and align changes with its existing structure.
4. Record cross-repository links between North Star specs and EKOS implementation issues or pull requests.
