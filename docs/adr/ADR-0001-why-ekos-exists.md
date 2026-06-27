# ADR-0001 - Why EKOS Exists

Date: 2026-06-27

Status: Accepted

## Context
Enterprise AI systems are improving quickly, but most deployments still struggle with enterprise understanding.

Current approaches often start from the AI layer:

- Retrieval over documents
- Tool calling through APIs
- Workflow automation
- Agent orchestration
- Model-specific prompt design

These approaches can be useful, but they do not by themselves explain how an enterprise works. They often miss stable business meaning: objects, processes, responsibilities, evidence, policies, events, and the relationships between them.

The key observation behind EKOS is:

> AI models change quickly. Enterprise semantics change slowly.

If the durable part of the system lives only in prompts, documents, or model-specific implementations, the enterprise loses continuity every time the AI layer changes.

## Decision
EKOS exists to provide enterprise intelligence infrastructure between enterprise systems and AI systems.

EKOS is not primarily an ontology project, a GraphRAG project, an agent framework, or an AI assistant. Those are implementation mechanisms.

EKOS is a platform that helps AI systems understand how an enterprise operates by making enterprise meaning explicit, connected, reviewable, and reusable.

## One-Sentence Definition
EKOS is enterprise intelligence infrastructure that turns business systems, processes, and evidence into a semantic foundation AI can reliably use.

## Mission
Build the foundational infrastructure that allows AI to understand enterprise systems.

## Principles
- Enterprise before AI
- Meaning before retrieval
- Architecture before implementation
- Evidence before confidence
- Problems before solutions

## Why Existing Approaches Are Not Enough
### Document Retrieval
Document retrieval can find text, but enterprise work is not only text. It is made of business objects, lifecycle states, responsibilities, exceptions, policies, and evidence.

### Vector Search
Vector search can find similarity, but similarity is not the same as business meaning. Enterprise AI needs explicit relationships and constraints, not only nearest neighbors.

### API and MCP Integration
APIs and MCP servers can execute actions, but execution does not guarantee understanding. An AI system needs to know what an action means, when it is valid, what evidence supports it, and what business consequences may follow.

### Agents
Agents can coordinate tasks, but without durable enterprise semantics they become orchestration over weak context. EKOS gives agents a stable operating substrate.

## What EKOS Enables
EKOS should help answer questions such as:

- What business object is being discussed?
- Which process does this object belong to?
- What evidence supports this conclusion?
- Which policy or responsibility constrains the action?
- What downstream business impact can follow?
- Which system should execute the next step?

## Positioning
The public message should not start with ontology or GraphRAG.

The correct order is:

1. Enterprise AI fails when it cannot understand how the business works.
2. Business meaning must be explicit, connected, and reviewable.
3. EKOS provides the enterprise intelligence infrastructure for that meaning.
4. Ontology, knowledge graphs, GraphRAG, MCP, and agents become implementation layers inside the platform.

## Consequences
Accepted consequences:

- North Star should present EKOS as a platform, not a feature or layer.
- All major North Star projects should connect to EKOS or be archived.
- Future README, portfolio, and white paper work should lead with enterprise problems.
- Claims about company data, customer impact, or production outcomes must be factual and evidence-backed.

Tradeoffs:

- This positioning is harder than presenting a simple demo.
- It requires stronger definitions and more disciplined documentation.
- It forces every technical component to justify its role in the enterprise intelligence architecture.

## Open Questions
- What is the smallest EKOS prototype that proves the architecture?
- Which enterprise concepts are universal across ERP, CRM, MES, PLM, and other systems?
- How should semantic changes be reviewed and versioned?
- What should count as evidence in an enterprise AI workflow?
- Where is the boundary between understanding and execution?

## Next Actions
1. Draft `docs/glossary/enterprise-intelligence.md`.
2. Draft the EKOS White Paper outline.
3. Create a repository map that links existing projects to EKOS.
4. Define the first prototype around a concrete enterprise workflow.

