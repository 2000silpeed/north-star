# 8. Evaluation

Status: Draft v0.1

## Reader-Facing Draft
EKOS should be evaluated against a clear baseline.

The goal is not to show that EKOS produces more impressive language. The goal is to show that EKOS improves enterprise understanding.

In the first prototype, the baseline should be a retrieval-only system using the same synthetic SAP logistics data. The EKOS-backed system should use the same data, but with explicit business objects, relationships, evidence, policies, and execution boundaries.

The evaluation should answer one question:

> Does explicit enterprise meaning produce better enterprise AI behavior than retrieval alone?

## Evaluation Scope
The first evaluation should be narrow and honest.

It should evaluate read-only understanding, not full enterprise automation.

The system should be judged on whether it can:

- Identify the right business object
- Traverse related objects
- Explain the reasoning path
- Cite evidence
- Distinguish facts from hypotheses
- Recognize policy and execution boundaries
- Produce stable answers across model changes

It should not claim measured business impact, operational savings, or production readiness.

## Baseline System
The baseline system should represent a typical retrieval-first enterprise AI approach.

It may include:

- Document chunks
- SOP text
- Tracking notes
- Exception ticket text
- Delivery summaries
- Vector search
- Prompt-based answer generation

The baseline should be allowed to retrieve relevant information. It should not be artificially weakened.

The point is not to prove that retrieval is useless. The point is to test what retrieval misses when it does not have explicit enterprise meaning.

## EKOS-Backed System
The EKOS-backed system should use the same synthetic scenario, but with additional structure:

- Business object identity
- Object relationships
- Process states
- Event history
- Evidence records
- Policy constraints
- Role ownership
- Execution boundaries

The EKOS-backed system should answer with structured context, not only natural language.

It should make visible:

- What object the answer is about
- Which related objects were used
- Which evidence supports each claim
- Which policy or boundary applies
- Whether the output is an answer, hypothesis, recommendation, draft, or execution step

## Evaluation Dataset
The first dataset should be synthetic but operationally realistic.

It should include multiple logistics delay cases, not only one happy path.

Suggested case types:

### Case A: Carrier Pickup Delay
A carrier misses pickup because of capacity shortage.

Expected EKOS behavior:

- Identify shipment and freight order
- Connect carrier event to delay
- Cite planned versus actual pickup
- Identify affected customer commitment
- Recommend review before customer update

### Case B: Warehouse Processing Delay
Goods issue is delayed because warehouse processing completes late.

Expected EKOS behavior:

- Identify delivery as primary object
- Avoid blaming carrier event
- Connect warehouse event to process state
- Cite goods issue timestamps
- Identify responsible internal role

### Case C: Documentation Hold
Shipment is delayed because required export documentation is missing.

Expected EKOS behavior:

- Identify document-related policy
- Connect missing document to shipment hold
- Distinguish operational delay from carrier delay
- Identify approval or document owner

### Case D: False Delay Signal
A tracking event appears delayed, but customer commitment is not at risk.

Expected EKOS behavior:

- Identify related event
- Check customer commitment date
- Avoid over-escalation
- State that there is no current commitment breach

### Case E: Conflicting Evidence
Carrier event and internal ticket disagree.

Expected EKOS behavior:

- Surface conflicting evidence
- Avoid unsupported certainty
- Separate fact from hypothesis
- Recommend human review

## Evaluation Questions
The evaluation should include questions that test enterprise understanding.

Examples:

- Why was delivery D-10045 delayed?
- Which business object is the primary object of analysis?
- Which related objects are relevant?
- What evidence supports the delay reason?
- Is the customer commitment at risk?
- What policy applies to the next action?
- Who should review the recommendation?
- Is the system allowed to execute the action directly?
- What changed when the carrier event arrived?
- Which evidence is conflicting or missing?

These questions should be answered by both the baseline system and the EKOS-backed system.

## Metrics
Evaluation should combine structured scoring and qualitative review.

The initial metrics should be simple enough to apply manually, then later automated where possible.

### 1. Object Identification Accuracy
Measures whether the system identifies the correct primary business object.

Example:

- Correct: Delivery D-10045 is the primary object.
- Incorrect: Shipment S-7781 is treated as the primary object when the question asks about delivery impact.

Initial target:

- EKOS should identify the correct primary object in most test cases.
- Baseline should be expected to fail when text mentions several related objects without explicit identity resolution.

### 2. Relationship Traversal Accuracy
Measures whether the system follows the correct business relationship path.

Example:

```text
Delivery -> Shipment -> Freight Order -> Carrier Event
Delivery -> Customer Commitment
```

The answer should not only mention related objects. It should explain how they are connected.

Initial target:

- EKOS should produce an explicit relationship path.
- Baseline may mention objects without preserving the path.

### 3. Evidence Traceability
Measures whether claims are supported by source evidence.

The answer should show:

- Evidence ID
- Timestamp
- Source
- Related business object
- Claim supported by the evidence

Initial target:

- Every root cause claim should have at least one evidence reference.
- Unsupported claims should be labeled as hypotheses.

### 4. Process State Awareness
Measures whether the system understands where the object is in the process.

For logistics, this may include:

- Planned pickup
- Actual pickup
- Goods issue
- In transit
- Customs hold
- Delivered
- Exception opened

Initial target:

- EKOS should distinguish delays at different process stages.
- Baseline may treat all delay signals as equivalent.

### 5. Policy and Boundary Recognition
Measures whether the system identifies relevant policies and action limits.

The answer should state whether the AI system may:

- Answer
- Recommend
- Draft
- Execute
- Escalate

Initial target:

- EKOS should identify when human approval is required.
- Baseline may recommend action without authority context.

### 6. Fact, Hypothesis, and Recommendation Separation
Measures whether the system separates what is known, inferred, and proposed.

The answer should distinguish:

- Fact: Carrier event CE-3302 reported missed pickup.
- Hypothesis: Missed pickup likely caused the delivery delay.
- Recommendation: Logistics operations should review impact and prepare customer update.
- Boundary: Manager approval is required before commitment change.

Initial target:

- EKOS should make these categories explicit.
- Baseline may merge them into one fluent explanation.

### 7. Model-Change Stability
Measures whether the enterprise context remains stable when the AI model changes.

The same business object graph, evidence records, policies, and boundaries should remain available regardless of which model generates the final answer.

Initial target:

- Changing the model may change wording.
- It should not change object identity, evidence references, or execution boundaries.

## Scoring Rubric
Each test answer can be scored from 0 to 2 on each metric.

```text
0 = Missing or incorrect
1 = Partially correct but incomplete or unclear
2 = Correct, explicit, and supported
```

For the first prototype, recommended metrics:

1. Object identification
2. Relationship traversal
3. Evidence traceability
4. Process state awareness
5. Policy and boundary recognition
6. Fact/hypothesis/recommendation separation
7. Model-change stability

Maximum score per case:

```text
7 metrics x 2 points = 14 points
```

The evaluation should compare:

```text
Retrieval-only baseline score
EKOS-backed score
Qualitative reviewer notes
```

## Qualitative Review
Not every important behavior can be reduced to a number.

Human reviewers should also assess:

- Does the answer reflect how enterprise operations actually work?
- Does it avoid overclaiming?
- Does it explain uncertainty clearly?
- Does it preserve operational boundaries?
- Would a domain expert know what to review next?

This qualitative review is especially important in early prototypes because the synthetic dataset will be small.

## Failure Modes to Track
EKOS should explicitly track its own failure modes.

Important failure modes include:

- Wrong primary object
- Missing relationship path
- Unsupported root cause
- Incorrect policy application
- Confusing event timestamp order
- Treating recommendation as execution
- Ignoring conflicting evidence
- Over-escalating a non-risk situation
- Under-escalating a commitment breach
- Producing fluent explanation without traceability

These failures should become test cases over time.

## Evaluation Output Format
Each evaluation run should produce a structured report.

Example:

```text
Case ID: CASE-A-Carrier-Pickup-Delay

Question:
  Why was delivery D-10045 delayed?

Baseline score:
  7 / 14

EKOS score:
  12 / 14

Key EKOS strengths:
  - Correctly identified Delivery D-10045 as primary object.
  - Traversed Delivery -> Shipment -> Freight Order -> Carrier Event.
  - Connected root cause hypothesis to CE-3302 and pickup timestamps.
  - Identified manager approval boundary.

Remaining issues:
  - Recommendation text should separate customer notification draft from internal review action.
```

The purpose of the evaluation report is not only to show improvement. It should guide the next architecture and prototype changes.

## What Would Count as Success
The first evaluation succeeds if EKOS consistently improves traceability, object grounding, relationship traversal, and boundary recognition compared with the retrieval-only baseline.

It does not need to prove full automation.

It does need to show that explicit enterprise meaning changes the quality of the AI system's behavior.

## Key Claims
- EKOS should be evaluated against retrieval-only systems, not only judged by demo quality.
- Evaluation should test enterprise understanding, not language fluency.
- The first metrics should focus on object identity, relationship traversal, evidence, process state, policy boundaries, and model-change stability.
- Human qualitative review remains necessary because enterprise correctness is partly domain-specific.
- Failure modes should become future test cases.

## Reasoning Preserved for Future Drafts
This section intentionally avoids exaggerated benchmark claims.

Rejected framing:

- "EKOS improves enterprise AI by X percent."
- "EKOS solves enterprise AI evaluation."
- "A single benchmark can prove enterprise understanding."

Preferred framing:

- "EKOS should be evaluated through specific enterprise reasoning behaviors against a retrieval-only baseline."

The first evaluation should be practical, transparent, and repeatable. Stronger metrics can be added after the prototype exists.

## Open Questions
- Should the first evaluation be manual, automated, or hybrid?
- How many synthetic cases are enough for v0.1?
- Should model-change stability be tested with multiple LLM providers or multiple prompts over the same model?
- Should evidence traceability require exact source IDs in every answer?
- Who should act as the domain reviewer for the first evaluation?

## Next Section Bridge
Section 9 should define the implementation roadmap. It should convert the white paper into a staged build plan: glossary, synthetic data, semantic model, graph-backed retrieval, evaluation harness, execution boundary, and public demo.
