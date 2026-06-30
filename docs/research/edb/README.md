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
8. `008-cli-agent-preliminary-run.md` — record the Claude Code / Codex CLI preliminary runner and one CASE-011~015 execution.
9. `009-real-model-data-r5.md` — record the first 5-run real data collection across Claude Code CLI, Codex CLI, and Gemini 2.5 Flash API.
10. `010-consistency-stress-test-plan.md` — record EKOS Issue #4 implementation and the first consistency analysis over R5 real-model data.
11. `011-prompt-variant-stress-test-r3.md` — record the first prompt-variant stress test over CLI agents, including Codex stability and Claude session-limit contamination.
12. `012-ekos-delta-benchmark.md` — define and record the first EKOS Delta Benchmark implementation: same model before vs after EKOS structured context.
13. `013-delta-causality-controls.md` — record negative controls, robustness controls, ablation, and the Provider API Delta runner needed to separate EKOS context effects from CLI-agent product-wrapper effects.

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

### H5 — Correct Delegation Level Is Not Enough

R5 data showed that Claude Code CLI, Codex CLI, and Gemini 2.5 Flash can keep
`maximum_safe_delegation_level` stable across CASE-011 through CASE-015.

The current stronger hypothesis is that enterprise delegation quality also
depends on consistency of evidence, policy, blocking-risk, reversibility,
confidence, and audit fields.

### H6 — Prompt Wording Can Preserve Authority While Shifting Reviewability

Prompt-variant R3 showed that Codex CLI preserved `maximum_safe_delegation_level`,
`action_category`, `evidence_ids`, and `policy_ids` across six prompt variants,
but still varied `audit_note_proxy`, `reversibility`, and `blocking_risks`.

This suggests prompt robustness must be measured separately for authority
fields and enterprise reviewability fields.

### H7 — EKOS Should Be Measured as Context Delta, Not as a Model

The strongest EKOS comparison is not `EKOS vs Codex`.

It is:

```text
Codex
vs
Codex + EKOS Context
```

The early delta smoke suggests EKOS context can improve reviewability score
while preserving max-safe accuracy and avoiding over-delegation. This remains a
small smoke result, not a superiority claim.

### H8 — CLI-Agent Causality Is Not Provider-Independent Evidence

Negative control, robustness, and ablation runs have strengthened the Codex CLI
causality story, but Codex CLI is still a product wrapper around model behavior.

The next evidence gap is a clean provider API delta:

```text
Provider API model
vs
Provider API model + EKOS Context
```

Until real provider API delta data exists, EDB should not claim
provider-independent EKOS causal value.

---

## Current Constraint

EDB must remain falsifiable.

If existing benchmarks already measure delegation authority, approval boundaries, and over-delegation risk, EDB should be reframed as an enterprise delegation profile over existing benchmarks rather than a standalone benchmark.
