# EKOS Glossary Roadmap

Date: 2026-06-27

Status: Conceptual taxonomy draft

Sources:

- `docs/whitepaper/the-ekos-white-paper.md`
- `docs/research/ekos-validation-matrix.md`

Purpose: Identify, cluster, deduplicate, and flag ambiguous enterprise concepts currently used in the EKOS white paper and validation matrix.

Non-goal: This document does not define terms. Definitions belong in later glossary work.

Repository boundary: This is a North Star research and glossary-planning artifact. It does not create implementation code, prototype data, or runnable assets.

## Classification Rules

- Core Concepts: Concepts that appear to be foundational to the EKOS thesis and should be considered first-class glossary candidates.
- Derived Concepts: Concepts composed from, or dependent on, core concepts.
- Implementation Concepts: Concepts used to describe implementation, evaluation, technology, repository coordination, or prototype mechanics.
- Deprecated Concepts: Labels or framings used in the white paper as rejected, unsafe, or misleading directions.
- Future Concepts: Concepts mentioned as later requirements, later domains, or production concerns, but not yet ready for current glossary definition.

Each concept appears in only one primary category below.

## Duplicate Normalization Map

This map removes duplicate wording before later glossary definition work.

| Normalize under | White paper variants to collapse |
| --- | --- |
| Enterprise Meaning | durable enterprise meaning, business meaning, enterprise semantics, enterprise structure, enterprise vocabulary |
| Semantic Foundation | stable representation of enterprise meaning, stable enterprise meaning layer, durable semantic foundation |
| Enterprise Context | business context, semantic context, AI-usable context, EKOS context, structured context |
| Enterprise Intelligence Infrastructure | enterprise intelligence layer, semantic infrastructure, meaning infrastructure |
| Business Object | durable enterprise entity, primary object, related object, core business object |
| Object Identity | business object identity, identity resolution, primary business object identification |
| Business Process | enterprise workflow, workflow, process, operational workflow |
| Process State | lifecycle state, object state, current status, process context |
| Event | business event, carrier event, tracking event, state-changing event |
| Evidence | source reference, evidence record, evidence reference, supporting record, citation |
| Evidence Path | evidence chain, evidence trail, reasoning path, traceable support |
| Policy | rule, constraint, control, operating procedure, service-level commitment |
| Role | responsibility, owner role, responsible role, role ownership |
| Authority | permission, approval authority, business authority, semantic authority |
| Relationship | typed relationship, object relationship, relationship model |
| Relationship Path | traversal path, business chain, connected business context |
| Execution Boundary | action boundary, action limit, execution limit, boundary |
| Human Review | human approval, review point, manager approval, reviewer notes |
| Governance | reviewability, versioning, auditability, controlled evolution |
| Retrieval-Only Baseline | baseline system, retrieval-first approach, document-centric baseline |
| EKOS-Backed System | EKOS-backed answering, EKOS-backed answer generation, EKOS-backed context |
| Model-Change Stability | model independence, stable answers across model changes |
| Public-Safe Data | synthetic data, anonymized data, public-safe dataset |
| Claim | technical claim, explicit claim, implicit claim, hypothesis |
| Supporting Reasoning | existing supporting reasoning, internal reasoning, rationale |
| Missing Evidence | evidence gap, unproven claim, missing experiment |
| Experimental Validation | validation method, how to validate, experiment plan |
| Benchmark | suggested benchmark, evaluation, test, rubric |
| Confidence Level | current confidence, confidence scale, confidence label |
| Falsification Condition | risks if false, fail condition, rejection threshold |

## Core Concepts

### Enterprise Meaning Cluster

- Enterprise Meaning
- Semantic Foundation
- Enterprise Context
- Enterprise Intelligence Infrastructure
- Enterprise Semantic Model
- Semantic Layer
- Meaning Layer
- Semantic Infrastructure
- Business Context
- Enterprise Understanding
- Enterprise AI Understanding Gap

### Enterprise Structure Cluster

- Business Object
- Object Identity
- Business Process
- Process State
- Lifecycle
- Event
- Policy
- Role
- Responsibility
- Authority
- Evidence
- Claim
- Relationship
- Execution Boundary
- Human Review
- Governance
- Source System
- System of Record
- Source Reference
- Provenance

### Enterprise Quality Attributes Cluster

- Explicitness
- Connectedness
- Reviewability
- Versioning
- Reusability
- Governed Meaning
- Traceability
- Auditability
- Accountability
- Semantic Consistency
- Operational Accuracy
- Operational Boundary
- Control

### AI Consumption Cluster

- AI System
- Model
- Agent
- Assistant
- Retrieval System
- Tool-Calling Model
- MCP-Connected Workflow
- Human-in-the-Loop Automation
- AI Consumer
- Enterprise Intelligence Consumer

## Derived Concepts

### Context and Reasoning Cluster

- Meaning Construction Flow
- Question Answering Flow
- Action Flow
- Context Assembly
- Context Assembler Output
- AI-Usable Business Context
- Business Situation
- Business Chain
- Reasoning Path
- Root Cause
- Root Cause Claim
- Root Cause Hypothesis
- Unsupported Claim
- Fact
- Hypothesis
- Recommendation
- Draft
- Execution Step
- Escalation
- Customer-Facing Commitment Change

### Evidence and Review Cluster

- Evidence Path
- Evidence Chain
- Evidence Trail
- Evidence Grounding
- Evidence Traceability
- Evidence Record
- Evidence Link
- Claim-Supported Evidence
- Conflicting Evidence
- Stale Evidence
- Confidence Basis
- Review Requirement
- Human Review Requirement
- Semantic Authority
- Approval Requirement
- Approval Boundary

### Object and Relationship Cluster

- Primary Business Object
- Related Object
- Affected Object
- Object Relationship
- Relationship Path
- Relationship Traversal
- Object Grounding
- Object Identification
- Object Identification Accuracy
- Relationship Traversal Accuracy
- Primary Object Confusion
- Identity Resolution
- Cross-System Relationship

### Process and Action Cluster

- Process State Awareness
- Event Interpretation
- State Change
- Timeline-Based Reasoning
- Event Timestamp Order
- Policy Applicability
- Policy Constraint
- Boundary Recognition
- Action Category
- Allowed Action
- Risky Action
- Prohibited Action
- Required Action
- Direct Execution
- Live Execution
- Read-Only Understanding
- Read-Only Action

### Evaluation Cluster

- Enterprise AI Behavior
- Evaluation Scope
- Evaluation Dataset
- Evaluation Question
- Evaluation Metric
- Scoring Rubric
- Qualitative Review
- Reviewer Notes
- Failure Mode
- Test Case
- Regression Case
- Baseline Score
- EKOS Score
- Model-Change Stability
- Model Independence
- Fact/Hypothesis/Recommendation Separation
- Policy and Boundary Recognition

### Validation Matrix Cluster

- Claim ID
- Claim Matrix
- Exact Claim
- Explicit Claim
- Implicit Claim
- Technical Claim
- Testable Claim
- Core Hypothesis
- Supporting Reasoning
- Missing Evidence
- Experimental Validation
- Validation Method
- Risk if False
- Suggested Benchmark
- Suggested Evaluation
- Confidence Level
- Confidence Scale
- Current Confidence
- Conceptual Plausibility
- Prototype Evidence
- Production Evidence
- Research Program
- Immediate Research Question
- Next Research Step
- Claim Hygiene
- Claim Scope
- Architecture Validation
- Production Readiness Claim
- Business Impact Claim
- Generalization Claim
- Cross-Domain Validation
- Falsification Condition
- Rejection Threshold
- Baseline Tie Condition
- Score Delta
- Ablation Study
- Inter-Rater Reliability
- Domain Reviewer Realism Score

### SAP Logistics Scenario Cluster

- SAP Logistics Delay Analysis
- Logistics Delay
- Logistics Fulfillment Process
- Logistics Exception
- Logistics Operations
- Delivery
- Delivery ID
- Delivery Status
- Delivery Note
- Sales Order
- Shipment
- Shipment ID
- Shipment Status
- Freight Order
- Freight Order ID
- Carrier
- Carrier Assignment
- Carrier Event
- Carrier Pickup Delay
- Carrier Capacity Shortage
- Carrier Reassignment
- Carrier Performance Review
- Tracking Event
- Tracking Note
- Customer
- Customer ID
- Customer Commitment
- Customer Commitment Date
- Customer Commitment Risk
- Customer Escalation
- Customer Notification
- Exception
- Exception Ticket
- Exception Status
- Plant
- Warehouse
- Warehouse Processing Delay
- Warehouse Capacity Issue
- Goods Issue
- Planned Goods Issue Date
- Actual Goods Issue Date
- Pickup
- Planned Pickup Time
- Actual Pickup Time
- Arrival
- Planned Arrival Time
- Actual Arrival Time
- Transit
- Customs Clearance
- Customs Hold
- Goods Receipt
- Billing
- Billing Consequence
- Billing Dispute Investigation
- Invoice
- Invoice Blocked
- Purchase Order
- Material
- Container
- Route
- Transportation Mode
- Planned Cost
- Location
- Event Description
- Promised Date
- Service-Level Category
- Escalation Priority
- Documentation Hold
- Export Documentation

## Implementation Concepts

### Architecture Layer Cluster

- EKOS Architecture
- Layered Architecture
- Source Systems Layer
- Enterprise Semantic Model Layer
- Evidence Layer
- Retrieval and Reasoning Layer
- Execution Boundary Layer
- Agent Layer
- Governance Layer
- Minimal Reference Architecture
- Semantic Operating Layer
- Operating Substrate
- Platform
- Pipeline

### Retrieval and AI Technology Cluster

- Retrieval
- Document Retrieval
- Vector Similarity
- Vector Search
- Semantic Retrieval
- Generic RAG
- GraphRAG
- Knowledge Graph
- Ontology
- Graph Traversal
- Query Planning
- Business Rule Check
- Tool Calling
- API
- MCP
- Agent Framework
- AI Assistant
- Prompt
- Context Window
- Answer Generation
- Prompt-Based Answer Generation
- Vector Store
- Graph Engine
- Graph Storage
- Database
- LLM Provider

### Enterprise System Cluster

- ERP
- CRM
- MES
- PLM
- Workflow System
- Ticketing System
- Document Repository
- Event Log
- External Partner Feed
- SAP
- SAP System
- Private SAP System
- Live SAP Integration
- Source Record
- System-Generated Fact
- External Partner Update

### Prototype and Evaluation Build Cluster

- EKOS Prototype
- First Prototype
- Synthetic SAP Logistics Dataset
- Synthetic Scenario
- Public-Safe Dataset
- Case Fixture
- Case A: Carrier Pickup Delay
- Case B: Warehouse Processing Delay
- Case C: Documentation Hold
- Case D: False Delay Signal
- Case E: Conflicting Evidence
- Retrieval-Only Baseline
- EKOS-Backed System
- EKOS-Backed Answering
- Evaluation Harness
- Evaluation Report
- Failure Mode Tracking
- Structured Output
- Structured Report
- Public Demo
- Public Case Study
- Runnable Walkthrough
- Demo Surface

### Repository Coordination Cluster

- North Star
- EKOS Implementation Repository
- Repository Boundary
- Cross-Repository Alignment
- ADR
- Glossary Artifact
- Evaluation Specification
- Public Narrative
- Portfolio Positioning
- Implementation Issue
- Pull Request
- Milestone
- Release
- README Link
- Implementation Roadmap
- Coordination Milestone

### Concrete Representation Cluster

- JSON
- SQLite
- Typed Code
- Schema Name
- Output Field
- Relationship Type
- Context Assembler
- Context Assembly Logic
- Data Generation Script
- Validation Script
- Semantic Model Implementation
- Runnable Harness
- CLI
- Notebook
- Web UI

## Deprecated Concepts

Deprecated here means "do not promote as an EKOS glossary concept or product identity." Some terms may still remain useful as comparison points.

### Rejected EKOS Identity Labels

- EKOS as chatbot
- EKOS as AI assistant
- EKOS as ontology only
- EKOS as ontology demo
- EKOS as knowledge graph only
- EKOS as knowledge graph demo
- EKOS as GraphRAG only
- EKOS as RAG pattern
- EKOS as agent framework
- EKOS as application UI
- EKOS as ERP replacement
- EKOS as CRM replacement
- EKOS as MES replacement
- EKOS as PLM replacement
- EKOS as ticketing system replacement
- EKOS as document repository replacement
- EKOS as workflow engine replacement

### Rejected Reasoning Equivalences

- Retrieval as understanding
- Vector similarity as business equivalence
- Tool access as business judgment
- API permission as execution boundary
- MCP capability as authority
- Model confidence as evidence
- Fluent answer as enterprise understanding
- Citation-only evidence model
- Prompt-only enterprise meaning
- Application-code-only enterprise meaning
- Document-only enterprise meaning

### Rejected v0.1 Claims

- Production-ready EKOS
- Production-ready SAP environment
- Measured business ROI from synthetic prototype
- Full workflow automation in first prototype
- Direct customer communication automation in first prototype
- Complete governance solution
- Complete security model
- Generalization to all enterprise domains
- Removal of human review
- Live source-system updates in first prototype

### Rejected North Star Directions

- North Star prototype directory
- North Star app implementation
- North Star runnable demo
- North Star sample dataset
- North Star graph database
- North Star CLI
- North Star runtime
- North Star test harness
- North Star API server

## Future Concepts

### Production Governance Cluster

- Access Control
- Approval Workflow
- Audit Trail
- Change Management
- Rollback
- Ownership
- Organizational Process
- Semantic Model Review
- Evidence Review
- Policy Update
- Human Feedback
- Evaluation Governance

### Security and Privacy Cluster

- Role-Based Access Control
- Data Privacy
- Data Minimization
- Sensitive Data Handling
- Threat Model
- Security Model
- Access-Control Design
- Privacy Constraint
- Audit Log
- Approval History
- Secure Tool Execution
- Secure Integration
- Permission Model

### Production Readiness Cluster

- Production Deployment
- Production Integration
- Integration Issue
- Latency Constraint
- Data Quality Issue
- Incomplete Record
- Inconsistent Process Handling
- Historical Context
- Real Enterprise Data
- Private Enterprise Data
- Real Workflow Evidence
- Private-Data Prototype
- Live-Integration Prototype
- Organizational Review Process

### Business Impact Cluster

- Reduced Time to Investigate Exceptions
- Fewer Incorrect Escalations
- Faster Evidence Gathering
- Better Audit Readiness
- Improved Consistency Across Analysts
- Operational Savings
- Business Impact Measurement

### Future Domain Cluster

- Procurement Approval
- Order-to-Cash Exception
- Customer Service Escalation
- Manufacturing Quality Event
- Financial Close Control
- HR Policy Workflow

## Ambiguous Concepts to Resolve Later

This section lists concept boundaries that need later clarification. It does not resolve or define them.

| Ambiguous area | Competing labels in current white paper | Resolution question |
| --- | --- | --- |
| Enterprise meaning vocabulary | Enterprise Meaning, Enterprise Context, Business Context, Semantic Context, Enterprise Semantics | Which label should be canonical, and which should become aliases? |
| Infrastructure label | Enterprise Intelligence Infrastructure, Semantic Layer, Meaning Layer, Semantic Foundation | Which label names the product-level idea versus the architectural layer? |
| Evidence boundary | Evidence, Event, Source Reference, Citation, Evidence Record | Which items are evidence primitives versus evidence sources? |
| Policy vocabulary | Policy, Rule, Constraint, Control, Service-Level Commitment | Should these become separate glossary terms or one cluster? |
| Role and authority | Role, Responsibility, Owner, Authority, Accountability | Which terms represent organizational ownership versus permission to act? |
| Execution boundary | Execution Boundary, Action Boundary, API Permission, MCP Capability, Human Approval | Which labels belong to EKOS semantics versus external system permissions? |
| Process vocabulary | Business Process, Workflow, Process State, Lifecycle, Operational Workflow | Which terms are general and which are state-specific? |
| Source system vocabulary | Source System, Enterprise System, System of Record, External Partner Feed | Which terms are authoritative sources versus supporting sources? |
| AI actor vocabulary | AI System, Model, Agent, Assistant, Tool-Calling Model | Which terms identify consumers of EKOS versus implementation actors? |
| Graph vocabulary | Knowledge Graph, GraphRAG, Relationship Graph, Semantic Model | Which graph-related labels are implementation choices versus EKOS concepts? |
| Governance vocabulary | Governance, Human Review, Approval, Auditability, Versioning | Which are governance mechanisms versus quality attributes? |
| Prototype success vocabulary | Architecture Validation, Prototype Result, Production Readiness, Business Impact | Which claims are allowed after synthetic evaluation? |

## Roadmap Implications

1. Keep the existing ten glossary terms as the current core baseline.
2. Add no new definitions until ambiguous concept boundaries are resolved.
3. Treat SAP logistics objects as scenario-specific terms before promoting any of them to universal EKOS glossary terms.
4. Treat implementation concepts as mappings to `2000silpeed/ekos-sap-knowledge-os`, not as North Star implementation plans.
5. Treat deprecated concepts as guardrails for public narrative and future white paper edits.
6. Treat future concepts as backlog candidates for later research notes, ADRs, or glossary expansions.

## Suggested Next Glossary Work

1. Decide canonical labels for the ambiguous areas.
2. Create a concept dependency map from core concepts to derived concepts.
3. Decide which SAP logistics scenario terms need short glossary entries and which should remain example-only.
4. Align the roadmap with the validation matrix in `docs/research/ekos-validation-matrix.md`.
5. Only after that, write or revise definitions.
