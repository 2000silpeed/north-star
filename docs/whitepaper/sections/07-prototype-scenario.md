# 7. Prototype Scenario

Status: Draft v0.1

## Reader-Facing Draft
EKOS should be evaluated through a concrete enterprise workflow, not an abstract demo.

The first prototype scenario should be SAP logistics delay analysis.

This scenario is narrow enough to implement with synthetic data, but rich enough to test the core EKOS thesis: enterprise AI needs explicit business objects, relationships, evidence, policies, and execution boundaries to produce reliable answers.

The prototype should be read-only first. It should answer questions, trace evidence, explain business context, and identify action boundaries. It should not directly update enterprise systems in the first version.

## Scenario Overview
A customer delivery is delayed.

The business wants to know:

> Why was this delivery delayed, what business objects were involved, what evidence supports the answer, and what action should be taken next?

A generic AI system may retrieve tracking notes, delivery records, SOP documents, and emails. It may produce a plausible explanation.

EKOS should go further. It should identify the primary business object, traverse related objects, detect the process state, assemble evidence, distinguish facts from hypotheses, and identify whether the next action is a recommendation or an execution step requiring human review.

## Why This Scenario Fits EKOS
Logistics delay analysis is a strong prototype because it contains the core complexity of enterprise AI in a compact form.

It requires:

- Business object identity
- Cross-system relationships
- Event interpretation
- Process state understanding
- Evidence tracing
- Policy constraints
- Role ownership
- Action boundaries

The problem is easy to understand, but difficult to answer reliably without enterprise context.

## Synthetic Data Requirement
The prototype must not use private company data.

All data should be synthetic, anonymized, or publicly safe.

Synthetic data should still look operationally realistic. It should include identifiers, timestamps, object relationships, event history, process states, and evidence records.

The goal is not to simulate a full SAP system. The goal is to model enough enterprise structure to test whether EKOS improves understanding over retrieval-only approaches.

## Core Business Objects
The prototype should model the following objects.

### Delivery
The primary object of analysis.

Fields may include:

- Delivery ID
- Customer ID
- Plant
- Planned goods issue date
- Actual goods issue date
- Delivery status
- Related sales order
- Related shipment
- Customer commitment date

### Shipment
The logistics movement that includes one or more deliveries.

Fields may include:

- Shipment ID
- Carrier
- Planned pickup time
- Actual pickup time
- Planned arrival time
- Actual arrival time
- Shipment status
- Related freight order

### Freight Order
The transportation planning object.

Fields may include:

- Freight order ID
- Carrier assignment
- Route
- Transportation mode
- Planned cost
- Exception status
- Related shipment

### Carrier Event
An operational event reported by a carrier or tracking system.

Fields may include:

- Event ID
- Event type
- Timestamp
- Location
- Source
- Related shipment
- Related freight order
- Event description

### Customer Commitment
The promised delivery expectation.

Fields may include:

- Commitment ID
- Customer ID
- Delivery ID
- Promised date
- Service-level category
- Escalation priority

### Exception
A business issue requiring attention.

Fields may include:

- Exception ID
- Exception type
- Severity
- Created timestamp
- Owner role
- Current status
- Related delivery
- Related shipment
- Evidence references

### Policy
A rule or constraint that affects next actions.

Fields may include:

- Policy ID
- Policy type
- Applicability
- Approval requirement
- Role requirement
- Execution boundary

## Relationship Model
The scenario should include explicit relationships.

Examples:

- Delivery belongs to Sales Order
- Delivery is included in Shipment
- Shipment is planned by Freight Order
- Freight Order is assigned to Carrier
- Carrier Event affects Shipment
- Shipment delay affects Customer Commitment
- Exception is opened for Delivery
- Policy constrains Recommended Action
- Evidence supports Delay Claim

These relationships allow EKOS to answer questions through business context rather than isolated retrieval.

## Example Synthetic Situation
The prototype can use a synthetic situation like this:

```text
Delivery D-10045 was planned to ship from Plant P-10 to Customer C-200.
The delivery was included in Shipment S-7781.
Shipment S-7781 was planned under Freight Order FO-5510.
Carrier K-Line was assigned to FO-5510.
A carrier event reported "pickup missed due to capacity shortage" at 2026-06-24 09:30.
The planned pickup time was 2026-06-24 08:00.
Actual pickup occurred at 2026-06-24 17:20.
Customer commitment date was 2026-06-25.
An exception ticket EX-9007 was opened by logistics operations.
Policy POL-Delay-Approval requires manager approval before customer-facing commitment changes.
```

This data is synthetic. It should be used only to demonstrate the architecture.

## Example User Questions
The prototype should answer questions such as:

### Root Cause
> Why was delivery D-10045 delayed?

Expected answer:

- Primary object: Delivery D-10045
- Related shipment: S-7781
- Related freight order: FO-5510
- Root cause hypothesis: missed carrier pickup due to capacity shortage
- Evidence: carrier event, planned pickup time, actual pickup time, exception ticket
- Confidence basis: aligned timestamps and event relationship

### Business Impact
> Which customer commitment is affected?

Expected answer:

- Affected customer commitment
- Promised date
- Delay risk
- Related delivery and shipment
- Evidence path

### Evidence
> What evidence supports the delay reason?

Expected answer:

- Carrier event ID and timestamp
- Planned versus actual pickup
- Related exception ticket
- Source-system references
- Relationship path connecting evidence to delivery

### Next Action
> What should happen next?

Expected answer:

- Recommended action
- Responsible role
- Required approval
- Execution boundary
- Human review point
- Draft message or ticket update only if allowed

## Expected EKOS Output Shape
The system should not return only a paragraph.

It should produce structured output that separates:

- Business object
- Related objects
- Process state
- Evidence
- Root cause hypothesis
- Business impact
- Recommendation
- Execution boundary
- Human review requirement

Example:

```text
Primary object:
  Delivery D-10045

Related objects:
  Shipment S-7781
  Freight Order FO-5510
  Carrier Event CE-3302
  Exception EX-9007
  Customer Commitment CC-1201

Root cause hypothesis:
  Carrier pickup was missed due to capacity shortage.

Evidence:
  CE-3302 reported missed pickup at 2026-06-24 09:30.
  Planned pickup was 2026-06-24 08:00.
  Actual pickup was 2026-06-24 17:20.
  EX-9007 was opened for the affected delivery.

Business impact:
  Customer commitment CC-1201 is at risk.

Recommended next step:
  Logistics operations should review delay impact and prepare customer update.

Execution boundary:
  AI may recommend and draft.
  Manager approval is required before changing customer-facing commitment.
```

## What EKOS Should Demonstrate
The prototype should demonstrate five capabilities.

### 1. Object Identification
The system should identify the primary business object and avoid confusing it with related objects.

For example, the delivery is the object being investigated. The shipment, freight order, event, and exception are related context.

### 2. Relationship Traversal
The system should follow business relationships across objects.

For example:

```text
Delivery -> Shipment -> Freight Order -> Carrier Event
Delivery -> Customer Commitment
Delivery -> Exception
```

### 3. Evidence Grounding
The system should connect every important claim to evidence.

For example, "missed pickup caused the delay" must be supported by the carrier event and timestamp comparison.

### 4. Boundary Recognition
The system should distinguish answer, recommendation, draft, and execution.

For example, it may recommend a customer update but should not change the commitment date without approval.

### 5. Model Independence
The system should preserve enterprise context independently from the model used to answer the question.

If the model changes, the business objects, relationships, evidence, and policies should remain stable.

## Baseline Comparison
The prototype should compare EKOS against a retrieval-only baseline.

The baseline system may retrieve:

- SOP documents
- Delivery notes
- Tracking events
- Exception ticket text

The EKOS-backed system should additionally provide:

- Object identity
- Relationship path
- Process state
- Evidence chain
- Policy constraint
- Execution boundary

The evaluation should ask whether EKOS produces answers that are more traceable, more operationally accurate, and less likely to confuse recommendation with execution.

## Success Criteria
The prototype succeeds if it can:

- Identify the correct primary business object
- Traverse related objects
- Explain the delay with evidence
- Separate fact, hypothesis, recommendation, and execution
- Identify human review requirements
- Preserve source references
- Produce consistent answers when the model changes

## Non-Goals
The first prototype should not attempt to:

- Connect to a real private SAP system
- Execute live updates
- Automate customer communication directly
- Claim measured business impact
- Model all logistics scenarios
- Replace human logistics operations

These are future possibilities, not first-version requirements.

## Key Claims
- SAP logistics delay analysis is a strong first EKOS scenario because it requires object identity, relationships, evidence, policy, and execution boundaries.
- The first prototype should use synthetic data and remain read-only.
- The goal is to prove enterprise understanding before workflow automation.
- EKOS should be evaluated against a retrieval-only baseline.
- A useful prototype must show evidence paths and action boundaries, not only fluent answers.

## Reasoning Preserved for Future Drafts
This section intentionally keeps the first scenario narrow.

Rejected framing:

- "Build a complete SAP AI platform first."
- "Connect to real company data immediately."
- "Automate the logistics workflow end-to-end."

Preferred framing:

- "Prove read-only enterprise understanding with synthetic SAP logistics data."

This keeps the prototype feasible, public-safe, and aligned with the design principles.

## Open Questions
- Which synthetic object IDs and sample records should be standardized for the first demo?
- Should the first prototype use a graph database, relational schema, or simple JSON graph?
- Should the baseline comparison be implemented as a separate retrieval-only demo?
- Which UI is best for the first demo: CLI, notebook, API, or simple web app?
- Should the prototype include a generated evidence graph visualization?

## Next Section Bridge
Section 8 should define evaluation. It should specify how EKOS will be judged against retrieval-only systems: object identification accuracy, evidence traceability, relationship traversal, boundary recognition, and model-change stability.
