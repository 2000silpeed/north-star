# EKOS Validation Matrix

Date: 2026-06-27

Status: Research program draft

Scope: Current EKOS white paper and supporting North Star research memory.

Primary output: A validation matrix that turns the EKOS thesis into testable claims. This is not new white paper content and does not modify the white paper.

Implementation boundary: Experiments, datasets, runnable benchmarks, and harnesses belong in `2000silpeed/ekos-sap-knowledge-os`. North Star owns this research specification and the reasoning memory behind it.

## Repository Reading Scope

This matrix was created after reading the tracked North Star repository, including:

- `AGENTS.md`
- `README.md`
- `AIOS/`
- `docs/adr/`
- `docs/diary/`
- `docs/glossary/`
- `docs/methodology/`
- `docs/research/`
- `docs/whitepaper/`
- `docs/whitepaper-ko/`

The direct claim source is the current assembled EKOS white paper at `docs/whitepaper/the-ekos-white-paper.md`, cross-checked against the section drafts in `docs/whitepaper/sections/`.

## Method

Claims are separated into explicit and implicit claims.

- Explicit claims are stated directly in the white paper.
- Implicit claims are architectural assumptions required for the explicit claims to hold.
- Current confidence means confidence before prototype evidence. It measures plausibility and internal support, not empirical proof.
- Confidence levels are intentionally conservative:
  - High: strongly supported by internal reasoning and low-risk to state.
  - Medium: plausible and central, but requires prototype evidence.
  - Low: important but currently under-evidenced or likely domain-dependent.

## Claim Matrix

### EKOS-CLAIM-001

- Claim ID: EKOS-CLAIM-001
- Exact claim: "Modern AI systems can read documents, call tools, generate code, and coordinate workflows, but they do not automatically understand how enterprises operate."
- Why this claim matters: This is the entry point for EKOS. If current AI capabilities already produced enterprise understanding, EKOS would be unnecessary.
- Existing supporting reasoning: The white paper distinguishes AI capability from enterprise understanding and shows that enterprise work depends on objects, process states, policies, roles, evidence, and boundaries.
- Missing evidence: Comparative evidence showing capable models failing enterprise-structure questions even when documents or tools are available.
- How the claim could be experimentally validated: Give a retrieval-only or tool-using model realistic enterprise delay cases and score whether it identifies primary objects, process state, evidence, and boundaries without an explicit semantic layer.
- Risks if the claim is false: EKOS becomes an unnecessary abstraction over model/tool improvements.
- Suggested benchmark or evaluation: Retrieval/tool baseline on synthetic SAP logistics cases, scored with the Section 8 seven-metric rubric.
- Current confidence level: High for the conceptual claim; Medium until demonstrated with controlled baseline results.

### EKOS-CLAIM-002

- Claim ID: EKOS-CLAIM-002
- Exact claim: "Enterprise work is not stored in one place."
- Why this claim matters: EKOS depends on the premise that enterprise meaning must be reconstructed across multiple systems and sources.
- Existing supporting reasoning: The white paper lists ERP tables, CRM records, workflow systems, documents, approvals, policies, emails, spreadsheets, logs, and tacit knowledge as distributed sources.
- Missing evidence: Real or synthetic source maps showing how one business situation spans multiple records and systems.
- How the claim could be experimentally validated: Build a source coverage map for each synthetic logistics case showing which facts live in which source category.
- Risks if the claim is false: A simpler single-system integration or document retrieval system may be enough.
- Suggested benchmark or evaluation: Source fragmentation score per case: number of required source types, cross-source joins, and missing-context risks.
- Current confidence level: High.

### EKOS-CLAIM-003

- Claim ID: EKOS-CLAIM-003
- Exact claim: "The gap is not only a data-access problem. It is a meaning problem."
- Why this claim matters: It shifts the research question from access and retrieval to semantic representation.
- Existing supporting reasoning: The white paper argues that a model can retrieve fragments while missing the meaning that connects them.
- Missing evidence: Failure cases where all relevant facts are accessible but the system still gives a wrong or incomplete enterprise answer.
- How the claim could be experimentally validated: Create cases where baseline retrieval includes all relevant snippets, then test whether the system still fails object identity, relationship path, or boundary recognition.
- Risks if the claim is false: EKOS overweights semantic modeling when better connectors, prompts, or retrieval may solve the problem.
- Suggested benchmark or evaluation: "Full-context retrieval" baseline where all relevant text is supplied, then scored against EKOS-assembled semantic context.
- Current confidence level: Medium-High.

### EKOS-CLAIM-004

- Claim ID: EKOS-CLAIM-004
- Exact claim: "Enterprise questions cannot be answered reliably by text retrieval alone. They require a stable representation of enterprise meaning."
- Why this claim matters: This is one of the strongest technical claims against retrieval-only enterprise AI.
- Existing supporting reasoning: The white paper lists unanswered logistics questions: primary object, relevant related objects, state-changing event, applicable policy, evidence, and action authority.
- Missing evidence: Measured retrieval-only failure rates across cases with multiple objects, policies, and evidence conflicts.
- How the claim could be experimentally validated: Compare retrieval-only answers against EKOS-backed answers on the same synthetic dataset and score relationship traversal, evidence traceability, and boundary recognition.
- Risks if the claim is false: The EKOS semantic layer may add complexity without measurable gains.
- Suggested benchmark or evaluation: Retrieval-only vs EKOS-backed score delta on all five initial logistics cases.
- Current confidence level: Medium.

### EKOS-CLAIM-005

- Claim ID: EKOS-CLAIM-005
- Exact claim: "In an enterprise workflow, the answer must be traceable, bounded, and actionable."
- Why this claim matters: It defines enterprise answer quality differently from ordinary chatbot quality.
- Existing supporting reasoning: The white paper argues that enterprise systems must distinguish fact, hypothesis, recommendation, and execution step, and must know when human review is required.
- Missing evidence: Domain-expert agreement that these dimensions are required for the selected logistics workflow.
- How the claim could be experimentally validated: Ask enterprise reviewers to rate outputs with and without traceability, boundary statements, and action classification.
- Risks if the claim is false: EKOS may optimize for qualities that are not actually required by enterprise users in early scenarios.
- Suggested benchmark or evaluation: Human qualitative review: "Would a domain expert know what to review next?"
- Current confidence level: High for regulated or operational contexts; Medium for all enterprise tasks.

### EKOS-CLAIM-006

- Claim ID: EKOS-CLAIM-006
- Exact claim: "Documents are important evidence. They are not the full enterprise state."
- Why this claim matters: It prevents EKOS from collapsing into document RAG.
- Existing supporting reasoning: The white paper notes that SOPs can describe handling rules but do not know which shipment is delayed, which event caused the delay, or whether approval is required.
- Missing evidence: Case examples where document retrieval returns correct SOPs but lacks live object state or event context.
- How the claim could be experimentally validated: Compare SOP-only retrieval against structured object/event context on questions requiring current state.
- Risks if the claim is false: Document-centric systems may be sufficient for many early EKOS claims.
- Suggested benchmark or evaluation: SOP-only baseline on process-state and policy-applicability questions.
- Current confidence level: High.

### EKOS-CLAIM-007

- Claim ID: EKOS-CLAIM-007
- Exact claim: "Semantic similarity is not the same as business equivalence."
- Why this claim matters: This is a precise technical critique of vector-search-only architectures.
- Existing supporting reasoning: The white paper explains that similar language can refer to different objects, while different language can belong to the same process; region, product line, approval status, and lifecycle state can change applicability.
- Missing evidence: Synthetic or real examples where vector-nearest results are semantically similar but operationally wrong.
- How the claim could be experimentally validated: Create distractor records with high textual similarity but different business identity or applicability, then test baseline retrieval errors.
- Risks if the claim is false: Typed identity and relationship modeling may deliver less value than expected.
- Suggested benchmark or evaluation: Distractor set with near-duplicate descriptions but different delivery IDs, regions, process states, or policies.
- Current confidence level: High.

### EKOS-CLAIM-008

- Claim ID: EKOS-CLAIM-008
- Exact claim: "Tool access still does not guarantee understanding."
- Why this claim matters: It separates capability exposure from business judgment.
- Existing supporting reasoning: The white paper states that an API can return status but the model must still know what that status means, and that MCP can expose capability without business judgment.
- Missing evidence: Tool-using baseline failures where the model calls valid tools but chooses the wrong interpretation or action boundary.
- How the claim could be experimentally validated: Give a tool-enabled baseline access to delivery, shipment, carrier event, and ticket lookup functions without EKOS semantic context; score decisions.
- Risks if the claim is false: EKOS may be less important in environments with well-designed tools and strong prompts.
- Suggested benchmark or evaluation: Tool-calling baseline with the same synthetic records and no semantic model, scored on policy and boundary recognition.
- Current confidence level: Medium-High.

### EKOS-CLAIM-009

- Claim ID: EKOS-CLAIM-009
- Exact claim: "Capability is not the same as business judgment."
- Why this claim matters: It grounds the need for execution boundaries instead of treating MCP/API availability as permission.
- Existing supporting reasoning: The white paper argues that the ability to update a ticket or shipment exception does not mean the AI is allowed to do it.
- Missing evidence: Policy examples showing actions that are technically possible but organizationally forbidden or approval-gated.
- How the claim could be experimentally validated: Include cases where a tool exists but policy requires recommendation, draft, escalation, or manager approval.
- Risks if the claim is false: Execution boundary modeling may become redundant with tool-level permissioning.
- Suggested benchmark or evaluation: Policy-gated action cases scored on whether the system refuses direct execution and identifies the approval role.
- Current confidence level: High.

### EKOS-CLAIM-010

- Claim ID: EKOS-CLAIM-010
- Exact claim: "Agents inherit the quality of their operating context."
- Why this claim matters: It positions EKOS below agents, not as another agent framework.
- Existing supporting reasoning: The white paper argues that agents can plan and call tools but still need object identity, state changes, evidence support, and execution boundaries.
- Missing evidence: Agent baseline comparison showing that planning ability alone does not solve enterprise grounding.
- How the claim could be experimentally validated: Run an agent over loose retrieved text and the same agent over EKOS context, then compare failure modes.
- Risks if the claim is false: Agent orchestration may be enough to infer context dynamically, reducing the need for EKOS.
- Suggested benchmark or evaluation: Agent-with-RAG vs agent-with-EKOS context on multi-step logistics questions.
- Current confidence level: Medium.

### EKOS-CLAIM-011

- Claim ID: EKOS-CLAIM-011
- Exact claim: "Enterprise meaning should not be reconstructed from scratch every time a model receives a prompt."
- Why this claim matters: It justifies durable semantic infrastructure rather than prompt-only reasoning.
- Existing supporting reasoning: The white paper says runtime inference is fragile because names, terms, and allowed actions vary by process state.
- Missing evidence: Repeated-run instability showing model outputs drifting when asked to infer the same enterprise meaning from text.
- How the claim could be experimentally validated: Run the same case across prompts and models; measure object identity, evidence, and boundary stability with and without EKOS.
- Risks if the claim is false: Prompt engineering may be enough for early workflows.
- Suggested benchmark or evaluation: Model-change and prompt-change stability test.
- Current confidence level: Medium-High.

### EKOS-CLAIM-012

- Claim ID: EKOS-CLAIM-012
- Exact claim: "Enterprise meaning should become infrastructure for AI systems."
- Why this claim matters: This is the core architectural thesis.
- Existing supporting reasoning: The white paper compares enterprise meaning to identity, permissions, APIs, schemas, and event logs as durable shared foundations.
- Missing evidence: Evidence that a shared semantic layer is reusable across workflows and reduces repeated context construction.
- How the claim could be experimentally validated: Reuse the same semantic model across delay analysis, escalation support, and billing dispute investigation; measure reuse and reduced duplication.
- Risks if the claim is false: EKOS may remain a scenario-specific modeling exercise rather than infrastructure.
- Suggested benchmark or evaluation: Cross-workflow reuse evaluation: percentage of object, relationship, evidence, and policy model reused across scenarios.
- Current confidence level: Medium.

### EKOS-CLAIM-013

- Claim ID: EKOS-CLAIM-013
- Exact claim: "At minimum, enterprise meaning includes business objects, processes, events, policies, roles, evidence, relationships, and execution boundaries."
- Why this claim matters: It defines the minimum EKOS semantic substrate.
- Existing supporting reasoning: The white paper defines each element and explains the failure caused by its absence.
- Missing evidence: Ablation evidence showing which elements are truly required for the first logistics scenario.
- How the claim could be experimentally validated: Remove one semantic element at a time and measure score degradation on the seven metrics.
- Risks if the claim is false: EKOS may over-model early prototypes or miss a more important missing concept.
- Suggested benchmark or evaluation: Semantic component ablation study.
- Current confidence level: Medium.

### EKOS-CLAIM-014

- Claim ID: EKOS-CLAIM-014
- Exact claim: "Without business object identity, AI systems can confuse related records, merge unrelated situations, or reason over fragments without knowing what the primary object is."
- Why this claim matters: Object identity is the first metric in the evaluation rubric.
- Existing supporting reasoning: The white paper uses delivery, shipment, freight order, customer commitment, and exception as distinct but related objects.
- Missing evidence: Measured baseline confusion when multiple related IDs appear in the same context.
- How the claim could be experimentally validated: Create cases with several related objects and score whether systems identify the primary object requested.
- Risks if the claim is false: Object identity may be less central than expected.
- Suggested benchmark or evaluation: Object Identification Accuracy on Cases A-E plus distractor objects.
- Current confidence level: High.

### EKOS-CLAIM-015

- Claim ID: EKOS-CLAIM-015
- Exact claim: "AI systems need process context because the meaning of information changes by state."
- Why this claim matters: It explains why static retrieval over records is insufficient.
- Existing supporting reasoning: The white paper contrasts delays before pickup, after customs clearance, before shipment creation, and after invoicing.
- Missing evidence: Cases where identical terms or events require different interpretation depending on lifecycle state.
- How the claim could be experimentally validated: Build paired cases with similar text but different process states and score whether systems distinguish them.
- Risks if the claim is false: Process-state modeling may add complexity without improving outputs.
- Suggested benchmark or evaluation: Process State Awareness metric with state-paired test cases.
- Current confidence level: Medium-High.

### EKOS-CLAIM-016

- Claim ID: EKOS-CLAIM-016
- Exact claim: "Without events, enterprise AI becomes static. It can describe records, but it cannot reliably explain how a situation evolved."
- Why this claim matters: Delay analysis requires temporal causality, not only current object state.
- Existing supporting reasoning: The white paper describes events as changes in business state with source, time, and consequences.
- Missing evidence: Baseline errors caused by missing event ordering or causal sequence.
- How the claim could be experimentally validated: Include delayed events, reversed timestamps, and conflicting event histories; score causal explanation.
- Risks if the claim is false: Event modeling may be optional for some workflows.
- Suggested benchmark or evaluation: Temporal reasoning tests inside the logistics dataset.
- Current confidence level: Medium.

### EKOS-CLAIM-017

- Claim ID: EKOS-CLAIM-017
- Exact claim: "Without policies, AI recommendations can sound useful while ignoring the constraints that govern real enterprise work."
- Why this claim matters: It links answer quality to governance and operational safety.
- Existing supporting reasoning: The white paper gives rerouting and customer commitment changes as actions that may require approvals or threshold checks.
- Missing evidence: Domain-specific policy examples and reviewer scoring of policy applicability.
- How the claim could be experimentally validated: Include policy-constrained actions and compare whether systems identify allowed, required, risky, or prohibited actions.
- Risks if the claim is false: Policy modeling may be unnecessary for the first read-only prototype.
- Suggested benchmark or evaluation: Policy and Boundary Recognition metric.
- Current confidence level: High for action-capable systems; Medium for read-only answering.

### EKOS-CLAIM-018

- Claim ID: EKOS-CLAIM-018
- Exact claim: "Without roles and responsibilities, AI systems cannot respect organizational boundaries."
- Why this claim matters: Enterprise workflows depend on ownership, approvals, notifications, and accountability.
- Existing supporting reasoning: The white paper lists roles such as logistics planner, warehouse team, carrier, finance, compliance, approver, and system owner.
- Missing evidence: Workflow cases where correct next action depends on role ownership.
- How the claim could be experimentally validated: Add expected owner role to each synthetic case and score whether the system identifies who should review or act.
- Risks if the claim is false: Role modeling may be overkill for initial scenarios.
- Suggested benchmark or evaluation: Responsible-role identification and approval-owner accuracy.
- Current confidence level: Medium.

### EKOS-CLAIM-019

- Claim ID: EKOS-CLAIM-019
- Exact claim: "Evidence must be connected to claims."
- Why this claim matters: It turns explanations into auditable business reasoning.
- Existing supporting reasoning: The white paper says a delay claim should identify the event, source, timestamp, related object, and reasoning path.
- Missing evidence: Scored comparison showing claim-linked evidence improves reviewer trust or correctness.
- How the claim could be experimentally validated: Compare answers with general citations versus claim-level evidence links; have reviewers score auditability.
- Risks if the claim is false: EKOS may spend effort on evidence structure that users do not need.
- Suggested benchmark or evaluation: Evidence Traceability metric requiring evidence ID, timestamp, source, related object, and supported claim.
- Current confidence level: High.

### EKOS-CLAIM-020

- Claim ID: EKOS-CLAIM-020
- Exact claim: "Without relationships, AI can retrieve fragments but cannot follow the business chain."
- Why this claim matters: Relationship traversal is the difference between isolated retrieval and enterprise context.
- Existing supporting reasoning: The white paper uses Delivery -> Shipment -> Freight Order -> Carrier Event and Delivery -> Customer Commitment as example paths.
- Missing evidence: Baseline results showing objects are mentioned but relationship paths are missing or wrong.
- How the claim could be experimentally validated: Score whether answers contain explicit relationship paths connecting primary object to cause, impact, and policy.
- Risks if the claim is false: Graph or relationship modeling may not improve output quality enough to justify it.
- Suggested benchmark or evaluation: Relationship Traversal Accuracy metric.
- Current confidence level: Medium-High.

### EKOS-CLAIM-021

- Claim ID: EKOS-CLAIM-021
- Exact claim: "Without execution boundaries, AI systems risk turning uncertain reasoning into operational action."
- Why this claim matters: This claim supports EKOS's safety architecture for tool-using and agentic systems.
- Existing supporting reasoning: The white paper states that once AI can call tools, update records, create tickets, trigger workflows, or send messages, meaning must be connected to authority.
- Missing evidence: Case evidence where an AI recommendation would become unsafe if directly executed.
- How the claim could be experimentally validated: Include questions asking whether the system is allowed to execute; score direct-execution refusals and escalation behavior.
- Risks if the claim is false: Execution boundary layer may be seen as unnecessary overhead.
- Suggested benchmark or evaluation: Boundary-recognition tests with direct-execute traps.
- Current confidence level: High.

### EKOS-CLAIM-022

- Claim ID: EKOS-CLAIM-022
- Exact claim: "Enterprise meaning should be explicit, connected, reviewable, versioned, reusable, and governed."
- Why this claim matters: This defines the quality standard for the EKOS semantic layer.
- Existing supporting reasoning: The white paper contrasts durable shared infrastructure with prompts, documents, and application code.
- Missing evidence: Concrete governance workflow showing how semantic changes are reviewed, versioned, and reused.
- How the claim could be experimentally validated: Create versioned semantic model changes and audit whether outputs remain explainable across changes.
- Risks if the claim is false: EKOS may become heavier than necessary, or governance requirements may slow useful iteration.
- Suggested benchmark or evaluation: Semantic change audit: every definition or relationship change has reviewer, version, rationale, affected cases, and rollback path.
- Current confidence level: Medium.

### EKOS-CLAIM-023

- Claim ID: EKOS-CLAIM-023
- Exact claim: "EKOS is enterprise intelligence infrastructure: a semantic layer between enterprise systems and AI systems."
- Why this claim matters: It defines the system boundary and product category.
- Existing supporting reasoning: The white paper repeatedly positions EKOS between source systems and AI consumers, rather than as a chatbot, graph demo, RAG pattern, or agent framework.
- Missing evidence: A concrete implementation showing the boundary in practice.
- How the claim could be experimentally validated: In the EKOS implementation repository, show source records, semantic model, evidence layer, context assembler, and AI answer generator as separable components.
- Risks if the claim is false: EKOS may be too broad or too vague to implement.
- Suggested benchmark or evaluation: Architecture review against the seven-layer reference architecture.
- Current confidence level: Medium.

### EKOS-CLAIM-024

- Claim ID: EKOS-CLAIM-024
- Exact claim: "EKOS does not replace ERP, retrieval, APIs, MCP, agents, or human judgment."
- Why this claim matters: It protects the repository and product boundary.
- Existing supporting reasoning: The white paper states that EKOS gives these systems a shared foundation rather than replacing them.
- Missing evidence: Integration patterns showing EKOS complementing rather than duplicating source systems and AI components.
- How the claim could be experimentally validated: Map each prototype component to one responsibility and verify no source-system or agent-framework replacement is being built inside North Star.
- Risks if the claim is false: EKOS may become an oversized platform that recreates existing systems.
- Suggested benchmark or evaluation: Responsibility-boundary checklist across source systems, EKOS, AI layer, and human review.
- Current confidence level: High as a design constraint.

### EKOS-CLAIM-025

- Claim ID: EKOS-CLAIM-025
- Exact claim: "Source systems remain systems of record."
- Why this claim matters: EKOS relies on preserving authoritative operational facts rather than becoming another operational system.
- Existing supporting reasoning: The architecture section states that EKOS reads from source systems, references them, and may expose controlled execution paths back into them.
- Missing evidence: Prototype source references that preserve authoritative IDs and provenance.
- How the claim could be experimentally validated: Every semantic object in the synthetic scenario must link back to a source record or source-like fixture.
- Risks if the claim is false: EKOS could drift away from operational truth and become a competing source of record.
- Suggested benchmark or evaluation: Provenance completeness: percentage of semantic objects and evidence records with source ID, source type, and timestamp.
- Current confidence level: High.

### EKOS-CLAIM-026

- Claim ID: EKOS-CLAIM-026
- Exact claim: "The enterprise semantic model converts source signals into business meaning."
- Why this claim matters: This is the core transformation EKOS claims to perform.
- Existing supporting reasoning: The white paper defines the semantic model as the layer that answers, "What is this thing, and how does it participate in the business?"
- Missing evidence: A working semantic model that maps raw or source-like records into typed business objects and relationships.
- How the claim could be experimentally validated: Transform synthetic source fixtures into a semantic graph or structured model, then inspect expected objects, lifecycle states, and links.
- Risks if the claim is false: EKOS may fail at the central conversion from data to meaning.
- Suggested benchmark or evaluation: Semantic mapping accuracy against expected object and relationship fixtures.
- Current confidence level: Medium.

### EKOS-CLAIM-027

- Claim ID: EKOS-CLAIM-027
- Exact claim: "The semantic model must resolve multiple references to the same object."
- Why this claim matters: Identity resolution is necessary when the same delivery appears in SAP data, spreadsheets, tracking updates, tickets, or emails.
- Existing supporting reasoning: The white paper says a delivery may be represented across structured data, email, tracking events, and escalation discussion, and references should resolve to the same object when appropriate.
- Missing evidence: Identity-resolution cases with aliases, partial IDs, and conflicting references.
- How the claim could be experimentally validated: Create reference variants and distractors for the same delivery, then test resolution precision and recall.
- Risks if the claim is false: EKOS may either merge unrelated entities or fragment one entity into many objects.
- Suggested benchmark or evaluation: Object identity resolution benchmark with true positives, false merges, and missed links.
- Current confidence level: Medium.

### EKOS-CLAIM-028

- Claim ID: EKOS-CLAIM-028
- Exact claim: "The evidence layer connects claims to support."
- Why this claim matters: It is the technical mechanism behind traceability and reviewability.
- Existing supporting reasoning: The architecture section defines evidence as source records, events, documents, approvals, change history, timestamps, confirmations, and external updates connected to claims.
- Missing evidence: Implemented claim-evidence data structure and reviewer scoring.
- How the claim could be experimentally validated: Require each answer claim to reference structured evidence and verify reviewer ability to trace the reasoning path.
- Risks if the claim is false: EKOS answers may remain fluent but hard to audit.
- Suggested benchmark or evaluation: Claim coverage ratio: percentage of root-cause, impact, and boundary claims with evidence links.
- Current confidence level: Medium-High.

### EKOS-CLAIM-029

- Claim ID: EKOS-CLAIM-029
- Exact claim: "Retrieval should not only return related text. It should return business context."
- Why this claim matters: It defines the expected improvement over generic RAG.
- Existing supporting reasoning: The white paper says retrieval should assemble the relevant object, related objects, process state, evidence, policies, and boundaries.
- Missing evidence: Side-by-side outputs showing business-context retrieval is more accurate or reviewable than text retrieval.
- How the claim could be experimentally validated: Compare chunk-only retrieval with context assembly for the same questions and score the seven metrics.
- Risks if the claim is false: Generic retrieval may be good enough for the prototype.
- Suggested benchmark or evaluation: Context assembly completeness score per answer.
- Current confidence level: Medium.

### EKOS-CLAIM-030

- Claim ID: EKOS-CLAIM-030
- Exact claim: "GraphRAG is not the whole architecture, but it can use the semantic model and evidence layer to retrieve connected business context."
- Why this claim matters: It positions GraphRAG as a possible consumer or technique rather than EKOS itself.
- Existing supporting reasoning: The white paper separates EKOS from GraphRAG and defines GraphRAG as a retrieval and reasoning pattern.
- Missing evidence: Prototype evidence that graph-backed retrieval improves connected-context assembly.
- How the claim could be experimentally validated: Implement a graph-backed retrieval path in the EKOS repo and compare it with vector-only retrieval.
- Risks if the claim is false: Graph retrieval may add complexity without measurable improvement.
- Suggested benchmark or evaluation: Vector-only vs graph-assisted context assembly on relationship traversal and evidence traceability.
- Current confidence level: Medium.

### EKOS-CLAIM-031

- Claim ID: EKOS-CLAIM-031
- Exact claim: "EKOS must connect capabilities to business authority."
- Why this claim matters: It makes execution boundary enforcement a first-class requirement.
- Existing supporting reasoning: The architecture states that MCP servers and APIs may expose capabilities, but EKOS must decide whether direct use, draft, recommendation, or review is allowed.
- Missing evidence: Formal boundary categories and policy-to-action mappings for the first scenario.
- How the claim could be experimentally validated: For each proposed action, require the system to output action category, required role, policy, evidence, and whether execution is allowed.
- Risks if the claim is false: EKOS could allow technically valid but unauthorized action.
- Suggested benchmark or evaluation: Execution boundary classification accuracy.
- Current confidence level: High.

### EKOS-CLAIM-032

- Claim ID: EKOS-CLAIM-032
- Exact claim: "Agents are consumers of EKOS. They are not the foundation."
- Why this claim matters: It prevents the architecture from becoming agent-first.
- Existing supporting reasoning: The white paper says agents should operate over business objects, relationships, evidence, policies, process states, and execution boundaries provided by EKOS.
- Missing evidence: Agent comparison showing improved behavior when the same agent consumes EKOS context.
- How the claim could be experimentally validated: Use identical agent logic with and without EKOS context and compare object grounding, action boundaries, and evidence use.
- Risks if the claim is false: Agent frameworks may absorb enough semantics internally, reducing EKOS's independent role.
- Suggested benchmark or evaluation: Agent context ablation: loose text vs EKOS structured context.
- Current confidence level: Medium.

### EKOS-CLAIM-033

- Claim ID: EKOS-CLAIM-033
- Exact claim: "Governance is not an optional compliance wrapper. It is part of the architecture."
- Why this claim matters: It treats semantic correctness and evolution as engineering concerns, not documentation afterthoughts.
- Existing supporting reasoning: The white paper says enterprise meaning, processes, policies, systems, and AI models change, so EKOS must preserve continuity while allowing controlled evolution.
- Missing evidence: A minimal governance workflow integrated into prototype iteration.
- How the claim could be experimentally validated: Require every semantic model change to have an ADR or review note, version, reviewer, and evaluation impact.
- Risks if the claim is false: Governance may slow the project without improving correctness.
- Suggested benchmark or evaluation: Governance traceability audit across semantic model versions.
- Current confidence level: Medium.

### EKOS-CLAIM-034

- Claim ID: EKOS-CLAIM-034
- Exact claim: "Architecture should remain stable even when implementation choices change."
- Why this claim matters: It keeps EKOS independent from any one model, database, graph engine, vector store, framework, or protocol.
- Existing supporting reasoning: The design principles explicitly reject vendor-shaped architecture and require implementation choices to map to architectural roles.
- Missing evidence: At least two possible technology mappings that preserve the same architecture.
- How the claim could be experimentally validated: Document alternative implementation ADRs and show that source systems, semantic model, evidence, retrieval, boundary, agent, and governance responsibilities remain intact.
- Risks if the claim is false: EKOS may become tightly coupled to whichever implementation is built first.
- Suggested benchmark or evaluation: Architecture portability review.
- Current confidence level: High as a design principle; Medium as a practical implementation claim.

### EKOS-CLAIM-035

- Claim ID: EKOS-CLAIM-035
- Exact claim: "Enterprise meaning should outlive any model."
- Why this claim matters: It is the continuity principle behind North Star and EKOS.
- Existing supporting reasoning: The white paper says models, prompts, agent frameworks, and retrieval systems will change, while enterprise meaning should remain durable.
- Missing evidence: Model-swap experiments showing stable object identity, evidence references, and execution boundaries.
- How the claim could be experimentally validated: Run the same EKOS context through multiple models or prompts and compare stable semantic fields.
- Risks if the claim is false: EKOS may not actually preserve continuity better than prompt or model-level memory.
- Suggested benchmark or evaluation: Model-change stability metric.
- Current confidence level: Medium-High.

### EKOS-CLAIM-036

- Claim ID: EKOS-CLAIM-036
- Exact claim: "The first EKOS prototype should prove read-only understanding before claiming automation."
- Why this claim matters: It defines the first research milestone and prevents premature automation.
- Existing supporting reasoning: The white paper repeatedly states that the first prototype should answer questions, trace evidence, and identify boundaries without live updates.
- Missing evidence: None for the project-management claim; missing evidence remains for whether read-only understanding can be proven.
- How the claim could be experimentally validated: Build only read-only answer generation and boundary classification in the EKOS repo, then evaluate without tool execution.
- Risks if the claim is false: A read-only prototype may fail to demonstrate enough value or may miss execution-specific issues.
- Suggested benchmark or evaluation: Read-only evaluation using Cases A-E and no live execution.
- Current confidence level: High as sequencing strategy.

### EKOS-CLAIM-037

- Claim ID: EKOS-CLAIM-037
- Exact claim: "SAP logistics delay analysis is a narrow but useful test."
- Why this claim matters: It justifies the first prototype domain.
- Existing supporting reasoning: The white paper says the scenario is compact but includes object identity, cross-system relationships, event interpretation, process state, evidence, policy, role ownership, and action boundaries.
- Missing evidence: Confirmation that the synthetic scenario is representative enough of real logistics reasoning.
- How the claim could be experimentally validated: Have logistics or SAP-domain reviewers assess whether Cases A-E capture meaningful operational reasoning.
- Risks if the claim is false: The prototype may overfit to an artificial slice and fail to validate EKOS broadly.
- Suggested benchmark or evaluation: Domain reviewer realism score for each synthetic case.
- Current confidence level: Medium.

### EKOS-CLAIM-038

- Claim ID: EKOS-CLAIM-038
- Exact claim: "Synthetic data can test architecture, but it cannot prove production readiness."
- Why this claim matters: It keeps claims honest while enabling public-safe validation.
- Existing supporting reasoning: The limitations section states that synthetic data can test objects, relationships, evidence, and boundaries, but not data quality, integrations, latency, access control, or organizational review at production scale.
- Missing evidence: None for the limitation itself; missing evidence remains for how realistic the synthetic data is.
- How the claim could be experimentally validated: Separate architecture metrics from production-readiness metrics and label results accordingly.
- Risks if the claim is false: If synthetic data is too weak, early validation may be misleading; if too strong a claim is made, public positioning may overstate results.
- Suggested benchmark or evaluation: Public-safety and realism review checklist for synthetic cases.
- Current confidence level: High.

### EKOS-CLAIM-039

- Claim ID: EKOS-CLAIM-039
- Exact claim: "The baseline should be a retrieval-only system using the same synthetic SAP logistics data."
- Why this claim matters: A fair baseline is required to make EKOS improvement claims credible.
- Existing supporting reasoning: The evaluation section says the baseline should be allowed to retrieve relevant information and should not be artificially weakened.
- Missing evidence: A defined baseline configuration and proof that it uses the same underlying facts.
- How the claim could be experimentally validated: Implement baseline and EKOS-backed systems over identical synthetic data and record all retrieval inputs.
- Risks if the claim is false: Any score improvement may be dismissed as unfair comparison.
- Suggested benchmark or evaluation: Baseline fairness audit: same facts, same questions, comparable model, documented retrieval context.
- Current confidence level: High as evaluation design.

### EKOS-CLAIM-040

- Claim ID: EKOS-CLAIM-040
- Exact claim: "Explicit enterprise meaning improves enterprise AI understanding over retrieval alone."
- Why this claim matters: This is the central testable EKOS hypothesis.
- Existing supporting reasoning: The white paper argues that EKOS adds object identity, relationship paths, evidence chains, policy constraints, process states, and execution boundaries that retrieval-only systems lack.
- Missing evidence: Actual measured score delta between EKOS-backed and retrieval-only systems.
- How the claim could be experimentally validated: Run both systems over Cases A-E and compare scores across all seven metrics.
- Risks if the claim is false: EKOS's core thesis weakens significantly; the project may need to narrow, redesign, or shift value proposition.
- Suggested benchmark or evaluation: Primary EKOS validation benchmark: average EKOS score minus retrieval-only baseline score, plus failure-mode analysis.
- Current confidence level: Medium.

### EKOS-CLAIM-041

- Claim ID: EKOS-CLAIM-041
- Exact claim: "The first metrics should focus on object identity, relationship traversal, evidence, process state, policy boundaries, and model-change stability."
- Why this claim matters: It defines what EKOS means by "understanding" in the first prototype.
- Existing supporting reasoning: Section 8 defines seven metrics: object identification, relationship traversal, evidence traceability, process state awareness, policy and boundary recognition, fact/hypothesis/recommendation separation, and model-change stability.
- Missing evidence: Reviewer validation that these metrics cover the important behaviors for the first scenario.
- How the claim could be experimentally validated: Have domain and AI reviewers score sample outputs and identify missing evaluation dimensions.
- Risks if the claim is false: The evaluation may reward the wrong behavior or miss important failures.
- Suggested benchmark or evaluation: Rubric validation session with inter-rater agreement.
- Current confidence level: Medium.

### EKOS-CLAIM-042

- Claim ID: EKOS-CLAIM-042
- Exact claim: "Each test answer can be scored from 0 to 2 on each metric."
- Why this claim matters: It provides a simple repeatable first scoring method.
- Existing supporting reasoning: Section 8 defines 0 as missing or incorrect, 1 as partially correct, and 2 as correct, explicit, and supported.
- Missing evidence: Whether different reviewers can apply the scale consistently.
- How the claim could be experimentally validated: Run two or more reviewers over the same outputs and calculate agreement.
- Risks if the claim is false: Scores may look precise while hiding subjective inconsistency.
- Suggested benchmark or evaluation: Inter-rater reliability on all seven metrics.
- Current confidence level: Medium.

### EKOS-CLAIM-043

- Claim ID: EKOS-CLAIM-043
- Exact claim: "Failure modes should become future test cases."
- Why this claim matters: It turns evaluation into an iterative research program rather than a one-time demo.
- Existing supporting reasoning: Section 8 lists failure modes such as wrong primary object, missing relationship path, unsupported root cause, incorrect policy application, timestamp confusion, and ignoring conflicting evidence.
- Missing evidence: A process for converting observed failures into dataset additions.
- How the claim could be experimentally validated: After each evaluation run, add at least one new fixture for every repeated failure mode and track whether the failure recurs.
- Risks if the claim is false: EKOS may stagnate after a small hand-picked benchmark.
- Suggested benchmark or evaluation: Failure-mode regression suite growth over time.
- Current confidence level: High as research methodology.

### EKOS-CLAIM-044

- Claim ID: EKOS-CLAIM-044
- Exact claim: "Model-change stability means wording may change, but object identity, evidence references, and execution boundaries should not change."
- Why this claim matters: It operationalizes the durability claim.
- Existing supporting reasoning: The evaluation section says the same business object graph, evidence records, policies, and boundaries should remain available regardless of which model generates the final answer.
- Missing evidence: Multi-model or multi-prompt runs over the same EKOS context.
- How the claim could be experimentally validated: Generate answers from multiple models or prompts using the same structured context and diff stable fields.
- Risks if the claim is false: EKOS may still be too dependent on final model behavior.
- Suggested benchmark or evaluation: Stability score: stable fields unchanged across model runs divided by expected stable fields.
- Current confidence level: Medium.

### EKOS-CLAIM-045

- Claim ID: EKOS-CLAIM-045
- Exact claim: "EKOS should not claim measured business impact, operational savings, or production readiness in the first evaluation."
- Why this claim matters: It protects credibility and prevents overclaiming.
- Existing supporting reasoning: The limitations and evaluation sections explicitly reject production-readiness and business-impact claims for the first synthetic prototype.
- Missing evidence: None for the communication boundary; business impact itself remains future work.
- How the claim could be experimentally validated: Review public outputs and case studies for prohibited claims.
- Risks if the claim is false: Public positioning may overstate the prototype and reduce trust.
- Suggested benchmark or evaluation: Claim hygiene checklist before publishing any case study or README update.
- Current confidence level: High.

### EKOS-CLAIM-046

- Claim ID: EKOS-CLAIM-046
- Exact claim: "EKOS does not eliminate human judgment."
- Why this claim matters: It defines semantic authority and high-impact action governance.
- Existing supporting reasoning: The white paper says enterprise meaning often requires interpretation, ownership, approval, and accountability, and that humans must retain authority over semantic changes and high-impact actions.
- Missing evidence: Concrete review workflow and role definitions in the first prototype.
- How the claim could be experimentally validated: Include expected human-review points in each case and score whether the system identifies them.
- Risks if the claim is false: EKOS could be misread as full automation or could produce uncontrolled semantic drift.
- Suggested benchmark or evaluation: Human-review requirement accuracy.
- Current confidence level: High as design principle.

### EKOS-CLAIM-047

- Claim ID: EKOS-CLAIM-047
- Exact claim: "The first scenario may not generalize."
- Why this claim matters: It limits interpretation of the first benchmark and defines future research requirements.
- Existing supporting reasoning: The limitations section says SAP logistics delay analysis does not prove generalization to procurement, order-to-cash, customer service, manufacturing quality, financial close, or HR workflows.
- Missing evidence: Cross-domain validation.
- How the claim could be experimentally validated: After logistics, test at least two additional workflows with different object, policy, and evidence patterns.
- Risks if the claim is false: Overfitting to logistics may produce an architecture that fails in other enterprise domains.
- Suggested benchmark or evaluation: Cross-domain transfer test using two additional enterprise workflows.
- Current confidence level: High for the limitation; Low for any broad generalization claim.

### EKOS-CLAIM-048

- Claim ID: EKOS-CLAIM-048
- Exact claim: "Production EKOS would require security and privacy design as first-class architecture layers."
- Why this claim matters: Enterprise deployment cannot be credible without access control, privacy, auditability, and secure integration.
- Existing supporting reasoning: The limitations section lists role-based access control, data minimization, sensitive data handling, audit logs, approval history, and secure tool execution as future work.
- Missing evidence: A security model, threat model, access-control design, and privacy constraints.
- How the claim could be experimentally validated: Future production-oriented research should create a threat model and evaluate access-control enforcement over semantic context and execution tools.
- Risks if the claim is false: EKOS may be architecturally incomplete for real enterprises.
- Suggested benchmark or evaluation: Security and privacy architecture review before any private-data or live-integration prototype.
- Current confidence level: High.

## Research Program Summary

The current white paper contains one central empirical hypothesis:

> Explicit enterprise meaning improves enterprise AI understanding over retrieval alone.

The most important validation path is:

1. Build synthetic SAP logistics cases in `2000silpeed/ekos-sap-knowledge-os`.
2. Build a fair retrieval-only baseline over the same facts.
3. Build EKOS-backed context assembly over explicit objects, relationships, evidence, policies, roles, and boundaries.
4. Score both systems with the seven-metric rubric.
5. Track qualitative reviewer notes and convert repeated failures into new test cases.
6. Keep claims limited to architecture validation until real workflow evidence exists.

## Immediate Research Questions

1. Which claims can be validated with the existing five-case synthetic logistics plan?
2. Which claims require additional cases or domains?
3. Which metrics need reviewer calibration before they can be treated as reliable?
4. Which semantic components are essential, and which can be deferred?
5. How large must the evaluation be before EKOS can make stronger public claims?

## Next Research Step

Create a benchmark specification that maps:

- each synthetic case,
- each expected object and relationship,
- each evidence record,
- each policy and boundary,
- each question,
- each scoring metric,
- and each claim in this matrix.

That benchmark specification should remain in North Star. The runnable benchmark should be implemented in `2000silpeed/ekos-sap-knowledge-os`.
