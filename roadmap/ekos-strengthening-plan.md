# EKOS Strengthening Plan

Date: 2026-06-27

Status: Active project plan

Owner: Project North Star

Implementation boundary: Runnable experiments, benchmark code, datasets, applications, APIs, CLIs, demos, and prototypes belong in `2000silpeed/ekos-sap-knowledge-os`. North Star owns the research plan, evaluation specification, decision memory, glossary, ADRs, white paper, and portfolio narrative.

## Mission

North Star exists to ensure that EKOS becomes stronger with every research cycle.

North Star is not the product. North Star is the research headquarters for EKOS.

AIOS is the operating rule set. EKOS is the product. North Star must keep asking one question:

> Does this change make EKOS stronger?

If the answer is yes, preserve it.

If the answer is no, do not expand it. Leave it unchanged or move it to the Parking Lot.

## Commit KPI

Every meaningful North Star commit must pass this test:

> If this commit disappeared tomorrow, would EKOS become weaker?

Answer yes:

- Keep the change.
- Commit it as durable research memory.
- Link it to a claim, decision, evaluation gap, glossary ambiguity, ADR, benchmark need, or implementation-repository handoff.

Answer no:

- Do not commit it.
- Move it to the Parking Lot if it may matter later.
- Delete it if it is only repository decoration, writing volume, or speculative structure.

## What Counts as Making EKOS Stronger

A North Star change strengthens EKOS when it does at least one of these:

- Makes an EKOS claim more testable or falsifiable.
- Defines evidence that would accept, reject, narrow, or revise a claim.
- Reduces ambiguity in a concept that blocks evaluation or implementation.
- Clarifies the boundary between North Star and `2000silpeed/ekos-sap-knowledge-os`.
- Produces a benchmark specification that the EKOS implementation repository can execute.
- Turns hostile review findings into concrete evaluation requirements.
- Records a decision that prevents future architectural drift.
- Prevents overclaiming in the white paper or public narrative.
- Connects EKOS research artifacts to implementation work without creating implementation inside North Star.

A North Star change does not strengthen EKOS merely because it:

- Adds more documentation.
- Makes the repository look more complete.
- Improves wording without improving research quality.
- Expands AIOS as a generic operating system.
- Creates a new taxonomy that no current EKOS claim needs.
- Adds speculative architecture before evidence requires it.
- Designs implementation files that belong in the EKOS repository.

## Current State

North Star already has the core research assets needed to move from thesis to research program:

- EKOS white paper: `docs/whitepaper/the-ekos-white-paper.md`
- Korean white paper: `docs/whitepaper-ko/the-ekos-white-paper-ko.md`
- Validation matrix: `docs/research/ekos-validation-matrix.md`
- Hostile review: `docs/reviews/review-001-hostile-review.md`
- Falsification strategy: `docs/reviews/falsification-strategy.md`
- Glossary roadmap: `docs/glossary/glossary-roadmap.md`
- Repository boundary ADR: `docs/adr/ADR-0002-north-star-ekos-repository-boundary.md`
- AIOS operating rules: `AIOS/`

The biggest weakness is no longer lack of narrative.

The biggest weakness is no longer the absence of a first validation path.

The first benchmark specification now exists in North Star, and CASE-001 through CASE-010 have runnable smoke implementations in `2000silpeed/ekos-sap-knowledge-os`.

The current weakness is evidence independence:

- The cases are synthetic smoke benchmarks.
- The baseline suite still lacks a workflow-specific context assembler baseline.
- CASE-010 prompt/model stability is deterministic simulation, not live model evidence.
- Independent utility metrics are proxy metrics, not domain-reviewer measurements.

The current result interpretation is recorded in `docs/research/2026-06-28-ekos-benchmark-results-001-010.md`.

## Highest-Leverage Next Improvement

The single highest-leverage next task is:

> Define the next validation cycle that closes the largest evidence gaps exposed by CASE-001 through CASE-010.

This has higher leverage than glossary expansion, white paper rewriting, AIOS expansion, or portfolio work because:

- CASE-001 through CASE-010 already provide a repeatable first-pass signal.
- The current strongest objection is no longer "no benchmark exists"; it is that the benchmark still depends on synthetic fixtures, deterministic proxy metrics, and an incomplete baseline suite.
- A workflow-specific context assembler baseline could narrow or invalidate the EKOS necessity claim.
- Live model or reviewer-based runs could reveal whether the deterministic proxy wins hold outside the harness.
- The next handoff should tell the EKOS implementation repository exactly which missing evidence would most strengthen or falsify EKOS.

Until these evidence gaps are closed, more white paper work risks becoming persuasion instead of research.

## Operating Cycle

Every North Star research cycle should follow this sequence:

1. Read AIOS and the latest relevant research artifacts.
2. Ask which single change would most strengthen EKOS.
3. Reject lower-leverage work unless it directly unblocks the chosen change.
4. Make one focused change.
5. Record why it matters, what it rejects, and how it affects EKOS.
6. Commit the result if EKOS would become weaker without it.
7. Do not push unless the user asks.

## Priority Roadmap

### Phase 1: Validation Contract

Goal: Convert the current EKOS thesis into a benchmark specification that can be implemented and run in the EKOS repository.

North Star owns:

- Benchmark purpose and scope.
- Claims covered by each test.
- Case requirements.
- Baseline requirements.
- Scoring rubric.
- Falsification thresholds.
- Success and failure interpretation.
- Required result-reporting format.

EKOS repository owns:

- Dataset fixtures.
- Runnable benchmark code.
- Retrieval baseline implementation.
- Full-context baseline implementation.
- Typed-tool baseline implementation.
- EKOS-backed system implementation.
- Evaluation harness and result artifacts.

Deliverable:

- `docs/research/ekos-benchmark-specification.md`

The first version should define:

- which claims are in scope,
- which claims are explicitly out of scope,
- required SAP logistics cases,
- required distractor and adversarial cases,
- required baselines,
- primary metrics,
- independent utility metrics,
- minimum success thresholds,
- baseline tie interpretation,
- falsification outcomes,
- and how EKOS repository results should be reported back to North Star.

Completion test:

> A future AI or engineer can implement the benchmark in `2000silpeed/ekos-sap-knowledge-os` without guessing what North Star meant.

### Phase 2: Baseline Discipline

Goal: Prevent EKOS from winning against weak or unfair baselines.

Deliverable:

- A baseline requirements section inside the benchmark specification, or a separate baseline spec only if the benchmark document becomes too dense.

Required baselines:

- Retrieval-only baseline.
- Full-context prompt baseline.
- Typed-tool/API baseline.
- Agentic retrieval-and-tool baseline.
- Workflow-specific context assembler baseline.

Completion test:

> If EKOS wins, the result cannot be dismissed as beating naive chunk retrieval only.

### Phase 3: Metric Calibration

Goal: Ensure the evaluation does not reward EKOS-shaped output instead of enterprise utility.

Deliverable:

- Metric calibration rules in the benchmark specification.

Required checks:

- Inter-rater agreement.
- Domain expert correctness.
- Review time.
- Escalation correctness.
- False-confidence detection.
- Unsupported-action detection.
- Audit readiness.

Completion test:

> A high EKOS score must mean something outside the EKOS architecture itself.

### Phase 4: Glossary Only Where Evaluation Requires It

Goal: Define only the concepts that block measurement, implementation handoff, or claim interpretation.

Do not define every term in the glossary roadmap yet.

Start with:

- Enterprise Meaning
- Business Object
- Object Identity
- Evidence
- Claim
- Relationship
- Process State
- Policy
- Execution Boundary
- System of Record
- Baseline
- Falsification Condition

Completion test:

> Each definition must change how a claim is scored, falsified, or implemented in the EKOS repository.

### Phase 5: Cross-Repository Handoff

Goal: Turn North Star research specs into EKOS implementation tasks without creating implementation in North Star.

North Star should produce:

- Implementation handoff notes.
- Claim-to-test mapping.
- Result interpretation criteria.
- Links to relevant specs and ADRs.

The EKOS repository should produce:

- Code.
- Data.
- Tests.
- Benchmark runs.
- Result logs.
- Implementation decisions.

Completion test:

> The EKOS repository can build and run the benchmark while North Star remains the research memory.

### Phase 6: Result Interpretation and Thesis Revision

Goal: Use benchmark results to strengthen or narrow EKOS.

Possible outcomes:

- EKOS wins against strong baselines: strengthen the thesis cautiously.
- EKOS ties strong baselines: downgrade necessity claims.
- EKOS wins only on EKOS-shaped metrics: revise the evaluation.
- EKOS fails adversarial cases: improve the thesis or narrow the domain.
- EKOS proves too costly: reframe as a method or pattern, not infrastructure.

Completion test:

> North Star records what the evidence changed, not what the team hoped it would show.

### Phase 7: Public Narrative After Evidence

Goal: Update the white paper and portfolio only after the research program has stronger evidence.

Allowed work:

- Revise public claims to match benchmark results.
- Add limitations where evidence is weak.
- Explain validated findings without production overclaiming.

Not allowed:

- Marketing language ahead of evidence.
- Production-readiness claims from synthetic data.
- Broad enterprise infrastructure claims from a single logistics benchmark.

Completion test:

> Public narrative becomes more credible because it became more constrained.

## Parking Lot Rules

Move work to the Parking Lot when it is interesting but does not currently make EKOS stronger.

Parking Lot examples:

- Generalizing AIOS beyond North Star.
- Creating a separate AIOS repository.
- Expanding portfolio narrative before EKOS evidence improves.
- Writing more white paper sections without new evidence.
- Defining every glossary term before the benchmark needs it.
- Designing implementation architecture inside North Star.
- Adding public-facing polish before validation.

Parking Lot rule:

> A parked idea is not rejected. It is not allowed to consume the next research cycle.

## Decision Heuristic for Every New Request

When a new task appears, classify it:

1. Direct EKOS strengthener: do it first.
2. EKOS blocker remover: do it if it unblocks the direct strengthener.
3. Research memory preservation: do it if the reasoning would otherwise be lost.
4. Parking Lot: save only if it may matter later.
5. Reject: do not do it if it creates implementation drift, documentation volume, or speculative architecture.

If two tasks compete, choose the one that improves the next empirical test of EKOS.

## Immediate Next Task

Use the CASE-001 through CASE-010 result interpretation to define the next validation cycle.

The next North Star task should not be more white paper writing.

It should produce a focused research handoff that answers:

- Which missing baseline most threatens the current EKOS result?
- Which deterministic proxy metrics must become live-run or human-review metrics?
- Which EKOS claims are supported only by synthetic smoke evidence?
- Which claim should be narrowed if a workflow-specific baseline ties EKOS?
- What exactly should `2000silpeed/ekos-sap-knowledge-os` implement next?

Do not create runnable code, sample data, app files, or implementation directories in North Star.

## Current Working Principle

North Star should not become larger by default.

North Star should become sharper.

The measure is not document count. The measure is whether EKOS would be weaker without the commit.
