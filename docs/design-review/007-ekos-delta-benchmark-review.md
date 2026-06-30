# EKOS Design Review Note #7

## EKOS Delta Benchmark Research Control

**Date:** 2026-06-30

---

# Design Review Question

The current EDB track has moved beyond model-vs-model comparison.

The active research question is now:

> Does adding EKOS structured context improve the same model or CLI agent's delegation decision quality, reviewability, and consistency without increasing unsafe over-delegation?

This is a stronger and cleaner question than comparing different models. It tests the marginal value of EKOS itself.

---

# Repository State Read

This review was written after reading:

- `2000silpeed/north-star/README.md`
- `docs/adr/ADR-0001-research-continuity-workflow.md`
- `docs/design-review/006-delegation-validation-benchmark.md`
- `docs/research/ekos-validation-matrix.md`
- `docs/research/backlog/001-enterprise-roi-benchmark.md`
- `2000silpeed/ekos-sap-knowledge-os/README.md`
- latest EKOS implementation commit adding `benchmark delta`
- current open EKOS issues related to EDB and ERB

Current EKOS implementation state:

- CASE-011 through CASE-015 exist as the delegation validation suite.
- `benchmark live-models` exists for provider-style model runs.
- `benchmark cli-agents` exists for external CLI-agent runs.
- `benchmark consistency` exists for repeated-run stability analysis.
- `benchmark delta` exists for before/after comparison of the same CLI agent with and without EKOS structured context.

Therefore, the next research work should not be to invent a new benchmark family. It should be to harden the Delta Benchmark interpretation.

---

# Principal Reviewer Challenge

The Delta Benchmark is directionally correct, but it introduces a new failure mode:

> EKOS may appear to improve the model simply because the after-condition prompt is richer, longer, or closer to the scoring rubric, not because EKOS adds reusable enterprise meaning.

The benchmark must therefore distinguish three possible explanations:

1. **EKOS causal value** — structured context improves delegation decisions because enterprise meaning is preserved.
2. **Prompt enrichment effect** — any extra structured facts would improve the model similarly.
3. **Scorer-shape leakage** — the after prompt resembles the expected `DelegationDecisionContract` enough to improve formatting rather than judgment.

Only the first explanation supports the EKOS thesis.

---

# Research Decision

The EKOS Delta Benchmark should become the primary EDB implementation phase, but its claim must be constrained.

Allowed claim if the benchmark succeeds:

> For the same model or CLI agent, adding EKOS structured context improves EDB delegation outputs on pre-registered safety, reviewability, and consistency metrics.

Not allowed yet:

> EKOS improves enterprise ROI.

> EKOS is production-ready.

> EKOS is universally better than all model/tooling alternatives.

> EKOS alone is an authoritative policy or workflow control system.

---

# Required Delta Controls

## 1. Same-System Pairing

Every before/after comparison must use the same:

- system identifier
- case ID
- prompt variant
- run index
- schema
- scorer
- temperature where controllable

If pairing fails, the delta result should be invalid rather than partially interpreted.

## 2. No Completed Contract Leakage

The after-condition may include EKOS structured context, but it must not include a completed or near-completed `DelegationDecisionContract`.

Forbidden after-context fields:

- gold `maximum_safe_delegation_level`
- gold `action_category`
- gold `required_approver_role` if the case is designed to test approver inference
- completed `blocking_risks` if the case is designed to test risk detection
- completed evidence-to-authority mapping if that is the scored target

Allowed after-context fields:

- source facts
- object identifiers
- relationship paths
- source timestamps
- evidence records
- policy text or policy references
- capability boundaries
- business object state

## 3. Same Scorer, Same Schema

Before and after conditions must use the same JSON schema and scorer.

The after condition must not receive a scoring-specific shortcut unavailable to the before condition.

## 4. Negative Control Context

Add at least one future negative control condition:

```text
same model + non-EKOS structured context
```

This can be a plain JSON fact bundle or workflow-specific context assembler.

If the non-EKOS structured context ties EKOS, the claim narrows from:

> EKOS structured context improves delegation.

To:

> structured context improves delegation; EKOS has not yet proven unique value.

## 5. Stability Control

The delta should measure not only score improvement but also whether EKOS reduces instability in critical delegation fields:

- maximum safe delegation level
- action category
- evidence IDs
- policy IDs
- blocking risks
- reversibility
- confidence
- audit-note proxy features

A higher mean score with worse stability is not a clean win.

---

# Primary Delta Metrics

## Safety Metrics

- unsafe over-delegation rate
- hard-fail count
- maximum-safe-delegation accuracy
- action-category accuracy
- approval-boundary accuracy

Safety is the first gate. If safety worsens, the benchmark fails even if total score improves.

## Reviewability Metrics

- evidence-to-authority coverage
- policy-to-authority coverage
- blocking-risk detection
- reversibility classification
- audit-note usefulness

Reviewability is the core EKOS value proposition in EDB.

## Consistency Metrics

- authority-level stability
- action-category stability
- evidence-set stability
- policy-set stability
- blocking-risk stability
- reversibility stability
- critical-field stability mean

Consistency matters because enterprise adoption needs repeatable decisions, not lucky single answers.

## Utility Metrics

- correct enablement of Level 2 where justified
- correct enablement of Level 3 where justified
- under-delegation rate

A system that only refuses is safe but not adoption-enabling.

---

# Success Criteria

The Delta Benchmark should be considered a meaningful EKOS win only if all are true:

1. after-condition total score improves over before-condition
2. unsafe over-delegation does not increase
3. hard-fail unsafe delegation remains zero or decreases
4. reviewability metrics improve materially
5. critical-field stability does not degrade
6. Level 2 or Level 3 utility improves where the gold case permits it
7. pair alignment is complete
8. no completed-contract leakage is found

---

# Falsification Conditions

The EKOS Delta claim weakens if:

1. the after condition improves only formatting completeness but not authority correctness
2. the after condition increases over-delegation
3. EKOS improves score while worsening stability
4. a non-EKOS structured context control ties EKOS
5. the result depends on a single prompt variant
6. the result disappears under risk-emphasis or productivity-emphasis prompt variants
7. the result is driven by one case rather than the suite
8. the after prompt leaks gold labels or scorer-shaped answers

If these occur, narrow the claim to:

> EKOS-style context packaging can improve some delegation outputs, but EKOS has not yet proven unique or robust causal value.

---

# Implementation Implications for EKOS

Implementation belongs in `2000silpeed/ekos-sap-knowledge-os`.

Required next work:

1. Add a delta validity report section that explicitly checks pair alignment, leakage controls, and safety gates.
2. Add a negative-control structured-context condition.
3. Add per-case delta attribution so one case cannot hide weak generalization.
4. Add fail-fast behavior when unsafe over-delegation increases.
5. Add README wording that separates deterministic delta evidence from live-model or human-review evidence.

---

# North Star Update

The active EDB phase is now:

```text
EKOS Delta Benchmark
```

The research question is no longer primarily:

```text
Which model is better?
```

It is:

```text
What is Δ(Model + EKOS, Model) on enterprise delegation safety, reviewability, consistency, and utility?
```

This should remain the active track until the delta result is either validated or falsified.

---

# Next Sprint Prompt

```text
Continue EKOS Research Sprint.

Current focus: EDB EKOS Delta Benchmark.

Objective: harden `benchmark delta` in `2000silpeed/ekos-sap-knowledge-os` so Δ(Model + EKOS, Model) cannot be explained by prompt enrichment, scorer-shape leakage, or incomplete pair alignment.

Do not start ERB yet.
Do not claim ROI.
Do not broaden the philosophy.

Primary safety gate: unsafe over-delegation must not increase.
Primary research question: does EKOS structured context improve the same model's delegation safety, reviewability, consistency, and utility?

Add negative-control structured context if implementation time allows.
If a strong non-EKOS structured context ties EKOS, narrow the claim.
```
