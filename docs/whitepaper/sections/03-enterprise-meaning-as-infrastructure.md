# 3. Enterprise Meaning as Infrastructure

Status: Draft v0.1

## Reader-Facing Draft
If retrieval alone is not enough, the next question is where enterprise meaning should live.

In many AI systems, meaning is treated as something the model will infer at runtime. The system retrieves documents, passes them to a model, and expects the model to reconstruct the business context from text. This can work for simple questions, but it is fragile for enterprise workflows. The same business object may appear under different names. The same term may mean different things in different process states. The same action may be allowed in one context and prohibited in another.

Enterprise meaning should not be reconstructed from scratch every time a model receives a prompt.

It should be modeled as infrastructure.

Infrastructure is durable, shared, governed, and reusable. It does not belong to one prompt, one model, one workflow, or one assistant. It provides a stable foundation that many systems can depend on. In the same way that identity, permissions, APIs, schemas, and event logs become infrastructure in software systems, enterprise meaning should become infrastructure for AI systems.

This is the layer EKOS is designed to provide.

## What Enterprise Meaning Includes
Enterprise meaning is not a single knowledge base. It is a connected model of how the business operates.

At minimum, it includes business objects, processes, events, policies, roles, evidence, relationships, and execution boundaries.

These concepts are not documentation artifacts. They are the operating vocabulary of the enterprise. AI systems need access to this vocabulary in a form they can use consistently.

## Business Objects
Business objects are the things enterprise systems operate on.

Examples include:

- Customer
- Sales order
- Delivery
- Shipment
- Freight order
- Invoice
- Purchase order
- Material
- Plant
- Container
- Exception

A business object is not just a database row or an API response. It is an entity with identity, lifecycle, ownership, relationships, and business consequences.

For example, a delivery may be represented in SAP as structured data, mentioned in an email, referenced in a tracking event, and discussed in a customer escalation. These references should resolve to the same business object when they describe the same delivery.

Without business object identity, AI systems can confuse related records, merge unrelated situations, or reason over fragments without knowing what the primary object is.

## Business Processes
Business processes define how work moves through the enterprise.

A process is more than a checklist. It defines states, transitions, dependencies, responsibilities, and expected outcomes.

For example, a logistics process may include order confirmation, delivery creation, shipment planning, carrier assignment, pickup, transit, customs clearance, arrival, goods receipt, billing, and exception handling.

AI systems need process context because the meaning of information changes by state. A delay before pickup means something different from a delay after customs clearance. A missing document before shipment creation is different from a missing document after customer invoicing.

Without process context, AI can retrieve relevant information while misunderstanding where the business actually is.

## Events
Events represent changes in business state.

Examples include:

- Delivery created
- Shipment delayed
- Carrier reassigned
- Goods issued
- Customs hold released
- Customer escalation opened
- Invoice blocked
- Approval completed

Events matter because enterprise work is temporal. The same object can move through different meanings over time. A freight order before carrier confirmation is not the same operational situation as a freight order after repeated missed pickups.

AI systems need to understand which event changed the state, when it happened, where it came from, and what consequences followed.

Without events, enterprise AI becomes static. It can describe records, but it cannot reliably explain how a situation evolved.

## Policies
Policies define constraints on what should happen.

Policies may come from contracts, compliance requirements, internal controls, service-level commitments, approval rules, security rules, or operational playbooks.

Policies are important because AI should not only ask:

> What can be done?

It must also ask:

> What is allowed, required, risky, or prohibited?

For example, an AI system may be able to recommend rerouting a shipment. But a policy may require customer approval, manager approval, cost threshold checks, or regional compliance review before that action can be executed.

Without policies, AI recommendations can sound useful while ignoring the constraints that govern real enterprise work.

## Roles and Responsibilities
Enterprise work is performed by people, teams, systems, and external partners with different responsibilities.

Examples include:

- Sales operations
- Logistics planner
- Warehouse team
- Carrier
- Customer service
- Finance
- Compliance
- Manager approver
- System owner

Roles matter because enterprise AI must know who owns a decision, who can approve an action, who should be notified, and who is accountable when a workflow changes.

An AI system should not treat every action as if it belongs to the same user. Some actions can be automated. Some require review. Some require escalation. Some belong outside the AI system's authority entirely.

Without roles and responsibilities, AI systems cannot respect organizational boundaries.

## Evidence
Evidence is the basis for confidence.

In enterprise workflows, a fluent answer is not enough. The system must show why it believes something is true.

Evidence may include:

- System records
- Event logs
- Documents
- Approvals
- Timestamps
- Change history
- Source-system references
- User confirmations
- External partner updates

Evidence must be connected to claims. If an AI system says a shipment was delayed because of a carrier event, it should identify the event, source, timestamp, related object, and reasoning path.

Without evidence, AI cannot be audited, reviewed, or trusted in operational workflows.

## Relationships
Relationships connect enterprise meaning.

A delivery may belong to a sales order. A shipment may include multiple deliveries. A freight order may be assigned to a carrier. A carrier event may affect a shipment. A shipment delay may affect a customer commitment. A customer commitment may affect billing or escalation priority.

These relationships are the difference between isolated information and business context.

AI systems need relationships because enterprise questions often require traversal:

> Starting from this delivery, which shipment is affected, which customer commitment is at risk, what evidence supports the delay reason, and who must approve the next action?

Without relationships, AI can retrieve fragments but cannot follow the business chain.

## Execution Boundaries
Execution boundaries define what an AI system may do, what it may recommend, and what it must escalate.

This is critical because enterprise AI is moving from answering questions to participating in workflows. Once an AI system can call tools, update records, create tickets, trigger workflows, or send messages, meaning must be connected to authority.

Execution boundaries answer questions such as:

- Can the AI system only summarize this situation?
- Can it recommend an action?
- Can it draft the action for human approval?
- Can it execute the action directly?
- Which policy constrains execution?
- Which role must approve the step?
- Which system records the result?

Without execution boundaries, AI systems risk turning uncertain reasoning into operational action.

## Why This Must Be Infrastructure
These concepts should not live only inside prompts. Prompts are too local, too fragile, and too dependent on a specific model or workflow.

They should not live only in documents. Documents can describe concepts, but they do not reliably enforce identity, relationships, state, or authority.

They should not live only inside application code. Code can implement behavior, but the meaning must be visible, reviewable, versioned, and reusable across AI systems.

Enterprise meaning should be a shared infrastructure layer that models and agents can consume.

That infrastructure should be:

- Explicit: concepts and relationships are represented directly
- Connected: objects, events, policies, roles, and evidence are linked
- Reviewable: humans can inspect and correct the meaning layer
- Versioned: semantic changes can be tracked over time
- Reusable: multiple AI systems can use the same enterprise meaning
- Governed: execution boundaries and authority are part of the model

## What EKOS Provides
EKOS turns enterprise meaning into infrastructure.

It provides a semantic foundation that allows AI systems to understand not only what information exists, but how that information participates in enterprise work.

With EKOS, retrieval can return evidence in business context. Tools can execute within semantic boundaries. Agents can plan over business objects and process states instead of loose text. Human reviewers can inspect the reasoning path rather than only the final answer.

The result is not an AI system that knows everything. The result is an AI system that has a durable foundation for asking better questions, grounding answers in evidence, and respecting enterprise boundaries.

## Key Claims
- Enterprise meaning should not be inferred from scratch at runtime.
- Business objects, processes, events, policies, roles, evidence, relationships, and execution boundaries form the minimum semantic substrate for enterprise AI.
- Enterprise meaning should be explicit, connected, reviewable, versioned, reusable, and governed.
- EKOS positions this meaning layer as infrastructure rather than as prompt logic, documentation, or application-specific code.
- This infrastructure lets retrieval, tools, and agents operate over business context instead of fragmented information.

## Reasoning Preserved for Future Drafts
This section intentionally uses infrastructure language rather than only ontology language.

Rejected framing:

- "We need an ontology for enterprise AI."
- "We need a knowledge graph for enterprise AI."

Preferred framing:

- "Enterprise meaning should be modeled as infrastructure."

Ontology and graph structures may implement this infrastructure, but they should not be the first message. The first message is that enterprise AI needs durable, governed meaning that survives model changes and workflow changes.

## Open Questions
- Should this section introduce the term "semantic substrate" or avoid it until the architecture section?
- Should "execution boundary" become an official EKOS glossary term?
- Should evidence be modeled as a first-class object or as metadata attached to claims and relationships?
- How should EKOS distinguish policy, rule, constraint, and control?
- Which of these concepts should appear in the first prototype?

## Next Section Bridge
Section 4 should define what EKOS is and what it is not. It should use the infrastructure concepts from this section to position EKOS as a platform, not as an ontology demo, knowledge graph wrapper, RAG pattern, agent framework, or assistant UI.
