# ADR-0001 — Research Continuity Workflow

**Status:** Accepted

**Date:** 2026-06-29

---

# Context

EKOS research increasingly depends on long-form architectural discovery sessions.

These sessions produce important reasoning that influences:

- EKOS architecture
- North Star white papers
- validation matrices
- design review notes
- benchmark design
- portfolio positioning
- future implementation priorities

However, chat sessions are temporary.

Long conversations are difficult to preserve, transfer, and resume accurately.

If important reasoning remains only in chat history, the research becomes fragile.

North Star exists to prevent that failure mode.

---

# Decision

North Star will treat Git as the durable research memory.

Chat will be used as a reasoning engine.

GitHub will be used as the persistent memory system.

Research work will therefore be organized into short, self-contained **Research Sprints** or **Discovery Sprints**.

Each sprint should produce one durable research result and then be summarized into North Star before a new session begins.

---

# Workflow

```text
Discovery Session
        ↓
Challenge and Debate
        ↓
Architectural Discovery
        ↓
Design Review Note or ADR
        ↓
GitHub Commit
        ↓
Session Summary
        ↓
Next Sprint Prompt
        ↓
New Chat Session
```

The goal is not to keep one conversation alive forever.

The goal is to preserve research continuity through repository artifacts.

---

# Sprint Size

A sprint should remain small enough to preserve clarity.

Recommended size:

- one primary research question
- 20–30 meaningful exchanges
- one durable conclusion
- one repository update

A sprint may be shorter if the discovery is clear.

A sprint should end when the conversation begins to depend heavily on accumulated chat context rather than repository state.

---

# End-of-Sprint Checklist

At the end of each sprint, the AI research reviewer should produce or update:

1. A concise summary of the discovery.
2. The appropriate repository artifact:
   - Design Review Note
   - ADR
   - Research Note
   - Validation Matrix update
   - White Paper update
3. GitHub commit.
4. Impact assessment:
   - Impact on EKOS architecture
   - Impact on White Paper
   - Impact on Validation Matrix
   - Impact on implementation roadmap
5. A next-session prompt.

---

# Repository Artifact Selection

Not every idea should become an ADR.

Use the following rule:

## Design Review Note

Use when the session discovers or clarifies an architectural insight, research framing, or conceptual evolution.

## ADR

Use when the project makes a durable operating, architectural, or governance decision.

## Validation Matrix Update

Use when the session changes what EKOS must prove or how it can be falsified.

## White Paper Update

Use only when the insight is stable enough to become part of the public thesis.

## EKOS Implementation Issue or Roadmap Item

Use when the discovery implies a concrete implementation change in `2000silpeed/ekos-sap-knowledge-os`.

---

# Session Transfer Rule

A new chat session must not rely on hidden memory alone.

A new session should begin by reading North Star artifacts first.

Minimum starting context:

1. `README.md`
2. latest ADRs
3. latest Design Review Notes
4. current Validation Matrix
5. current EKOS portfolio or benchmark summary

The assistant should then continue from the repository state rather than from assumed chat memory.

---

# Standard Next Sprint Prompt

Use the following prompt when starting a new session:

```text
Continue EKOS Research Sprint.

Before reasoning:

1. Read `2000silpeed/north-star` README.
2. Read the latest ADRs.
3. Read the latest Design Review Notes.
4. Read the current Validation Matrix if the task affects research claims.
5. Continue from the latest repository state.

Do not repeat previous discussions.
Do not expand philosophy unless current hypotheses fail.
Optimize for research continuity, falsifiability, and EKOS strength.
```

---

# Roles

## Chief Architect — Human

Responsible for:

- bringing real enterprise context
- challenging abstract assumptions
- identifying practical adoption constraints
- deciding whether an insight reflects enterprise reality

## Principal Research Reviewer — AI

Responsible for:

- challenging hypotheses
- identifying hidden assumptions
- converting discoveries into durable artifacts
- maintaining North Star consistency
- updating GitHub when appropriate

The AI must not optimize for agreement.

The AI must optimize for truth, evidence, and research continuity.

---

# Rationale

This workflow prevents three failure modes:

## 1. Context Collapse

Important reasoning disappears when chat history becomes too long or inaccessible.

## 2. Philosophy Drift

Ideas become larger and more abstract without being converted into testable research artifacts.

## 3. Repository Drift

GitHub becomes outdated relative to the current research state.

By ending each sprint with a repository update, North Star remains the source of truth.

---

# Consequences

This decision means:

- long conversations should be intentionally closed
- important reasoning must become repository artifacts
- future sessions should read GitHub before continuing
- North Star becomes long-term working memory
- chat becomes temporary reasoning space

This also strengthens the original North Star principle:

> Chat is temporary. Git is persistent.

---

# Guiding Principle

> Discovery creates ideas.

> North Star preserves them.

> EKOS proves them.
