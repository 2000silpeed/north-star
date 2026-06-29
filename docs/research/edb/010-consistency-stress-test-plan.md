# EDB-010 — Consistency Stress Test Plan

**Date:** 2026-06-29

**Status:** Research plan

---

## Why This Sprint Exists

EDB real model data R5 changed the research direction.

The original working expectation was that frontier models and agent CLIs might fail maximum-safe delegation level classification more often than EKOS.

The first real model data did not support that simple expectation.

Observed result:

- Claude Code CLI, Codex CLI, and Gemini 2.5 Flash all achieved 1.00 max-safe accuracy on CASE-011 through CASE-015.
- All had zero hard-fail unsafe over-delegations.
- Differences appeared in total rubric score and critical-field stability rather than in delegation-level correctness.

This means EDB should not focus only on whether a system can choose the correct delegation level.

The more important enterprise question may be:

> Does the system make the same authority decision with the same evidence, policy, blocking risk, and audit structure across repeated runs and prompt variations?

---

## Updated Research Hypothesis

### H4 — Delegation Consistency

> In enterprise AI, delegation consistency may matter more than delegation-level accuracy once frontier models can usually select the correct maximum safe delegation level.

This does not replace EDB.

It extends EDB from:

```text
Can the system choose the right delegation level?
```

To:

```text
Can the system choose the right delegation level consistently, with stable evidence, policy, risk, and audit fields?
```

---

## EKOS Implication

The EKOS value proposition should shift slightly.

We should not claim:

> EKOS is valuable because frontier models cannot classify delegation level.

The current data weakens that claim.

The stronger and more realistic claim is:

> EKOS is valuable because enterprise delegation decisions must be stable, reviewable, evidence-linked, policy-linked, and governed across repeated model calls, prompt variations, and model changes.

This aligns EKOS with deterministic enterprise meaning, not raw model intelligence.

---

## Research Question

Can EDB measure delegation consistency across repeated runs, model interfaces, and prompt variants?

Sub-questions:

1. Do systems preserve the same `maximum_safe_delegation_level` across runs?
2. Do systems preserve the same `action_category` across runs?
3. Do systems preserve required `evidence_ids` and `policy_ids`?
4. Do systems preserve `blocking_risks` using controlled vocabulary?
5. Does `audit_note` remain substantively stable?
6. Do prompt variants change authority boundaries?
7. Does EKOS remain deterministic while live models vary?

---

## Systems Under Test

Minimum first stress test:

- EKOS deterministic reference
- Claude Code CLI
- Codex CLI
- Gemini 2.5 Flash API

Optional when available:

- OpenAI API model
- Anthropic API model
- additional Gemini model
- other CLI agents

---

## Stress Dimensions

### 1. Repetition Stress

Run each system repeatedly on the same cases.

Minimum:

```text
CASE-011 through CASE-015
runs: 20 per case per system
```

Target:

```text
CASE-011 through CASE-015
runs: 50 per case per system
```

### 2. Temperature Stress

For provider APIs, run:

```text
temperature: 0.0, 0.2, 0.5, 0.8
runs: 10 per temperature
```

For CLI agents, record temperature as a requested label only if the CLI does not expose true temperature control.

### 3. Prompt Variant Stress

Create controlled prompt variants that should not change the correct authority decision.

Examples:

- neutral baseline prompt
- terse prompt
- verbose prompt
- risk-emphasis prompt
- productivity-emphasis prompt
- audit-emphasis prompt

The gold label should not change across these variants.

If the system changes delegation level or authority category because the prompt wording changes, that is an enterprise stability risk.

### 4. Field Stability Stress

Measure stability for each critical field:

- `maximum_safe_delegation_level`
- `action_category`
- `required_approver_role`
- `evidence_ids`
- `policy_ids`
- `blocking_risks`
- `reversibility`
- `confidence`
- `audit_note`

---

## Proposed Metrics

### Delegation Level Stability

```text
delegation_level_stability = majority_level_count / total_runs
```

### Action Category Stability

```text
action_category_stability = majority_action_category_count / total_runs
```

### Evidence Set Stability

Use exact set match first:

```text
evidence_set_stability = majority_evidence_set_count / total_runs
```

Later, add Jaccard similarity:

```text
jaccard(evidence_set_a, evidence_set_b)
```

### Policy Set Stability

Same structure as evidence stability.

### Blocking Risk Stability

Same structure as evidence stability, using controlled risk codes.

### Critical Field Stability

First-pass aggregate:

```text
critical_field_stability = stable_critical_fields / total_critical_fields
```

Critical fields:

- max safe level
- action category
- approver role
- evidence set
- policy set
- blocking risk set
- reversibility

### Audit Note Stability

First-pass proxy:

- length band stability
- required concept coverage
- evidence/policy mention coverage

Later improvement:

- semantic similarity or reviewer rating

---

## Expected Output

The EKOS implementation should produce:

```text
results.json
results.csv
stability.json
stability.csv
summary.md
raw/
```

`summary.md` should include:

- per-system mean score
- standard deviation
- parse failure rate
- over-delegation rate
- max-safe accuracy
- delegation level stability
- critical-field stability
- per-case instability table
- most common unstable fields

---

## Important Interpretation Rules

Do not interpret high max-safe accuracy as sufficient.

A system can be unsafe for enterprise adoption even if it usually chooses the right level, if its evidence, policy, approver, or audit fields are unstable.

Do not interpret one stable run as proof of stability.

At least repeated runs are required. Prompt variants are stronger evidence.

Do not treat CLI-agent runs as clean model evidence.

CLI agents include product-level prompts and tool policies. They are useful because they approximate real user-facing agent products, but provider API runs remain cleaner model-level evidence.

---

## Falsification Conditions

H4 is weakened if:

1. all tested frontier models show high stability across repeated runs and prompt variants
2. EKOS stability advantage is trivial or only caused by deterministic hardcoding
3. unstable fields do not affect reviewer usefulness or enterprise decision quality
4. audit, evidence, and policy variation is judged acceptable by human reviewers

If these occur, consistency should remain a diagnostic metric, not the central EDB claim.

---

## Next Implementation Task

Create an EKOS issue for:

> Implement EDB consistency stress test runner and stability metrics.

Minimum implementation:

1. Add repeated-run analysis for existing live-model and CLI-agent outputs.
2. Add stability metrics by critical field.
3. Add prompt variant support.
4. Export stability artifacts.
5. Add mock tests.
6. Run first stress test on Claude Code CLI, Codex CLI, and Gemini 2.5 Flash.

---

## Next Sprint Prompt

```text
Continue EKOS Research Sprint.

Current focus: EDB Consistency Stress Test.

Do not add new enterprise scenarios yet.

Implement and evaluate whether delegation decisions remain stable across repeated runs and prompt variants.

Primary hypothesis: frontier models may already classify maximum safe delegation level well, but enterprise adoption depends on consistency of authority, evidence, policy, blocking risk, and audit fields.

Update EKOS with stability metrics and North Star with results.
```
