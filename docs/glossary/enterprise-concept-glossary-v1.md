# Enterprise Concept Glossary v1

Status: Draft v1

Date: 2026-06-27

Scope: North Star specification artifact

Implementation repository: `2000silpeed/ekos-sap-knowledge-os`

## Purpose
This glossary defines the core enterprise concepts EKOS uses to make business meaning explicit, connected, reviewable, and reusable for AI systems.

This is not implementation code. It is a specification vocabulary for the EKOS white paper, evaluation criteria, public narrative, and future implementation work in `2000silpeed/ekos-sap-knowledge-os`.

## Repository Boundary
North Star may define terms, examples, non-examples, evaluation expectations, and concept boundaries.

North Star must not create:

- Prototype code
- App code
- Dataset files
- Runnable demos
- Implementation modules

Those belong in `2000silpeed/ekos-sap-knowledge-os`.

## Term Template
Each term uses the same structure:

- Definition
- Why it matters
- SAP logistics delay example
- Non-example
- EKOS implementation repository use

---

## 1. Business Object
Definition:

A durable enterprise entity that business work refers to, changes, governs, or evaluates.

Why it matters:

AI systems need to know what the work is about. Without business object identity, retrieved text can be mixed across unrelated records, and answers can lose the primary object of analysis.

SAP logistics delay example:

`Delivery D-10045`, `Shipment S-7781`, `Freight Order FO-5510`, `Carrier Event CE-3302`, `Customer Commitment CC-1201`, and `Exception EX-9007` are business objects.

Non-example:

A paragraph that mentions a delivery is not itself the delivery. A vector chunk is not a business object unless it is linked to the durable enterprise entity it describes.

EKOS implementation repository use:

The EKOS implementation should use this term to name the primary entities that context assembly, relationship traversal, evidence linking, and evaluation operate on.

---

## 2. Business Process
Definition:

A structured flow of enterprise work that moves business objects through states, decisions, responsibilities, and outcomes.

Why it matters:

Business meaning changes with process state. The same event can mean different things before pickup, after shipment departure, during customs hold, or after customer commitment breach.

SAP logistics delay example:

A logistics fulfillment process may include delivery creation, shipment planning, freight order assignment, carrier pickup, transit, delivery arrival, exception handling, and customer commitment review.

Non-example:

A checklist in a document is not automatically a business process. It becomes useful to EKOS only when connected to states, transitions, responsible roles, evidence, and affected objects.

EKOS implementation repository use:

The EKOS implementation should use process definitions to interpret object state, determine which relationships matter, and distinguish normal progress from exceptions.

---

## 3. Event
Definition:

A time-bound record that something happened to a business object or process.

Why it matters:

Enterprise work changes over time. AI systems need event history to explain why a state changed, when it changed, and which source supports that claim.

SAP logistics delay example:

Carrier event `CE-3302` reports that pickup was missed because of capacity shortage at `2026-06-24 09:30`. Actual pickup later occurs at `2026-06-24 17:20`.

Non-example:

A static description such as "carrier pickup is important" is not an event. It lacks a specific occurrence, timestamp, affected object, and source.

EKOS implementation repository use:

The EKOS implementation should use events to connect evidence, process state, root-cause hypotheses, and timeline-based reasoning.

---

## 4. Evidence
Definition:

A source-backed record, observation, document, event, approval, timestamp, or reference that supports or weakens a claim.

Why it matters:

Enterprise AI answers must be reviewable. A fluent answer is not enough; the system must show what supports the answer and where that support came from.

SAP logistics delay example:

Evidence for a pickup delay may include planned pickup time, actual pickup time, carrier event record, exception ticket, and the shipment-to-freight-order relationship.

Non-example:

Model confidence is not evidence. A statement that "the delay was probably caused by the carrier" is not evidence unless linked to supporting records.

EKOS implementation repository use:

The EKOS implementation should expose evidence references in structured context and final answers so reviewers can trace conclusions back to source records.

---

## 5. Policy
Definition:

A business rule, contractual rule, compliance constraint, internal control, or operating procedure that limits or requires action.

Why it matters:

AI systems should not only answer what happened. They must know what actions are allowed, risky, required, or blocked by policy.

SAP logistics delay example:

`POL-Delay-Approval` requires manager approval before customer-facing commitment changes.

Non-example:

A recommendation such as "notify the customer" is not a policy. A policy defines the constraint that determines whether, when, and by whom that notification or commitment change may happen.

EKOS implementation repository use:

The EKOS implementation should use policies to determine execution boundaries, human review points, and whether an output is an answer, recommendation, draft, or action.

---

## 6. Role
Definition:

A person, team, system, partner, or organizational function with responsibility, authority, or accountability in a business process.

Why it matters:

Enterprise decisions have owners. AI systems need role context to know who should review, approve, receive, or execute the next step.

SAP logistics delay example:

Logistics operations may open an exception. A logistics manager may approve customer commitment changes. Customer service may communicate with the customer. A carrier may provide pickup event updates.

Non-example:

A username by itself is not a role. A role is defined by responsibility and authority in the process, not only by identity.

EKOS implementation repository use:

The EKOS implementation should use roles to identify responsibility, escalation points, approval requirements, and output ownership.

---

## 7. Relationship
Definition:

A typed connection between business objects, events, policies, roles, evidence, or process states.

Why it matters:

Enterprise understanding depends on knowing how things are connected. Retrieval can find related text, but EKOS needs explicit relationship paths that explain why objects matter to each other.

SAP logistics delay example:

`Delivery D-10045` is included in `Shipment S-7781`. `Shipment S-7781` is planned under `Freight Order FO-5510`. `Carrier Event CE-3302` affects `Shipment S-7781`. `POL-Delay-Approval` constrains customer commitment changes.

Non-example:

Textual similarity is not a relationship. Two chunks may sound similar while referring to different deliveries, regions, customers, or process stages.

EKOS implementation repository use:

The EKOS implementation should use relationships to support traversal, context assembly, evidence paths, and evaluation of relationship-grounded answers.

---

## 8. Execution Boundary
Definition:

The explicit limit that defines what an AI system may answer, recommend, draft, escalate, or execute in a given enterprise context.

Why it matters:

Tool access does not equal business authority. EKOS must separate understanding from action and identify when human review or approval is required.

SAP logistics delay example:

An AI system may explain why a delivery was delayed and draft an internal recommendation. It may not change a customer commitment if `POL-Delay-Approval` requires manager approval.

Non-example:

An API permission is not enough to define an execution boundary. The boundary must also include business policy, role authority, evidence, and process context.

EKOS implementation repository use:

The EKOS implementation should use execution boundaries to label outputs and prevent recommendations from being confused with approved actions.

---

## 9. Enterprise Semantic Model
Definition:

A governed model of business objects, processes, events, policies, roles, evidence, relationships, and execution boundaries that represents enterprise meaning for AI systems.

Why it matters:

The enterprise semantic model is the durable meaning layer EKOS provides. It should survive changes in models, prompts, retrieval systems, and agent frameworks.

SAP logistics delay example:

A logistics semantic model defines deliveries, shipments, freight orders, carrier events, customer commitments, exceptions, policies, relationships, evidence links, and allowed action categories.

Non-example:

A database schema alone is not an enterprise semantic model. It may describe storage, but it does not necessarily explain business meaning, evidence, policy, responsibility, or execution boundaries.

EKOS implementation repository use:

The EKOS implementation should use this model as the shared structure that context assembly, retrieval, answer generation, evaluation, and future execution-boundary reasoning consume.

---

## 10. Enterprise Intelligence Infrastructure
Definition:

The durable infrastructure that turns business systems, processes, and evidence into a semantic foundation AI systems can reliably use.

Why it matters:

Enterprise AI should not rebuild meaning inside every prompt, retrieval pipeline, or agent workflow. The meaning layer should be explicit, shared, reviewable, and reusable.

SAP logistics delay example:

EKOS provides enterprise intelligence infrastructure when it can connect delivery records, shipment relationships, carrier events, policies, evidence, roles, and execution boundaries into structured context for AI use.

Non-example:

A chatbot, vector index, ontology demo, or agent workflow is not by itself enterprise intelligence infrastructure. Each can consume or implement part of the infrastructure, but none is the whole concept.

EKOS implementation repository use:

The EKOS implementation should demonstrate this infrastructure by showing that structured enterprise meaning improves object grounding, evidence traceability, relationship traversal, boundary recognition, and model-change stability.

---

## Glossary Review Criteria
A glossary term is acceptable when it:

1. Defines a durable enterprise concept, not a technology trend.
2. Has a clear purpose in EKOS.
3. Can be tested against the SAP logistics delay scenario.
4. Includes a non-example to prevent scope creep.
5. Can guide implementation in `2000silpeed/ekos-sap-knowledge-os` without creating implementation artifacts inside North Star.

## Open Questions
- Which glossary terms should become field names in the EKOS implementation repository?
- Which terms need tighter alignment with SAP terminology?
- Should evidence and event be separate primitives in v1, or should event be treated as one type of evidence?
- Which terms are universal across ERP, CRM, MES, and PLM, and which are logistics-specific?
- How should glossary changes be versioned once implementation work depends on them?

## Next Actions
1. Review these definitions against the EKOS white paper.
2. Inspect `2000silpeed/ekos-sap-knowledge-os` before proposing implementation mappings.
3. Create a cross-repository mapping from glossary terms to existing implementation concepts.
4. Use the glossary as the vocabulary for evaluation criteria and future case-study language.
