# AIOS Collaboration Protocol

Version: 1.0

Status: Draft

## Purpose
North Star is designed for collaboration across humans and AI systems.

GPT, Claude, Gemini, Codex, and future AI collaborators should be able to continue the same research without relying on hidden chat context.

## Shared Rule
Read AIOS first.

Before strategic, architectural, or documentation work, an AI collaborator should read:

1. `AIOS/Constitution.md`
2. `AIOS/MasterPrompt.md`
3. `AIOS/OperatingRules.md`
4. The latest relevant ADR
5. The latest relevant research note or diary entry

## Role Separation
North Star mode:

- Think
- Preserve reasoning
- Compare options
- Write ADRs
- Write research notes
- Define glossary
- Define evaluation criteria
- Shape public narrative

EKOS mode:

- Build
- Implement
- Test
- Run
- Package
- Demo
- Integrate

Do not use North Star mode to create implementation artifacts.

Do not use EKOS mode to rewrite North Star research memory without linking back to the reasoning.

## Collaboration Handoff
Every handoff should include:

- Current goal
- Relevant documents
- Decisions already made
- Open questions
- Repository boundary
- Next action

## Cross-Model Continuity
When one AI system produces reasoning for another AI system:

- Avoid relying on chat-only context.
- Link to files.
- Preserve rejected alternatives.
- Explain assumptions.
- State whether claims are facts, hypotheses, or plans.

## Conflict Handling
If a new instruction conflicts with AIOS:

1. Identify the conflict.
2. Prefer explicit user instruction for the current task.
3. Record durable policy changes only when the user clearly wants the rule to change.
4. Use ADRs for lasting changes.

## Research vs Implementation Escalation
If a task starts in North Star and becomes implementation:

1. Stop implementation inside North Star.
2. Record the research/spec artifact if needed.
3. Reference `2000silpeed/ekos-sap-knowledge-os`.
4. Continue implementation only in the EKOS repository when explicitly directed.

## Communication Standard
AI collaborators should be direct about:

- What they know
- What they infer
- What remains uncertain
- What they changed
- What they did not change

Avoid hiding uncertainty behind polished prose.
