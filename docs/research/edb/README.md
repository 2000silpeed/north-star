# Enterprise Delegation Benchmark (EDB)

**Status:** Research track

**Created:** 2026-06-29

---

## Purpose

Enterprise Delegation Benchmark (EDB) is a research track for evaluating a question that ordinary AI benchmarks usually do not isolate:

> How much enterprise authority can safely be delegated to an AI system?

EDB begins as an internal EKOS research framework. It should not yet be claimed as an industry standard or general benchmark until it demonstrates discriminative value across multiple systems and reviewers.

---

## Relationship to EKOS

EKOS is the implementation system being tested.

EDB is the evaluation framework that asks whether EKOS converts enterprise meaning into safer and more useful delegation decisions.

North Star owns:

- research framing
- hypotheses
- taxonomy
- metrics
- protocol
- falsification rules

`2000silpeed/ekos-sap-knowledge-os` owns:

- runnable benchmark cases
- fixtures
- scoring implementation
- baseline implementations
- test results

---

## Research Sequence

1. `001-gap-analysis.md` — determine whether existing benchmarks already measure safe enterprise delegation.
2. `002-taxonomy.md` — define delegation levels and authority categories.
3. `003-metrics.md` — define safety, utility, and delegatability metrics.
4. `004-protocol.md` — define benchmark execution and scoring protocol.
5. `005-benchmark-cases.md` — define CASE-011 through CASE-015.
6. `006-live-model-comparison.md` — define EDB Phase 2: live commercial model comparison using one prompt, one schema, and the existing scorer.
7. `007-live-model-runner-implementation.md` — record EKOS Issue #3 implementation, verification, and remaining live-run limits.

---

## Current Hypotheses

### H1 — Benchmark Gap

Existing AI benchmarks do not directly measure safe enterprise delegation.

### H2 — Delegation Is Distinct from Accuracy

Enterprise delegation can be evaluated independently from raw answer accuracy.

### H3 — Business Context Enables Delegation

Safe delegation depends on explicit business context, not only model capability.

### H4 — Live Model Comparison Is Required

Internal deterministic baselines are not enough to make a strong external research claim.

EDB must compare EKOS against live commercial model outputs using the same prompt, schema, and scorer before public claims about superiority become persuasive.

---

## Current Constraint

EDB must remain falsifiable.

If existing benchmarks already measure delegation authority, approval boundaries, and over-delegation risk, EDB should be reframed as an enterprise delegation profile over existing benchmarks rather than a standalone benchmark.
