# AIOS as Research Operating System

Date: 2026-06-27

Status: Research note

## Context
North Star began as the project headquarters for EKOS strategy, white paper work, architecture decisions, and portfolio positioning.

During the first white paper and glossary work, a recurring failure mode appeared: when an AI coding agent is not constrained, it naturally drifts toward implementation. It creates roadmap language, file structures, prototype paths, and runnable-system assumptions even when the correct task is research, architecture, or reasoning preservation.

This created a repository-boundary correction:

- North Star is not the EKOS implementation repository.
- EKOS implementation belongs in `2000silpeed/ekos-sap-knowledge-os`.
- North Star owns research memory, strategy, white papers, ADRs, glossary, evaluation criteria, and public narrative.

That correction led to a deeper architectural insight.

## Original Discussion and Reasoning
The initial idea was to put stronger instructions into `AGENTS.md`.

That is useful, but insufficient.

`AGENTS.md` is a bootloader. It tells a coding agent how to enter the repository. It should not become the full operating system for long-term research behavior.

The larger need is an AI research operating system: a durable set of principles, prompts, thinking patterns, document standards, ADR rules, collaboration protocols, and continuity rules that future AI systems can read before they act.

The proposed hierarchy is:

```text
AIOS
  Research operating system

North Star
  Research lab, strategy headquarters, memory system

EKOS
  First major research result and implementation product
```

This reframes North Star.

North Star is not merely a repository for EKOS documents. North Star is a research lab. AIOS is the operating system of that lab. EKOS is the first product born from the lab.

## Initial Assumptions
The original working assumption was:

- North Star is the project headquarters for EKOS.
- `AGENTS.md` can carry the main operating rules.
- The next task after the white paper is glossary work.

Those remain partly true, but incomplete.

The deeper assumption now is:

- Long-term AI research needs an explicit operating system.
- Different repositories need different agent modes.
- North Star should optimize for thinking and continuity.
- EKOS should optimize for building and implementation.

## Competing Ideas
### Option A: Keep All Rules in AGENTS.md
Benefits:

- Simple.
- Already read by Codex.
- No new directory required.

Costs:

- Becomes too long.
- Mixes boot instructions with research philosophy.
- Harder for GPT, Claude, Gemini, and future AI systems to treat as a shared research operating system.

### Option B: Put Rules Under docs/methodology/
Benefits:

- Fits the existing repository map.
- Keeps methodology near existing AI continuity notes.

Costs:

- Still reads like documentation rather than an operating layer.
- Does not express that these rules govern every AI collaborator before normal work begins.

### Option C: Create AIOS/
Benefits:

- Makes the operating system explicit.
- Separates high-level AI research behavior from ordinary project docs.
- Gives all AI systems a clear first-read path.
- Can later generalize beyond North Star.

Costs:

- Adds a new top-level concept.
- Requires `AGENTS.md` and README to explain the new hierarchy.
- May feel larger than the immediate EKOS task unless the boundary is kept tight.

## Turning Point
The key sentence was:

> North Star is not writing. North Star is thinking. Writing is only a side effect of thinking.

This changed the task from "write better instructions" to "define a research operating system."

The second turning point was separating modes:

```text
North Star -> Think
EKOS -> Build
```

This separation prevents Codex and other AI systems from treating every repository as an implementation workspace.

## Final Decision
Create a top-level `AIOS/` directory in North Star.

The first version should include:

- `AIOS/Constitution.md`
- `AIOS/MasterPrompt.md`
- `AIOS/OperatingRules.md`
- `AIOS/ThinkingFramework.md`
- `AIOS/ResearchMethodology.md`
- `AIOS/DocumentStandard.md`
- `AIOS/ADRRules.md`
- `AIOS/CollaborationProtocol.md`
- `AIOS/AIContinuity.md`

`AGENTS.md` should become a bootloader that points future agents to AIOS before strategic, architectural, or documentation work.

## Rejected Alternatives
Rejected: Use AIOS to create implementation process rules for EKOS inside North Star.

Reason:

- That would repeat the repository-boundary failure.
- EKOS implementation belongs in `2000silpeed/ekos-sap-knowledge-os`.

Rejected: Make AIOS a generic productivity framework immediately.

Reason:

- The first version should serve North Star's actual research continuity needs.
- Generalization can come later after the rules are tested.

Rejected: Treat writing volume as success.

Reason:

- North Star should preserve reasoning, not produce documents for their own sake.

## Open Questions
- Should AIOS eventually become its own repository?
- Should EKOS have a separate Build OS or implementation Master Prompt?
- Which AIOS rules should be mirrored into `2000silpeed/ekos-sap-knowledge-os`?
- Should future research notes be required before every major architecture shift?
- How should AIOS evolve without becoming too heavy for day-to-day work?

## Context for Future AI Collaborators
If you are a future AI system reading this note, the important conclusion is not "create more docs."

The important conclusion is:

> North Star is a thinking system. AIOS defines how that thinking is preserved.

Before adding documents, ask what reasoning must be preserved. Before implementing anything, ask whether the work belongs in North Star or in the EKOS implementation repository.

## Next Research
1. Create AIOS v1 documents.
2. Update `AGENTS.md` so AI agents read AIOS first.
3. Keep the North Star/EKOS repository boundary explicit.
4. Later create a separate EKOS Master Prompt focused on building, testing, and implementation quality.
