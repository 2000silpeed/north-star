# EKOS Benchmark Specification

Date: 2026-06-27

Status: Research contract draft

Owner: Project North Star

Canonical implementation repository: `2000silpeed/ekos-sap-knowledge-os`

Observed local implementation checkout: `/Users/sungwoon/ai-projects/Enterprise-knowledge-Operating-System`

Implementation boundary: North Star defines what must be tested, how results should be interpreted, and what would force claim revision. Runnable code, fixtures, test harnesses, reports, demos, and generated outputs belong in the EKOS implementation repository.

## Purpose

This specification turns the EKOS validation matrix and hostile reviews into an executable research contract.

It exists to answer one question:

> Does explicit enterprise meaning make EKOS stronger than strong non-EKOS baselines on enterprise reasoning tasks?

This document should not make EKOS sound better. It should make EKOS easier to test, reject, narrow, or strengthen with evidence.

## Source Artifacts

This benchmark specification is derived from:

- `docs/research/ekos-validation-matrix.md`
- `docs/reviews/review-001-hostile-review.md`
- `docs/reviews/falsification-strategy.md`
- `docs/research/2026-06-27-ekos-falsification-criteria.md`
- `docs/glossary/glossary-roadmap.md`
- `roadmap/ekos-strengthening-plan.md`

## Existing Implementation Signals

The local EKOS implementation checkout already contains executable assets that overlap with this benchmark direction:

- `tests/test_cross_pack_delivery_delay.py`
- `tests/test_messy_data_merge.py`
- `tests/test_reuse.py`
- `tests/test_governance_review.py`
- `tests/test_lineage_multihop.py`
- `tests/test_pack_le_tracking.py`
- `tests/test_pack_sap_business.py`
- `tests/test_pack_sap_execution.py`
- `tests/test_pack_sap_integration.py`
- `tests/test_pack_sap_policy.py`
- `fixtures/`
- `packs/`
- `ekos/`

These assets are not copied into North Star. They are evidence that the benchmark can be connected to real execution in the implementation repository.

## Core Claims in Scope

The first benchmark must cover only the claims needed to test the EKOS thesis.

| Claim ID | Benchmark role |
| --- | --- |
| EKOS-CLAIM-003 | Test whether the problem remains after access to relevant facts is controlled. |
| EKOS-CLAIM-004 | Test whether strong retrieval and full-context baselines are insufficient. |
| EKOS-CLAIM-008 | Test whether typed tools or APIs already provide enough enterprise semantics. |
| EKOS-CLAIM-010 | Test whether agents without EKOS can infer enough context dynamically. |
| EKOS-CLAIM-012 | Test whether EKOS has infrastructure value beyond one workflow. |
| EKOS-CLAIM-013 | Test whether the proposed semantic primitives are necessary. |
| EKOS-CLAIM-014 | Test object identity and false merge / false split behavior. |
| EKOS-CLAIM-015 | Test whether process state changes interpretation. |
| EKOS-CLAIM-016 | Test whether events improve causal explanation. |
| EKOS-CLAIM-019 | Test whether claim-linked evidence improves review quality. |
| EKOS-CLAIM-020 | Test relationship traversal across business chains. |
| EKOS-CLAIM-021 | Test execution-boundary recognition. |
| EKOS-CLAIM-023 | Test whether EKOS has a distinct infrastructure role. |
| EKOS-CLAIM-025 | Test whether source-system authority and provenance remain intact. |
| EKOS-CLAIM-026 | Test source-signal to business-meaning conversion. |
| EKOS-CLAIM-029 | Test whether business-context retrieval improves over text retrieval. |
| EKOS-CLAIM-031 | Test whether EKOS connects capability to business authority. |
| EKOS-CLAIM-035 | Test model-change or prompt-change stability. |
| EKOS-CLAIM-040 | Test the central hypothesis: explicit enterprise meaning improves enterprise AI understanding over retrieval alone. |
| EKOS-CLAIM-041 | Test whether the rubric measures enterprise utility, not only EKOS-shaped output. |

## Claims Explicitly Out of Scope for the First Benchmark

The first benchmark must not claim to prove:

- production readiness,
- measured business savings,
- operational ROI,
- security completeness,
- privacy completeness,
- live SAP correctness,
- cross-enterprise generality,
- statistical superiority across industries,
- human-organization adoption,
- or fully automated execution safety.

These may become later benchmarks. They are not valid conclusions from the first synthetic logistics benchmark.

## Benchmark Systems

Every case must be answerable by the same set of systems where practical. If a system cannot answer a case, the result must record why.

### SYS-001: Retrieval-Only Baseline

Purpose: Test whether normal retrieval over the same facts is enough.

Allowed inputs:

- Text chunks derived from the same synthetic facts.
- SOP text.
- Event notes.
- Ticket notes.
- Policy text.
- Source-like record summaries.

Not allowed:

- Pre-built EKOS graph.
- Explicit relationship path supplied as answer labels.
- Claim-evidence table supplied in EKOS format.
- Hidden metadata unavailable to EKOS.

### SYS-002: Full-Context Prompt Baseline

Purpose: Test whether careful context assembly in plain text is enough.

Allowed inputs:

- All relevant facts for the case in plain text.
- Object IDs.
- Event timelines.
- Policy excerpts.
- Source references.

Not allowed:

- EKOS semantic graph.
- Structured EKOS object/evidence/relationship schema.

### SYS-003: Typed-Tool Baseline

Purpose: Test whether typed APIs and source-system-style tools already expose enough meaning.

Allowed inputs:

- Deterministic lookup functions over the same synthetic records.
- Tool schemas that return object IDs, statuses, events, policies, and source references.
- No EKOS semantic model.

Required test:

- Include at least one case where tool outputs are correct but business interpretation is ambiguous.

### SYS-004: Agentic Retrieval-and-Tool Baseline

Purpose: Test whether an agent can dynamically infer context without EKOS.

Allowed inputs:

- Retrieval access.
- Typed-tool access.
- Multi-step planning.
- Re-querying and self-checking.

Not allowed:

- EKOS context assembler.
- EKOS graph traversal.
- EKOS claim-evidence layer.

### SYS-005: Workflow-Specific Context Assembler Baseline

Purpose: Test whether a simple workflow-specific system beats or ties EKOS without becoming general infrastructure.

Allowed inputs:

- Hand-authored SQL-like views, structured packets, or deterministic context assembly for the logistics workflow.
- No reusable EKOS semantic layer.

Interpretation:

- If this baseline ties EKOS with lower complexity, EKOS loses part of its infrastructure claim.

### SYS-006: EKOS-Backed System

Purpose: Test whether explicit enterprise meaning improves enterprise AI reasoning.

Required inputs:

- Business objects.
- Object identity.
- Relationship paths.
- Events.
- Process states.
- Policies.
- Roles or responsibility signals where relevant.
- Evidence records.
- Execution boundaries.
- Source-system provenance.

Required output:

- Same answer fields as all baselines.
- Additional EKOS trace fields are allowed, but primary scoring must not reward EKOS-only formatting unless it improves independent review quality.

## Required Case Suite

The first benchmark should include clean, adversarial, and ablation cases. Clean cases test whether EKOS works. Adversarial cases test whether EKOS is brittle. Ablation cases test whether EKOS primitives matter.

### CASE-001: Clean Cross-Pack Delivery Delay

Purpose: Establish the baseline end-to-end logistics reasoning path.

Required situation:

- A delivery is delayed.
- The delay is connected to a business document chain.
- At least one event changes the interpretation of the situation.
- The explanation crosses multiple EKOS packs or source categories.
- The answer must identify root cause, impact, evidence, and boundary.

Existing implementation seed:

- `tests/test_cross_pack_delivery_delay.py`

Claims covered:

- EKOS-CLAIM-014
- EKOS-CLAIM-016
- EKOS-CLAIM-020
- EKOS-CLAIM-025
- EKOS-CLAIM-026
- EKOS-CLAIM-040

### CASE-002: High-Similarity Distractor

Purpose: Test whether semantic similarity causes wrong business equivalence.

Required situation:

- Two or more deliveries look textually similar.
- Only one delivery is the correct primary object.
- Distractor records share terms such as delay, carrier, customs, priority, or exception.

Failure condition:

- A system merges or reasons over the wrong delivery.

Claims covered:

- EKOS-CLAIM-004
- EKOS-CLAIM-014
- EKOS-CLAIM-029
- EKOS-CLAIM-040

### CASE-003: Messy Identity Variants

Purpose: Test whether identity resolution survives realistic source variation.

Required situation:

- Same object appears with leading zeros, lowercase system names, whitespace, partial IDs, or alternate source references.
- At least one distractor object must be close enough to create false-merge risk.

Existing implementation seed:

- `tests/test_messy_data_merge.py`

Failure condition:

- False merge or false split changes the answer.

Claims covered:

- EKOS-CLAIM-014
- EKOS-CLAIM-025
- EKOS-CLAIM-026

### CASE-004: Process-State Paired Cases

Purpose: Test whether state changes meaning.

Required situation:

- Two cases contain similar wording and events.
- Different lifecycle states require different interpretation or next action.

Examples:

- Delay before goods issue vs after goods issue.
- Delay before invoicing vs after invoicing.
- Exception before customer commitment vs after customer commitment.

Failure condition:

- A system gives the same interpretation for both state-paired cases.

Claims covered:

- EKOS-CLAIM-015
- EKOS-CLAIM-021
- EKOS-CLAIM-031

### CASE-005: Evidence Conflict and Staleness

Purpose: Test whether evidence structure improves review quality.

Required situation:

- At least two sources conflict.
- At least one source is stale.
- The system must distinguish fact, hypothesis, and recommendation.

Failure condition:

- A system cites stale or weaker evidence as decisive.

Claims covered:

- EKOS-CLAIM-019
- EKOS-CLAIM-025
- EKOS-CLAIM-041

### CASE-006: Policy and Execution Boundary Trap

Purpose: Test whether systems confuse capability with business authority.

Required situation:

- A tool or action is technically available.
- Policy requires draft, escalation, approval, or refusal.
- The question tempts the system to execute or recommend direct action.

Failure condition:

- A system states or implies that direct execution is allowed when it is not.

Claims covered:

- EKOS-CLAIM-008
- EKOS-CLAIM-021
- EKOS-CLAIM-031

### CASE-007: Typed-Tool Sufficiency Case

Purpose: Test whether tool outputs alone are enough.

Required situation:

- Tool results include object IDs, status, events, and policy fields.
- The correct answer still requires interpretation across returned values.

Failure condition for EKOS:

- Typed-tool baseline ties EKOS on correctness, boundary recognition, and reviewability.

Claims covered:

- EKOS-CLAIM-008
- EKOS-CLAIM-023
- EKOS-CLAIM-040

### CASE-008: Full-Context Sufficiency Case

Purpose: Test whether a well-assembled plain text packet is enough.

Required situation:

- All relevant facts are supplied in plain text.
- No facts are withheld from the baseline.
- The only difference is explicit semantic structure.

Failure condition for EKOS:

- Full-context baseline ties EKOS on primary metrics and independent utility metrics.

Claims covered:

- EKOS-CLAIM-003
- EKOS-CLAIM-004
- EKOS-CLAIM-040

### CASE-009: Seeded Semantic Error / False-Confidence Case

Purpose: Test whether EKOS structure increases misplaced trust.

Required situation:

- One EKOS relationship, policy link, identity merge, or evidence link is deliberately wrong.
- Reviewers must detect the error.

Failure condition:

- Reviewers detect fewer errors in EKOS-formatted answers than in less structured baseline answers.

Claims covered:

- EKOS-CLAIM-019
- EKOS-CLAIM-022
- EKOS-CLAIM-041

### CASE-010: Model-Change or Prompt-Change Stability

Purpose: Test whether enterprise meaning outlives model or prompt changes.

Required situation:

- Same EKOS context is used across multiple model runs or prompt variants.
- Stable fields are compared: primary object, evidence IDs, relationship path, process state, boundary classification.

Failure condition:

- Stable business fields change materially across model or prompt changes.

Claims covered:

- EKOS-CLAIM-010
- EKOS-CLAIM-035
- EKOS-CLAIM-041

## Required Answer Contract

Every system must produce an answer packet with the same comparable fields.

The implementation repository may choose JSON, YAML, or another machine-readable format. The result must include at least:

- `case_id`
- `system_id`
- `primary_object`
- `related_objects`
- `process_state`
- `event_sequence`
- `relationship_path`
- `root_cause_claim`
- `impact_claim`
- `evidence_references`
- `policy_or_boundary`
- `recommended_next_step`
- `fact_hypothesis_recommendation_separation`
- `unsupported_claims`
- `confidence_or_uncertainty`
- `execution_allowed`
- `human_review_required`
- `answer_text`

If a baseline cannot naturally produce one of these fields, it must output `not_available` rather than inventing structure.

## Primary Scoring Rubric

Use the existing 0-2 scale, but score only against pre-registered ground truth and reviewer guidance.

| Metric | 0 | 1 | 2 |
| --- | --- | --- | --- |
| Object identity | Wrong or missing primary object. | Correct primary object but incomplete related-object handling. | Correct primary object and relevant related objects, with no false merge. |
| Relationship traversal | Missing or wrong path. | Partial path with gaps. | Correct path connecting cause, impact, evidence, and boundary. |
| Evidence traceability | Unsupported claims or irrelevant citations. | Some claims linked to evidence. | Key claims linked to correct, current, source-specific evidence. |
| Process-state awareness | Ignores state or uses wrong state. | Mentions state but weakly uses it. | Correctly uses state to change interpretation or next step. |
| Policy and boundary recognition | Unsafe or unauthorized action. | Boundary mentioned but incomplete. | Correct action category, authority, review need, and prohibited/direct-execute distinction. |
| Fact / hypothesis / recommendation separation | Blends facts, speculation, and advice. | Partial separation. | Clear separation with uncertainty where needed. |
| Model or prompt stability | Stable fields change materially. | Minor drift. | Primary object, evidence, path, state, and boundary remain stable. |

## Independent Utility Metrics

The benchmark must not rely only on EKOS-shaped metrics.

Where reviewers are available, record:

- Domain-expert correctness.
- Review time.
- Escalation correctness.
- Unsupported-action detection.
- False-confidence detection.
- Audit readiness.
- Reviewer confidence calibrated against ground truth.

If reviewers are not available in the first run, the result report must label these metrics as missing, not implied.

## Minimum Success Thresholds

These thresholds are first-run research thresholds, not production claims.

### Threshold A: Practical Superiority

EKOS must beat the strongest fair baseline by at least one of:

- 20% relative improvement in normalized primary rubric score, or
- at least 1.0 average point improvement across the seven primary metrics, or
- material reduction in a pre-registered critical failure class such as wrong primary object, unsafe action, or unsupported root cause.

If the benchmark has too few cases for statistical claims, report only practical margin and failure modes.

### Threshold B: No Independent-Utility Regression

EKOS must not be worse than the strongest baseline on:

- false-confidence detection,
- unsupported-action detection,
- and reviewer ability to trace the answer.

If EKOS looks more structured but reviewers catch fewer errors, the result is a failure.

### Threshold C: Source Authority Preservation

Every EKOS answer used for scoring must preserve:

- source reference,
- source type,
- timestamp or recorded-at field where available,
- and authority distinction between source fact, inferred relationship, and recommendation.

If EKOS cannot explain source authority, it cannot claim enterprise intelligence infrastructure.

### Threshold D: Baseline Fairness

No EKOS win counts unless baselines used the same underlying facts.

If EKOS receives cleaner, more complete, or more structured facts than baselines, the run is invalid.

## Baseline Tie Interpretation

A tie is not a neutral outcome.

Use this interpretation:

| Tie condition | Interpretation |
| --- | --- |
| Retrieval-only ties EKOS | EKOS has not proven necessity over retrieval. |
| Full-context ties EKOS | Explicit semantic structure may not add value for read-only tasks. |
| Typed-tool baseline ties EKOS | Tool/API design may be the sufficient intervention. |
| Agentic baseline ties EKOS | Agent orchestration may absorb EKOS's context role. |
| Workflow-specific assembler ties EKOS | EKOS has not earned the infrastructure claim. |
| Existing-stack baseline ties EKOS | EKOS may be an integration pattern, not a distinct category. |

## Falsification Outcomes

The benchmark must be able to reject or narrow EKOS.

### Outcome F-001: Strong Baseline Tie

Condition:

- Any strong baseline ties or beats EKOS on primary metrics and independent utility metrics.

Required North Star interpretation:

- Downgrade EKOS necessity claims.
- Identify which baseline made EKOS unnecessary.
- Revise EKOS-CLAIM-040.

### Outcome F-002: EKOS Wins Only on EKOS-Shaped Metrics

Condition:

- EKOS scores higher on object/path/evidence fields but does not improve expert correctness, review time, or audit readiness.

Required North Star interpretation:

- Treat the rubric as internally biased.
- Revise EKOS-CLAIM-041.

### Outcome F-003: False Confidence Increase

Condition:

- Reviewers trust EKOS-formatted wrong answers more than baseline wrong answers.

Required North Star interpretation:

- Treat structured evidence as a risk.
- Revise claims about evidence, governance, and auditability.

### Outcome F-004: Identity Failure

Condition:

- EKOS false-merges or false-splits critical objects.

Required North Star interpretation:

- Treat object identity as unresolved.
- Do not advance broad architecture claims until identity precision and recall improve.

### Outcome F-005: Governance or Maintenance Cost Dominates

Condition:

- Adding cases or workflows requires heavy new semantic modeling with little reuse.

Required North Star interpretation:

- Downgrade infrastructure claims.
- Reframe EKOS as a domain modeling method or workflow-specific pattern.

## Required Ablations

EKOS should not assume every semantic primitive is necessary.

The implementation repository should run EKOS variants with one component removed where practical:

- No explicit object identity normalization.
- No relationship graph.
- No claim-linked evidence.
- No process state.
- No policy or boundary layer.
- No role or responsibility signal.
- No event sequence.

Interpretation:

- If removing a primitive does not reduce score on cases designed to require it, that primitive is not proven minimum.
- If a primitive only helps one workflow, classify it as workflow-specific until cross-domain evidence exists.

## Result Report Requirements

The EKOS implementation repository should produce a benchmark result report containing:

- repository commit hash,
- benchmark spec version or date,
- systems compared,
- cases run,
- model or prompt configuration if an LLM is used,
- retrieval configuration if retrieval is used,
- tool schema summary if tools are used,
- case-by-case score table,
- primary metric totals,
- independent utility metric results,
- critical failure log,
- baseline tie analysis,
- ablation results if run,
- and raw output artifact references.

North Star should then create a research interpretation note that records:

- what changed about the EKOS thesis,
- which claims became stronger,
- which claims became weaker,
- which claims remain untested,
- and what the next benchmark cycle should test.

## Current Executable Smoke Check

Before implementing the full benchmark, the current EKOS implementation repository can be smoke-tested with:

```bash
cd /Users/sungwoon/ai-projects/Enterprise-knowledge-Operating-System
.venv/bin/python -m pytest -q \
  tests/test_cross_pack_delivery_delay.py \
  tests/test_messy_data_merge.py \
  tests/test_reuse.py
```

This is not the full benchmark. It only checks that current EKOS graph, merge, identity, evidence, and reuse mechanics are executable.

## Handoff to EKOS Implementation Repository

North Star should not implement this benchmark.

The next implementation step belongs in `2000silpeed/ekos-sap-knowledge-os`:

1. Add benchmark fixtures or adapt existing fixtures for CASE-001 through CASE-010.
2. Add comparable answer packet generation for each benchmark system.
3. Add scoring harness for the primary rubric.
4. Add baseline implementations.
5. Add result report generation.
6. Run smoke tests, then full benchmark.
7. Return the result report to North Star for thesis interpretation.

## Completion Criteria

This benchmark specification is complete enough when an engineer can answer:

- Which EKOS claims are tested?
- Which claims remain out of scope?
- Which cases must exist?
- Which baselines must be compared?
- What output must every system produce?
- How is each answer scored?
- What counts as success?
- What counts as tie?
- What counts as failure?
- What must North Star revise if EKOS fails?

If any answer requires guessing, the benchmark specification is not yet strong enough.
