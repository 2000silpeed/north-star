# 6. Design Principles

Status: Draft v0.1

## Reader-Facing Draft
EKOS is not defined only by its architecture. It is also defined by the principles that constrain the architecture.

These principles exist because enterprise AI systems are easy to overbuild in the wrong direction. It is tempting to start with a model, a retrieval pattern, an agent framework, or a tool integration and then search for enterprise use cases afterward. EKOS takes the opposite direction.

The enterprise problem comes first. The AI system follows.

The following principles define how EKOS should be designed, implemented, evaluated, and extended.

## Principle 1: Enterprise Before AI
Enterprise reality must come before AI capability.

### What It Means
EKOS should begin with how the enterprise actually works: business objects, processes, responsibilities, policies, evidence, and execution boundaries.

AI is not the center of the architecture. AI is a consumer of enterprise intelligence.

The system should ask:

> What business situation must be understood?

before it asks:

> Which model, agent, or retrieval pattern should we use?

### What It Prevents
This principle prevents technology-first design.

It prevents EKOS from becoming:

- A demo built around the latest model
- A graph built without operational use
- An agent workflow without business authority
- A retrieval pipeline without process context

### Design Implication
Every EKOS feature must be connected to an enterprise problem.

If a component does not help AI understand a business object, process, event, policy, role, evidence chain, relationship, or execution boundary, it does not belong in the core architecture.

## Principle 2: Meaning Before Retrieval
Retrieval should operate over meaning, not replace it.

### What It Means
EKOS should not treat retrieval as the source of enterprise understanding.

Retrieval is necessary, but it should retrieve business context: objects, relationships, evidence, policies, and process states. It should not only retrieve text chunks that appear semantically similar to a question.

The system should ask:

> What does this information mean inside the business?

not only:

> What information is related to this query?

### What It Prevents
This principle prevents generic RAG from becoming the architecture.

It prevents:

- Overloading vector search with business reasoning
- Treating similarity as equivalence
- Retrieving policy text without knowing whether it applies
- Returning documents without object identity or process state

### Design Implication
The semantic model and evidence layer must exist before retrieval becomes enterprise-grade.

Retrieval should be evaluated by whether it returns the right business context, not only whether it returns relevant text.

## Principle 3: Architecture Before Implementation
Architecture should remain stable even when implementation choices change.

### What It Means
EKOS should not be defined by a specific model, database, vector store, graph engine, agent framework, or integration protocol.

Those choices matter, but they are replaceable. The architectural responsibilities should remain stable:

- Source systems preserve operational facts.
- The semantic model represents business meaning.
- The evidence layer grounds claims.
- Retrieval assembles business context.
- Execution boundaries constrain actions.
- Agents consume context.
- Governance keeps meaning reliable.

### What It Prevents
This principle prevents vendor-shaped architecture.

It prevents EKOS from being described as:

- A Neo4j project
- A LangChain project
- A GraphRAG project
- An MCP project
- A single-model workflow

### Design Implication
Implementation choices should be documented as replaceable decisions.

When a technology is selected, the reason should be connected to the architectural role it serves, not to hype or convenience alone.

## Principle 4: Evidence Before Confidence
Enterprise AI should earn confidence through evidence.

### What It Means
An answer should not be considered reliable only because it is fluent, plausible, or produced by a capable model.

EKOS should connect claims to evidence. The system should show which records, events, documents, approvals, timestamps, and source references support a conclusion.

The system should ask:

> What evidence supports this answer?

before it asks:

> How confident does the model sound?

### What It Prevents
This principle prevents unsupported automation.

It prevents:

- Fluent but ungrounded answers
- Recommendations without traceability
- Hidden reasoning paths
- Actions based on weak or missing evidence
- Operational decisions that cannot be audited

### Design Implication
Evidence must be modeled as a first-class part of EKOS.

Every important answer should be traceable to evidence. Every recommendation should separate fact, inference, and proposed action.

## Principle 5: Human Review for Semantic Authority
AI may propose meaning, but humans must govern enterprise meaning.

### What It Means
Enterprise semantics are not only technical artifacts. They encode how the business operates. Incorrect semantics can cause wrong recommendations, wrong escalations, or wrong actions.

AI can help detect patterns, propose relationships, summarize evidence, and suggest model changes. But semantic authority should remain governed by responsible humans.

The system should ask:

> Who has authority to approve this meaning?

not only:

> Can the model infer this relationship?

### What It Prevents
This principle prevents uncontrolled semantic drift.

It prevents:

- AI-generated ontologies becoming authoritative without review
- Business rules changing silently
- Process meanings drifting across teams
- Agents acting on unapproved interpretations

### Design Implication
Semantic changes should be reviewable, versioned, and reversible.

EKOS should support human approval for changes to business object definitions, relationships, policies, and execution boundaries.

## Principle 6: Execution Must Have Boundaries
The ability to call a tool is not permission to act.

### What It Means
Enterprise AI systems increasingly connect to APIs, MCP servers, workflow tools, ticketing systems, and operational platforms. This makes execution possible, but execution must be constrained.

EKOS should define what the AI system may do directly, what it may draft, what it may recommend, and what must be escalated to a human.

The system should ask:

> Is this action allowed in this business context?

before it asks:

> Which tool can perform this action?

### What It Prevents
This principle prevents tool access from becoming uncontrolled automation.

It prevents:

- Acting on incomplete context
- Bypassing approval workflows
- Updating source systems without authority
- Confusing recommendations with executable decisions
- Treating MCP or API availability as business permission

### Design Implication
Execution boundaries should be part of the semantic model and governance layer.

Every executable action should be tied to policy, role, evidence, approval requirement, and auditability.

## Principle 7: AI Consumes Enterprise Intelligence; It Does Not Own It
Enterprise intelligence should outlive any model.

### What It Means
AI models will change. Prompts will change. Agent frameworks will change. Retrieval systems will change.

Enterprise meaning should not disappear or become invalid when those systems change.

EKOS should preserve enterprise intelligence as a durable asset that different AI systems can consume over time.

The system should ask:

> What should remain stable when the AI layer changes?

before it asks:

> How can this model solve the current task?

### What It Prevents
This principle prevents model-coupled enterprise knowledge.

It prevents:

- Encoding business meaning only in prompts
- Losing reasoning context when models change
- Rebuilding enterprise context for every AI workflow
- Treating AI output as the system of record for business meaning

### Design Implication
Enterprise meaning must be stored, versioned, reviewed, and exposed independently from any one AI model.

Models should query EKOS, reason over EKOS, and act within EKOS boundaries. They should not be the owner of the enterprise meaning layer.

## Principle 8: Start Read-Only, Then Add Action
Understanding should precede automation.

### What It Means
The first EKOS prototype should prove that explicit enterprise meaning improves understanding before it attempts broad workflow automation.

A read-only system that answers with evidence is more valuable than an action-taking system that cannot explain itself.

The system should ask:

> Can we understand and explain the business situation reliably?

before it asks:

> Can we automate the next step?

### What It Prevents
This principle prevents premature automation.

It prevents:

- Building agents before the semantic foundation is reliable
- Adding tool execution before evidence is traceable
- Demonstrating workflow automation without business authority
- Mistaking activity for correctness

### Design Implication
The first EKOS prototype should focus on read-only understanding, evidence paths, and recommendation boundaries.

Execution can be added after the system can reliably identify objects, relationships, process states, evidence, and policy constraints.

## How These Principles Work Together
The principles are mutually reinforcing.

Enterprise before AI defines the starting point. Meaning before retrieval defines the context strategy. Architecture before implementation keeps the system independent from tooling. Evidence before confidence makes answers reviewable. Human review gives semantic authority. Execution boundaries make tool use safe. AI as a consumer keeps enterprise meaning durable. Starting read-only keeps the first prototype disciplined.

Together, they define the engineering posture of EKOS:

> Build enterprise meaning first, then let AI use it.

## Design Review Checklist
Every major EKOS design decision should answer these questions:

1. What enterprise problem does this solve?
2. What business object, process, event, policy, role, evidence, relationship, or boundary does it clarify?
3. What evidence supports the system's answer or recommendation?
4. Who can review or correct the meaning?
5. What happens if the model, database, retrieval tool, or agent framework changes?
6. Is the system answering, recommending, drafting, or executing?
7. Where is the execution boundary?
8. What is preserved for audit and future review?

If a design cannot answer these questions, it is not ready to become part of EKOS.

## Key Claims
- EKOS must be constrained by design principles, not only described by architecture.
- Enterprise meaning should be built before AI workflows are automated.
- Evidence, human review, and execution boundaries are architectural requirements, not optional compliance features.
- AI systems should consume enterprise intelligence rather than own it.
- The first EKOS prototype should prove read-only understanding before moving into execution.

## Reasoning Preserved for Future Drafts
This section intentionally turns strategic philosophy into engineering constraints.

Rejected framing:

- "Principles are branding."
- "Principles are high-level values only."
- "Implementation can decide the principles later."

Preferred framing:

- "Principles are design constraints."

These principles should later become review criteria for ADRs, prototype design, README positioning, and portfolio evaluation.

## Open Questions
- Should "Start Read-Only, Then Add Action" remain a core principle or move to prototype strategy?
- Should "Human Review for Semantic Authority" be renamed to "Human Authority over Enterprise Meaning"?
- Should design principles become separate files under `docs/principles/`?
- Which principles should be visible in the public README?
- Should every future ADR include a "Principles Applied" section?

## Next Section Bridge
Section 7 should introduce the prototype scenario. It should use SAP logistics delay analysis to show how EKOS applies these principles to a concrete enterprise workflow.
