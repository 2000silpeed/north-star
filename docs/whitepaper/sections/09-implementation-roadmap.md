# 9. Implementation Roadmap

Status: Draft v0.2

## Reader-Facing Draft
The EKOS implementation roadmap is not a plan to recreate EKOS inside North Star.

North Star is the research memory, strategy, white paper, and portfolio headquarters for EKOS. The executable EKOS implementation remains in `2000silpeed/ekos-sap-knowledge-os`.

This roadmap should therefore be read as a cross-repository coordination plan. North Star defines the thesis, vocabulary, evaluation criteria, and public narrative. The EKOS implementation repository turns those specifications into code, data, tests, demos, and releases.

The first EKOS implementation should still be small, public-safe, and testable. It should not start by connecting to private enterprise systems, full workflow automation, or every SAP process. It should prove one thing:

> Explicit enterprise meaning improves AI understanding over retrieval alone.

That proof belongs in the existing EKOS repository.

## Repository Boundary
North Star owns the durable reasoning around EKOS:

- White paper drafts
- ADRs
- Glossary and concept definitions
- Evaluation criteria
- Public narrative and portfolio positioning
- Cross-repository coordination notes

`2000silpeed/ekos-sap-knowledge-os` owns the implementation:

- Executable EKOS code
- Synthetic or public-safe data
- Semantic model implementation
- Context assembly logic
- Retrieval-only baseline
- EKOS-backed answer generation
- Evaluation harness
- Demo surfaces and runnable walkthroughs

North Star documents may propose what the implementation should prove. They should not prescribe a new local `prototype/` tree or duplicate the EKOS repository. Before proposing concrete implementation file paths, the existing EKOS repository structure should be inspected and followed.

## Roadmap Principles
The implementation roadmap follows the design principles from Section 6 and adds one repository rule:

- Do not recreate EKOS inside North Star.
- Use the existing EKOS repository as the implementation home.
- Treat North Star artifacts as specifications, not as a substitute codebase.
- Link implementation issues and pull requests back to the relevant North Star white paper, ADR, glossary, or evaluation section.
- Follow the current structure of `2000silpeed/ekos-sap-knowledge-os` before proposing new paths.
- Start with synthetic data and read-only understanding.
- Evaluate against a retrieval-only baseline.
- Make every public artifact reviewable and clear about its limits.

## Phase 0: Cross-Repository Alignment
Goal:

Align North Star and the EKOS implementation repository before adding implementation work.

North Star tasks:

1. Record the repository boundary in an ADR.
2. Keep the white paper, glossary, and evaluation rubric in North Star.
3. Link implementation-facing sections to `2000silpeed/ekos-sap-knowledge-os`.
4. Avoid local prototype directories in North Star.

EKOS repository tasks:

1. Inspect the existing repository structure.
2. Map current implementation artifacts to the white paper concepts.
3. Create implementation issues or milestones in the EKOS repository.
4. Link those issues back to the relevant North Star documents.

Exit criteria:

- North Star clearly states that it is not the implementation repository.
- The EKOS implementation repository is named wherever implementation work is discussed.
- The next implementation tasks can be tracked in the EKOS repository without duplicating files in North Star.

## Phase 1: Enterprise Concept Glossary
Goal:

Define the core concepts EKOS uses before implementation work expands.

North Star owns this phase as a specification artifact.

Core terms:

1. Business object
2. Business process
3. Event
4. Evidence
5. Policy
6. Role
7. Relationship
8. Execution boundary
9. Enterprise semantic model
10. Enterprise intelligence infrastructure

North Star artifacts:

```text
docs/glossary/
```

EKOS repository use:

- Use the glossary as implementation vocabulary.
- Align schema names, output fields, examples, and test cases with the glossary.
- Do not maintain a competing concept vocabulary in the implementation repository without linking back to North Star.

Exit criteria:

- Every term has a short definition, purpose, example, and non-example.
- The definitions match the white paper.
- No term depends on private company data.
- The EKOS implementation repository can use the glossary as a shared vocabulary.

## Phase 2: Synthetic SAP Logistics Dataset
Goal:

Create a public-safe dataset for SAP logistics delay analysis in the EKOS implementation repository.

North Star provides:

- Scenario requirements from Section 7
- Evaluation expectations from Section 8
- Public-safety constraints from Section 10

The EKOS implementation repository owns:

- Dataset files
- Data generation or validation scripts
- Case fixtures
- Any runnable examples that consume the data

Minimum case set:

- Case A: Carrier pickup delay
- Case B: Warehouse processing delay
- Case C: Documentation hold
- Case D: False delay signal
- Case E: Conflicting evidence

Exit criteria:

- Dataset includes at least five cases.
- Every case has expected primary object, related objects, evidence path, and expected boundary.
- Dataset is synthetic and explicitly marked as synthetic.
- Data is small enough to review manually.
- Implementation artifacts live in `2000silpeed/ekos-sap-knowledge-os`.

## Phase 3: Enterprise Semantic Model
Goal:

Turn synthetic records into connected enterprise meaning in the EKOS implementation repository.

North Star provides the conceptual model:

- Business objects
- Relationship types
- Lifecycle states
- Evidence links
- Policy applicability
- Execution boundary categories

The EKOS implementation repository owns the concrete representation. It may use JSON, SQLite, graph storage, typed code, or another structure, but the choice should follow the existing repository design rather than a new North Star layout.

Initial relationship types:

- `DELIVERY_BELONGS_TO_SALES_ORDER`
- `DELIVERY_INCLUDED_IN_SHIPMENT`
- `SHIPMENT_PLANNED_BY_FREIGHT_ORDER`
- `FREIGHT_ORDER_ASSIGNED_TO_CARRIER`
- `CARRIER_EVENT_AFFECTS_SHIPMENT`
- `SHIPMENT_DELAY_AFFECTS_CUSTOMER_COMMITMENT`
- `EXCEPTION_OPENED_FOR_DELIVERY`
- `POLICY_CONSTRAINS_ACTION`
- `EVIDENCE_SUPPORTS_CLAIM`

Exit criteria:

- Each synthetic case can be represented as business objects and relationships.
- The model can answer "what is connected to this delivery?"
- Evidence can be linked to claims.
- Policies can be linked to action boundaries.
- The implementation is committed in the EKOS repository.

## Phase 4: EKOS Context Assembler
Goal:

Build the first EKOS component that assembles business context for a question.

Implementation location:

- `2000silpeed/ekos-sap-knowledge-os`

Core behavior:

1. Accept a user question and object ID.
2. Identify the primary business object.
3. Retrieve related objects.
4. Traverse relationship paths.
5. Attach evidence.
6. Attach relevant policy constraints.
7. Return structured context for answer generation.

Example input:

```text
Why was delivery D-10045 delayed?
```

Example structured context:

```text
Primary object: Delivery D-10045
Related objects: Shipment S-7781, Freight Order FO-5510, Carrier Event CE-3302
Evidence: CE-3302, pickup timestamps, Exception EX-9007
Policy: POL-Delay-Approval
Boundary: AI may recommend and draft; manager approval required for commitment change
```

Exit criteria:

- Context assembler returns structured context.
- Output separates object, relationship, evidence, policy, and boundary.
- Context can be reviewed without an LLM.
- The component works on all evaluation cases.

## Phase 5: Retrieval-Only Baseline
Goal:

Build a fair baseline for comparison in the EKOS implementation repository.

North Star defines why the baseline matters. The EKOS repository implements it.

Core tasks:

1. Create document-style text from the same synthetic data.
2. Index documents or chunks.
3. Retrieve relevant chunks for each evaluation question.
4. Generate answers using retrieval-only context.
5. Record baseline outputs.

Exit criteria:

- Baseline uses the same underlying synthetic scenario.
- Baseline has access to relevant text.
- Baseline outputs can be scored using the Section 8 rubric.
- Baseline is not artificially weakened.

## Phase 6: EKOS-Backed Answering
Goal:

Generate answers using EKOS-assembled context in the implementation repository.

Core tasks:

1. Feed structured EKOS context into an answer generator.
2. Produce answers that separate fact, hypothesis, recommendation, and boundary.
3. Include evidence references.
4. Include relationship paths.
5. Include action boundary statements.

Exit criteria:

- EKOS-backed answers include evidence paths.
- Answers identify primary and related objects.
- Answers distinguish recommendation from execution.
- Answers can be scored against baseline outputs.

## Phase 7: Evaluation Harness
Goal:

Evaluate baseline and EKOS-backed outputs consistently.

North Star owns the evaluation rubric as a specification. The EKOS implementation repository owns the runnable harness and reports.

Core tasks:

1. Implement the scoring rubric from Section 8.
2. Store expected outputs for each case.
3. Compare baseline and EKOS-backed answers.
4. Produce a structured evaluation report.
5. Track failure modes.

Exit criteria:

- Evaluation can score all cases.
- Report includes baseline score, EKOS score, and reviewer notes.
- Failure modes are recorded.
- Evaluation results guide implementation changes in the EKOS repository.

## Phase 8: Execution Boundary Prototype
Goal:

Add read-only execution boundary reasoning before live execution.

Implementation location:

- `2000silpeed/ekos-sap-knowledge-os`

Core tasks:

1. Define action categories.
2. Define boundaries: answer, recommend, draft, execute, escalate.
3. Link policies to allowed actions.
4. Add human approval requirements.
5. Include boundary in answer output.

Exit criteria:

- System can say what it may and may not do.
- System can identify human approval requirements.
- No live updates are executed.
- Boundaries are evaluated in the rubric.

## Phase 9: Public Demo
Goal:

Create a simple public demonstration of EKOS from the implementation repository.

Recommended first interface:

- CLI or notebook first
- Web UI later

Reason:

- The first demo should prove reasoning and evidence, not frontend polish.

North Star may host the public narrative and case-study text. The runnable demo belongs in `2000silpeed/ekos-sap-knowledge-os`.

Exit criteria:

- A reviewer can run or read the demo from the EKOS repository.
- The demo explains why the EKOS-backed answer is better than the baseline.
- The demo uses only synthetic data.
- North Star links to the demo rather than duplicating it.

## Phase 10: Public Case Study
Goal:

Turn the prototype into a public engineering artifact.

North Star owns the narrative case study. The EKOS implementation repository owns the runnable evidence behind it.

Core tasks:

1. Explain the problem.
2. Explain the architecture.
3. Link to the EKOS implementation repository.
4. Show synthetic data assumptions.
5. Compare baseline and EKOS-backed outputs.
6. Discuss limitations.
7. Explain next steps.

Exit criteria:

- Case study is readable by enterprise AI engineers.
- Claims are evidence-backed.
- Limitations are explicit.
- Implementation claims link to runnable artifacts in `2000silpeed/ekos-sap-knowledge-os`.

## Suggested Build Order
The coordinated build should follow this order:

```text
1. North Star: glossary and evaluation specification
2. EKOS repository: synthetic data
3. EKOS repository: semantic model
4. EKOS repository: context assembler
5. EKOS repository: retrieval-only baseline
6. EKOS repository: EKOS-backed answering
7. EKOS repository: evaluation harness
8. EKOS repository: execution boundary prototype
9. EKOS repository: public demo
10. North Star + EKOS repository: public case study
```

Do not start by creating a local North Star prototype.

Do not start with agents.

Do not start with live SAP integration.

Do not start with full automation.

Start with durable enterprise meaning and evidence, then implement it in the existing EKOS repository.

## Coordination Milestones
### v0.1: Paper, Vocabulary, and Repository Boundary
Goal:

Complete the white paper draft, glossary, and repository-boundary ADR.

Exit criteria:

- White paper sections drafted
- Glossary v1 created in North Star
- ADRs updated
- Implementation work points to `2000silpeed/ekos-sap-knowledge-os`

### v0.2: Synthetic Data and Semantic Model
Goal:

Represent SAP logistics delay scenarios as business objects and relationships in the EKOS implementation repository.

Exit criteria:

- Synthetic dataset complete in the EKOS repository
- Semantic model complete in the EKOS repository
- Relationship paths reviewable
- North Star glossary referenced

### v0.3: Context Assembler
Goal:

Assemble EKOS context from object ID and question in the EKOS implementation repository.

Exit criteria:

- Context assembler works across all cases
- Evidence and policies included
- No LLM required to inspect context

### v0.4: Baseline and EKOS Answers
Goal:

Compare retrieval-only and EKOS-backed answers.

Exit criteria:

- Baseline outputs generated in the EKOS repository
- EKOS-backed outputs generated in the EKOS repository
- Initial scoring completed

### v0.5: Evaluation Report
Goal:

Produce repeatable evaluation.

Exit criteria:

- Rubric applied
- Failure modes recorded
- Evaluation report generated
- North Star links to the report instead of duplicating runnable assets

### v1.0: Public Demo and Case Study
Goal:

Publish the first coherent EKOS artifact.

Exit criteria:

- Demo available from the EKOS implementation repository
- Case study written in or referenced from North Star
- README links updated
- White paper linked

## Key Claims
- North Star is the EKOS research and strategy headquarters, not the implementation repository.
- The actual EKOS implementation belongs in `2000silpeed/ekos-sap-knowledge-os`.
- Implementation roadmaps in North Star should coordinate with the existing EKOS repository.
- The first public EKOS implementation should still prove understanding before automation.
- A retrieval-only baseline is necessary to make the prototype credible.

## Reasoning Preserved for Future Drafts
This roadmap intentionally separates strategy memory from implementation work.

Rejected framing:

- "Create a new EKOS prototype tree inside North Star."
- "Treat the white paper repository as the runnable implementation."
- "Design file paths before inspecting the existing EKOS repository."
- "Start by connecting to SAP."
- "Start by building an agent."
- "Start by building a polished web UI."
- "Start by automating workflow execution."

Preferred framing:

- "Use North Star to define the thesis, vocabulary, evaluation, and public narrative."
- "Use the existing EKOS repository to implement, test, and demonstrate the system."

This keeps the project public-safe, testable, and aligned with the actual repository architecture.

## Open Questions
- Which existing files in `2000silpeed/ekos-sap-knowledge-os` map to the white paper phases?
- Which North Star glossary terms should become implementation schema names?
- Should EKOS repository issues be created directly from the Section 9 phases?
- How should North Star link to implementation PRs and evaluation reports?
- Should the first public demo be a CLI, notebook, or simple web UI in the EKOS repository?

## Next Section Bridge
Section 10 should define limitations. It should state what EKOS does not yet prove, what risks remain, and why the first prototype should avoid private data, live execution, unsupported business-impact claims, overgeneralized architecture claims, and repository-boundary confusion.
