# 5. EKOS Architecture

Status: Draft v0.1

## Reader-Facing Draft
EKOS is best understood as a layered architecture between enterprise systems and AI systems.

The architecture has one purpose: convert fragmented enterprise signals into AI-usable business context with evidence and boundaries.

It is not a replacement for ERP, CRM, workflow tools, retrieval systems, MCP servers, agents, or human reviewers. It sits between them and provides the semantic infrastructure they need to work together.

At a high level, EKOS has seven layers:

1. Source systems
2. Enterprise semantic model
3. Evidence layer
4. Retrieval and reasoning layer
5. Execution boundary
6. Agent layer
7. Governance layer

The layers are not only a technical stack. They are a separation of responsibilities. Each layer answers a different enterprise AI question.

```text
AI Interfaces and Agents
        |
Agent Layer
        |
Execution Boundary
        |
Retrieval and Reasoning Layer
        |
Evidence Layer
        |
Enterprise Semantic Model
        |
Source Systems

Governance applies across every layer.
```

## Layer 1: Source Systems
Source systems contain the operational signals of the enterprise.

Examples include:

- ERP systems
- CRM systems
- MES systems
- PLM systems
- Workflow tools
- Ticketing systems
- Document repositories
- Event logs
- APIs
- External partner feeds

In an SAP logistics scenario, source systems may include delivery records, shipment records, freight orders, carrier tracking events, warehouse updates, customer commitments, and exception tickets.

The source systems are not replaced by EKOS. They remain systems of record. EKOS reads from them, references them, and may expose controlled execution paths back into them.

The architecture must preserve source-system identity. When EKOS refers to a delivery, shipment, invoice, ticket, or approval, it should maintain a link back to the authoritative source.

Key responsibility:

- Preserve operational facts and source references.

Key risk:

- Treating source data as raw text and losing object identity.

## Layer 2: Enterprise Semantic Model
The enterprise semantic model converts source signals into business meaning.

This layer defines the durable concepts EKOS works with:

- Business objects
- Object identity
- Object lifecycle states
- Business processes
- Events
- Policies
- Roles and responsibilities
- Relationships
- Execution boundaries

This layer answers:

> What is this thing, and how does it participate in the business?

For example, a delivery should not be treated only as a row from an ERP table. It should be represented as a business object connected to a sales order, shipment, freight order, carrier event, customer commitment, and possible exception process.

The semantic model must also resolve multiple references to the same object. A delivery ID may appear in SAP, a spreadsheet, a tracking update, a support ticket, and an email. EKOS should map those references back to the same business object when appropriate.

Key responsibility:

- Make enterprise meaning explicit and connected.

Key risk:

- Building a vocabulary that is too private, too abstract, or disconnected from how the business actually works.

## Layer 3: Evidence Layer
The evidence layer connects claims to support.

Enterprise AI cannot rely only on fluent explanations. It must show what evidence supports a conclusion, where that evidence came from, and how the evidence relates to the business object.

Evidence may include:

- Source-system records
- Event logs
- Documents
- Approvals
- Change history
- Timestamps
- User confirmations
- External partner updates
- System-generated facts

The evidence layer answers:

> Why should this answer be trusted?

For example, if EKOS says a delivery was delayed because of a carrier event, the evidence layer should preserve the carrier event, timestamp, related shipment, source system, and reasoning path.

Evidence should be connected to claims, not only stored as metadata. The claim "shipment delayed because of missed pickup" should point to specific facts that support it.

Key responsibility:

- Make answers traceable and reviewable.

Key risk:

- Treating evidence as citations only, instead of as structured support for business claims.

## Layer 4: Retrieval and Reasoning Layer
The retrieval and reasoning layer exposes EKOS context to AI systems.

This layer may use:

- Semantic retrieval
- Graph traversal
- GraphRAG
- Vector search
- Query planning
- Business rule checks
- Context assembly
- Reasoning traces

The layer answers:

> What context does the AI system need, and how should that context be assembled?

Retrieval should not only return related text. It should return business context: the relevant object, related objects, process state, evidence, policies, and boundaries.

For example, when a user asks why a delivery is delayed, the retrieval layer should not simply retrieve documents mentioning the delivery. It should assemble the delivery, shipment, freight order, carrier event, customer commitment, evidence trail, and applicable policy context.

This is where GraphRAG can become useful. GraphRAG is not the whole architecture, but it can use the semantic model and evidence layer to retrieve connected business context instead of isolated chunks.

Key responsibility:

- Assemble AI-usable context from enterprise meaning and evidence.

Key risk:

- Falling back to generic RAG and losing the business structure EKOS is designed to preserve.

## Layer 5: Execution Boundary
The execution boundary defines what an AI system may do.

As enterprise AI moves from answering questions to participating in workflows, execution must be constrained by meaning, policy, role, evidence, and risk.

This layer answers:

> What can the AI system do, what can it recommend, and what must be reviewed by a human?

Execution boundaries may define:

- Read-only actions
- Recommendations
- Drafted actions
- Human approval requirements
- Direct execution permissions
- Policy checks
- Audit requirements
- Rollback requirements

MCP servers and APIs may live near this layer because they expose capabilities to AI systems. But capability exposure is not enough. EKOS must connect capabilities to business authority.

For example, an AI system may be able to call a tool that updates a shipment exception. The execution boundary determines whether the AI can use that tool directly, draft the update for review, or only recommend the action to a responsible role.

Key responsibility:

- Prevent reasoning from becoming uncontrolled execution.

Key risk:

- Treating tool access as permission to act.

## Layer 6: Agent Layer
The agent layer uses EKOS context to plan and coordinate workflows.

Agents may:

- Analyze situations
- Ask follow-up questions
- Retrieve evidence
- Compare policies
- Recommend next steps
- Draft actions
- Coordinate tool calls
- Escalate to humans

The agent layer answers:

> Given the business context and boundaries, what should happen next?

Agents should not operate over loose text alone. They should operate over business objects, relationships, evidence, policies, process states, and execution boundaries provided by EKOS.

In this architecture, agents are consumers of EKOS. They are not the foundation.

Key responsibility:

- Coordinate work using enterprise context.

Key risk:

- Letting agent orchestration hide weak semantic grounding.

## Layer 7: Governance Layer
Governance applies across every layer.

Enterprise meaning changes. Processes change. Policies change. Systems change. AI models change. EKOS must preserve continuity while allowing controlled evolution.

The governance layer answers:

> Who can change enterprise meaning, how are changes reviewed, and how can the system be audited?

Governance includes:

- Semantic model review
- Versioning
- Approval workflows
- Audit trails
- Evidence review
- Policy updates
- Access control
- Human feedback
- Rollback
- Evaluation

Governance is not an optional compliance wrapper. It is part of the architecture because enterprise meaning becomes risky when it is wrong, outdated, or unreviewed.

Key responsibility:

- Keep enterprise meaning reliable over time.

Key risk:

- Treating the semantic layer as a static artifact rather than a governed system.

## Three Core Flows
The EKOS architecture can be understood through three flows.

### 1. Meaning Construction Flow
This flow turns enterprise signals into durable meaning.

```text
Source systems
  -> object identity resolution
  -> semantic model
  -> relationships and process states
  -> evidence attachment
  -> governed enterprise context
```

This flow builds the foundation that AI systems later consume.

### 2. Question Answering Flow
This flow answers enterprise questions with evidence.

```text
User question
  -> identify primary business object
  -> retrieve semantic context
  -> assemble evidence path
  -> reason over process, policy, and relationships
  -> produce answer with traceable support
```

This flow is where EKOS improves retrieval. The system does not only find related text. It assembles business context.

### 3. Action Flow
This flow moves from recommendation to controlled execution.

```text
Recommendation
  -> execution boundary check
  -> policy and role validation
  -> human approval if required
  -> tool or API execution
  -> audit record and evidence update
```

This flow is where EKOS improves tool use and agents. The system does not only call a tool. It determines whether the action is allowed and how it should be governed.

## Minimal Reference Architecture
A first EKOS prototype does not need to implement every layer fully.

A minimal version could include:

1. Synthetic SAP logistics data
2. A small business object model for delivery, shipment, freight order, carrier event, customer, and exception
3. A relationship graph connecting those objects
4. Evidence records with source references and timestamps
5. A retrieval layer that assembles graph-backed context
6. A read-only assistant that answers delay-analysis questions with evidence
7. A simple execution boundary that distinguishes answer, recommendation, and human approval

This prototype would be enough to test the core thesis:

> Does explicit enterprise meaning produce better AI answers than document retrieval alone?

## Architecture Principles
The EKOS architecture should follow these principles:

- Source systems remain systems of record.
- Enterprise meaning is modeled explicitly.
- Evidence is attached to claims.
- Retrieval returns business context, not only text.
- Tool access is constrained by execution boundaries.
- Agents consume EKOS context; they do not replace it.
- Governance applies to semantic changes and execution.

## Key Claims
- EKOS is a layered architecture between enterprise systems and AI systems.
- The semantic model and evidence layer are the architectural core.
- Retrieval, GraphRAG, MCP, APIs, and agents become stronger when grounded in EKOS context.
- Execution boundaries are part of the architecture, not an afterthought.
- A minimal prototype should start with read-only understanding and evidence before full workflow execution.

## Reasoning Preserved for Future Drafts
This section intentionally separates layers by responsibility instead of by technology vendor or framework.

Rejected framing:

- "EKOS architecture is Neo4j plus RAG plus agents."
- "EKOS architecture is MCP connected to SAP."
- "EKOS architecture is an assistant over ERP data."

Preferred framing:

- "EKOS architecture turns enterprise signals into governed semantic context for AI systems."

Specific technologies can be selected later. The architecture should remain stable even if the model, graph database, vector database, agent framework, or integration layer changes.

## Open Questions
- Should the first public architecture diagram show seven layers or three flows?
- Should the prototype implement governance minimally from the start, or only document it as a design requirement?
- Which layer owns semantic versioning: semantic model or governance?
- Should execution boundary be implemented before agent workflows?
- What storage pattern should represent evidence: graph nodes, claim records, event-sourced log, or hybrid?

## Next Section Bridge
Section 6 should define the design principles that keep the architecture disciplined: enterprise before AI, meaning before retrieval, architecture before implementation, evidence before confidence, human review for semantic authority, execution boundaries, and AI as a consumer of enterprise intelligence.
