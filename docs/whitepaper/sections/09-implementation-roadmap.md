# 9. Implementation Roadmap

Status: Draft v0.1

## Reader-Facing Draft
The first EKOS implementation should be small, public-safe, and testable.

It should not start by connecting to a private enterprise system. It should not begin with full workflow automation. It should not try to model every SAP process.

The first implementation should prove one thing:

> Explicit enterprise meaning improves AI understanding over retrieval alone.

The roadmap below converts the EKOS white paper into a staged build plan.

## Roadmap Principles
The implementation roadmap follows the design principles from Section 6.

- Start with the enterprise problem.
- Use synthetic data.
- Model meaning before retrieval.
- Ground answers in evidence.
- Keep execution read-only at first.
- Evaluate against a retrieval-only baseline.
- Make every artifact public-safe and reviewable.

## Phase 0: Repository Foundation
Goal:

Create the project structure needed to build EKOS in public.

Core tasks:

1. Define repository layout.
2. Keep white paper, ADRs, glossary, prototype, data, and evaluation separate.
3. Add documentation rules for synthetic data and public-safe examples.
4. Define how future decisions are recorded.

Suggested artifacts:

```text
docs/
  adr/
  glossary/
  whitepaper/
  methodology/
  research/

prototype/
  data/
  semantic_model/
  retrieval/
  evaluation/
  app/

roadmap/
reviews/
```

Exit criteria:

- Repository has clear structure.
- ADR-0001 exists.
- White paper outline exists.
- First sections of the white paper are drafted.
- Future AI sessions can continue from documented state.

Current status:

- Mostly complete.

## Phase 1: Enterprise Concept Glossary
Goal:

Define the core concepts EKOS uses before building the prototype.

Core tasks:

1. Define business object.
2. Define business process.
3. Define event.
4. Define evidence.
5. Define policy.
6. Define role.
7. Define relationship.
8. Define execution boundary.
9. Define enterprise semantic model.
10. Define enterprise intelligence infrastructure.

Suggested artifacts:

```text
docs/glossary/business-object.md
docs/glossary/business-process.md
docs/glossary/event.md
docs/glossary/evidence.md
docs/glossary/policy.md
docs/glossary/role.md
docs/glossary/relationship.md
docs/glossary/execution-boundary.md
docs/glossary/enterprise-semantic-model.md
docs/glossary/enterprise-intelligence-infrastructure.md
```

Exit criteria:

- Every glossary term has a short definition, purpose, example, and non-example.
- The definitions match the white paper.
- No term depends on private company data.
- The glossary can be used as the vocabulary for the prototype.

## Phase 2: Synthetic SAP Logistics Dataset
Goal:

Create a public-safe dataset for SAP logistics delay analysis.

Core tasks:

1. Create synthetic deliveries.
2. Create synthetic shipments.
3. Create synthetic freight orders.
4. Create synthetic carrier events.
5. Create synthetic customer commitments.
6. Create synthetic exception tickets.
7. Create synthetic policies.
8. Create evidence records with timestamps and source references.
9. Create multiple delay cases for evaluation.

Minimum case set:

- Case A: Carrier pickup delay
- Case B: Warehouse processing delay
- Case C: Documentation hold
- Case D: False delay signal
- Case E: Conflicting evidence

Suggested artifacts:

```text
prototype/data/deliveries.json
prototype/data/shipments.json
prototype/data/freight_orders.json
prototype/data/carrier_events.json
prototype/data/customer_commitments.json
prototype/data/exceptions.json
prototype/data/policies.json
prototype/data/evidence.json
prototype/data/cases.json
```

Exit criteria:

- Dataset includes at least five cases.
- Every case has expected primary object, related objects, evidence path, and expected boundary.
- Dataset is synthetic and explicitly marked as synthetic.
- Data is small enough to review manually.

## Phase 3: Enterprise Semantic Model
Goal:

Turn synthetic records into connected enterprise meaning.

Core tasks:

1. Define object schemas.
2. Define relationship types.
3. Define lifecycle states.
4. Define evidence links.
5. Define policy applicability.
6. Define execution boundary categories.
7. Build a simple semantic graph or structured model.

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

Suggested artifacts:

```text
prototype/semantic_model/schema.md
prototype/semantic_model/relationships.md
prototype/semantic_model/model.json
prototype/semantic_model/examples.md
```

Exit criteria:

- Each synthetic case can be represented as business objects and relationships.
- The model can answer "what is connected to this delivery?"
- Evidence can be linked to claims.
- Policies can be linked to action boundaries.

## Phase 4: EKOS Context Assembler
Goal:

Build the first EKOS component that assembles business context for a question.

Core tasks:

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

Suggested artifacts:

```text
prototype/retrieval/context_assembler.py
prototype/retrieval/examples.md
prototype/retrieval/output_schema.json
```

Exit criteria:

- Context assembler returns structured context.
- Output separates object, relationship, evidence, policy, and boundary.
- Context can be reviewed without an LLM.
- The component works on all evaluation cases.

## Phase 5: Retrieval-Only Baseline
Goal:

Build a fair baseline for comparison.

Core tasks:

1. Create document-style text from the same synthetic data.
2. Index documents or chunks.
3. Retrieve relevant chunks for each evaluation question.
4. Generate answers using retrieval-only context.
5. Record baseline outputs.

Suggested artifacts:

```text
prototype/baseline/documents/
prototype/baseline/retrieval_baseline.py
prototype/baseline/outputs/
```

Exit criteria:

- Baseline uses the same underlying synthetic scenario.
- Baseline has access to relevant text.
- Baseline outputs can be scored using the Section 8 rubric.
- Baseline is not artificially weakened.

## Phase 6: EKOS-Backed Answering
Goal:

Generate answers using EKOS-assembled context.

Core tasks:

1. Feed structured EKOS context into an answer generator.
2. Produce answers that separate fact, hypothesis, recommendation, and boundary.
3. Include evidence references.
4. Include relationship paths.
5. Include action boundary statements.

Suggested artifacts:

```text
prototype/app/answer_generator.py
prototype/app/prompts/
prototype/app/sample_outputs/
```

Exit criteria:

- EKOS-backed answers include evidence paths.
- Answers identify primary and related objects.
- Answers distinguish recommendation from execution.
- Answers can be scored against baseline outputs.

## Phase 7: Evaluation Harness
Goal:

Evaluate baseline and EKOS-backed outputs consistently.

Core tasks:

1. Implement the scoring rubric from Section 8.
2. Store expected outputs for each case.
3. Compare baseline and EKOS-backed answers.
4. Produce a structured evaluation report.
5. Track failure modes.

Suggested artifacts:

```text
prototype/evaluation/rubric.md
prototype/evaluation/evaluate.py
prototype/evaluation/cases.json
prototype/evaluation/reports/
```

Exit criteria:

- Evaluation can score all cases.
- Report includes baseline score, EKOS score, and reviewer notes.
- Failure modes are recorded.
- Evaluation results guide next prototype changes.

## Phase 8: Execution Boundary Prototype
Goal:

Add read-only execution boundary reasoning before live execution.

Core tasks:

1. Define action categories.
2. Define boundaries: answer, recommend, draft, execute, escalate.
3. Link policies to allowed actions.
4. Add human approval requirements.
5. Include boundary in answer output.

Suggested artifacts:

```text
prototype/execution/boundaries.md
prototype/execution/policy_rules.json
prototype/execution/boundary_checker.py
```

Exit criteria:

- System can say what it may and may not do.
- System can identify human approval requirements.
- No live updates are executed.
- Boundaries are evaluated in the rubric.

## Phase 9: Public Demo
Goal:

Create a simple public demonstration of EKOS.

Core tasks:

1. Provide a CLI, notebook, or simple web app.
2. Show one baseline answer.
3. Show one EKOS-backed answer.
4. Show relationship path.
5. Show evidence chain.
6. Show execution boundary.
7. Include evaluation output.

Recommended first interface:

- CLI or notebook first
- Web UI later

Reason:

- The first demo should prove reasoning and evidence, not frontend polish.

Suggested artifacts:

```text
prototype/app/demo_cli.py
prototype/app/demo_notebook.ipynb
docs/demo/walkthrough.md
docs/demo/sample-output.md
```

Exit criteria:

- A reviewer can run or read the demo.
- The demo explains why EKOS answer is better than baseline.
- The demo uses only synthetic data.
- The demo can be linked from README.

## Phase 10: Public Case Study
Goal:

Turn the prototype into a public engineering artifact.

Core tasks:

1. Write a case study.
2. Explain the problem.
3. Explain the architecture.
4. Show synthetic data.
5. Compare baseline and EKOS.
6. Discuss limitations.
7. Explain next steps.

Suggested artifacts:

```text
docs/case-study/sap-logistics-delay-analysis.md
docs/case-study/evaluation-results.md
docs/case-study/architecture-diagram.md
```

Exit criteria:

- Case study is readable by enterprise AI engineers.
- Claims are evidence-backed.
- Limitations are explicit.
- The case study can serve as portfolio material.

## Suggested Build Order
The implementation should follow this order:

```text
1. Glossary
2. Synthetic data
3. Semantic model
4. Context assembler
5. Retrieval-only baseline
6. EKOS-backed answering
7. Evaluation harness
8. Execution boundary prototype
9. Public demo
10. Public case study
```

Do not start with agents.

Do not start with live SAP integration.

Do not start with full automation.

Start with durable enterprise meaning and evidence.

## Version Targets
### v0.1: Paper and Vocabulary
Goal:

Complete the white paper draft and glossary.

Exit criteria:

- White paper sections drafted
- Glossary v1 created
- ADRs updated

### v0.2: Synthetic Data and Semantic Model
Goal:

Represent SAP logistics delay scenarios as business objects and relationships.

Exit criteria:

- Synthetic dataset complete
- Semantic model complete
- Relationship paths reviewable

### v0.3: Context Assembler
Goal:

Assemble EKOS context from object ID and question.

Exit criteria:

- Context assembler works across all cases
- Evidence and policies included
- No LLM required to inspect context

### v0.4: Baseline and EKOS Answers
Goal:

Compare retrieval-only and EKOS-backed answers.

Exit criteria:

- Baseline outputs generated
- EKOS outputs generated
- Initial scoring completed

### v0.5: Evaluation Report
Goal:

Produce repeatable evaluation.

Exit criteria:

- Rubric applied
- Failure modes recorded
- Evaluation report generated

### v1.0: Public Demo and Case Study
Goal:

Publish the first coherent EKOS artifact.

Exit criteria:

- Demo available
- Case study written
- README updated
- White paper linked

## Key Claims
- The first EKOS implementation should prove understanding before automation.
- Synthetic data is enough for v0.1 because the goal is architecture validation, not production integration.
- The semantic model, context assembler, and evaluation harness are more important than agents in the first build.
- A retrieval-only baseline is necessary to make the prototype credible.
- The first public artifact should be a demo plus case study, not only a README.

## Reasoning Preserved for Future Drafts
This roadmap intentionally delays agents, MCP execution, and live SAP integration.

Rejected framing:

- "Start by connecting to SAP."
- "Start by building an agent."
- "Start by building a polished web UI."
- "Start by automating workflow execution."

Preferred framing:

- "Start by proving enterprise understanding with synthetic data, explicit semantics, and evaluation."

This keeps the project public-safe, testable, and aligned with the white paper.

## Open Questions
- Should the prototype be implemented in Python first, or TypeScript?
- Should the semantic model begin as JSON, SQLite, or a graph database?
- Should the evaluation harness use LLM-as-judge later, or remain human-reviewed for v0.1?
- Should the first public demo be a CLI, notebook, or simple web UI?
- Should the roadmap become GitHub issues after the white paper draft is complete?

## Next Section Bridge
Section 10 should define limitations. It should state what EKOS does not yet prove, what risks remain, and why the first prototype should avoid private data, live execution, unsupported business-impact claims, and overgeneralized architecture claims.
