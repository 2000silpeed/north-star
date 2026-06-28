# EDB-001 — Benchmark Gap Analysis

**Date:** 2026-06-29

**Status:** Draft

---

## Research Question

Do existing AI benchmarks already measure the enterprise question:

> How much authority can safely be delegated to an AI system?

If yes, EDB should not be positioned as a new benchmark. It should become an enterprise delegation profile or extension of existing benchmarks.

If no, EDB may be justified as a new evaluation axis.

---

## Hypothesis H1

> Existing AI benchmarks do not directly measure safe enterprise delegation.

This is not assumed true. It must be tested.

---

## What Counts as Direct Measurement?

A benchmark directly measures safe enterprise delegation only if it evaluates most of the following:

1. maximum safe authority level
2. approval boundary
3. role or owner required for approval
4. over-delegation risk
5. conservative failure / under-delegation
6. evidence-to-authority trace
7. policy-to-authority trace
8. reversibility or business impact
9. auditability of the delegation decision
10. distinction between capability and business permission

A benchmark that measures reasoning, tool use, or task completion alone does not directly measure delegation unless authority and approval boundaries are explicit scoring targets.

---

## Initial Benchmark Categories to Review

| Category | Example benchmark type | Likely measures | EDB gap to check |
| --- | --- | --- | --- |
| Knowledge QA | MMLU-style | factual and domain knowledge | no authority boundary |
| Coding | SWE-bench-style | issue resolution and code correctness | no enterprise approval boundary |
| Tool use | TAU-bench-style | tool use and task completion | may not separate capability from permission |
| Agent tasks | GAIA-style | multi-step problem solving | may not score delegation safety |
| RAG | retrieval QA evaluations | answer groundedness | usually no business authority level |
| Safety | refusal and harmfulness evaluations | policy compliance and risk avoidance | may not measure useful delegation enablement |
| Enterprise assistants | vendor demos and internal evals | workflow utility | often not public, reproducible, or authority-scored |

---

## Expected Gap

The expected gap is not that existing benchmarks are weak.

The expected gap is that they answer different questions:

- Can the model answer?
- Can the model use tools?
- Can the agent complete a task?
- Is the response harmful?

EDB asks:

> Given a business context and a requested task, what is the maximum safe level of enterprise authority this AI system should receive?

---

## Falsification Condition

H1 is weakened or rejected if an existing benchmark explicitly scores:

- maximum safe delegation level
- approval or execution authority
- over-delegation and under-delegation
- evidence and policy support for authority decisions

If that benchmark exists and is suitable for EKOS, EDB should reuse or extend it rather than duplicate it.

---

## Research Task

Before public claims, review current AI benchmark literature and vendor evaluation frameworks for:

- delegation
- authority
- approval boundary
- enterprise autonomy
- tool permission vs business permission
- human-in-the-loop handoff
- auditability

---

## Current Position

Until this review is complete, the correct claim is conservative:

> EDB is a proposed evaluation axis for EKOS research, motivated by the apparent absence of direct delegation-level scoring in common AI benchmarks.
