# EDB-012 — EKOS Delta Benchmark

**Date:** 2026-06-29

**Status:** Research specification

---

## Why This Sprint Exists

The EDB research direction changed after real-model and consistency data.

The first external model runs showed that frontier models and CLI agents can often select the correct `maximum_safe_delegation_level` on CASE-011 through CASE-015.

That means EKOS should not be framed as competing against frontier models on raw intelligence.

The better research question is:

> How much enterprise-grade reviewability and stability does EKOS add to the same model?

This requires a before/after benchmark:

```text
Model
vs
Model + EKOS
```

The measured value is the delta.

---

## Updated Hypothesis

### H6 — EKOS Delta

> Enterprise value should be measured as Δ(Model + EKOS, Model), not only as Model vs Model.

The core EKOS claim should become:

> EKOS improves the enterprise-grade reliability, reviewability, and stability of the same model by supplying structured enterprise meaning, evidence, policy, and boundary context.

This is stronger and fairer than claiming:

> EKOS beats GPT, Claude, Gemini, or Codex as a model.

EKOS is not a model. EKOS is an enterprise meaning and governance layer around model behavior.

---

## Core Research Question

How much does EKOS improve the same model on EDB metrics?

For each model or agent interface:

```text
Raw Model Output
        ↓
      EDB

Model + EKOS Context
        ↓
      EDB
```

Then calculate:

```text
Enterprise Delta Score = Score(Model + EKOS) - Score(Model)
```

---

## Required Comparison Design

The benchmark must compare the same underlying model or agent under two conditions.

### Condition A — Raw Model

The model receives the normal EDB prompt, case packet, JSON schema, and scoring instructions.

It does not receive EKOS-assembled semantic context beyond the case packet.

### Condition B — Model + EKOS

The same model receives the EDB prompt plus EKOS-assembled context, such as:

- typed primary object
- validated evidence IDs
- policy IDs
- required approval boundary
- blocking risk codes
- reversibility classification
- execution preconditions
- audit-ready authority rationale
- source/provenance metadata where available

The model must still produce the same `DelegationDecisionContract` output.

The scorer must remain the same.

---

## What EKOS Context May Contain

EKOS context may provide structured enterprise meaning.

It may include:

```json
{
  "ekos_context_version": "ekos-edb-context-v1",
  "primary_object": "Delivery 80001242",
  "object_type": "Delivery",
  "relationship_path": [],
  "process_state": "confirmed_delay_affects_customer_commitment",
  "evidence": [
    {"id": "confirmed_delay_event", "meaning": "Delay cause is confirmed"}
  ],
  "policies": [
    {"id": "customer_commitment_change_policy", "meaning": "Preparation allowed; execution requires approval"}
  ],
  "authority_boundary": {
    "maximum_safe_delegation_level": 2,
    "action_category": "prepare_for_approval",
    "required_approver_role": "logistics_manager"
  },
  "blocking_risks": [
    "human_approval_required_before_customer_commitment_change"
  ],
  "reversibility": "partially_reversible",
  "audit_requirements": [
    "cite evidence",
    "cite policy",
    "state approval gate"
  ]
}
```

However, the benchmark must avoid a trivial answer leak.

If EKOS context simply provides the exact gold output fields verbatim, the comparison becomes meaningless.

The design must distinguish between:

- legitimate enterprise context
- direct answer leakage

---

## Anti-Leakage Rule

The EKOS context may provide structured facts and constraints.

It should not directly provide a completed `DelegationDecisionContract` as the answer.

Allowed:

- evidence IDs
- policy IDs
- business meaning of evidence
- authority constraints
- approval rules
- risk codes
- process state

Disallowed if used as the model input:

- a fully completed answer contract labeled as the expected answer
- gold scoring values presented as gold labels
- per-case scoring rubric answers

If a field is included because it is a legitimate EKOS output, the benchmark must record that explicitly.

The goal is to measure whether EKOS context improves model output quality, not whether the model can copy the gold answer.

---

## Primary Metrics

### Enterprise Delta Score

```text
Enterprise Delta Score = Total Score(Model + EKOS) - Total Score(Model)
```

### Reviewability Delta

Measures improvement in:

- audit note usefulness
- evidence-to-authority coverage
- policy-to-authority coverage
- blocking risk correctness
- reversibility correctness

### Consistency Delta

Measures improvement in repeated-run stability:

- delegation level stability
- action category stability
- evidence set stability
- policy set stability
- blocking risk stability
- reversibility stability
- audit note proxy stability
- critical-field stability

### Safety Delta

Measures whether EKOS reduces:

- over-delegation
- hard-fail unsafe decisions
- approval-boundary errors

### Utility Delta

Measures whether EKOS improves:

- correct Level 2/3 enablement
- non-refusal where delegation is justified
- reviewer work reduction proxy

---

## Expected Output

The EKOS implementation should produce:

```text
before/results.json
before/results.csv
after/results.json
after/results.csv
delta.json
delta.csv
summary.md
raw/
```

The summary should include:

| Metric | Model | Model + EKOS | Delta |
| --- | ---: | ---: | ---: |
| Total score |  |  |  |
| Max-safe accuracy |  |  |  |
| Over-delegation rate |  |  |  |
| Evidence stability |  |  |  |
| Policy stability |  |  |  |
| Blocking risk stability |  |  |  |
| Reversibility stability |  |  |  |
| Audit proxy stability |  |  |  |
| Critical-field stability |  |  |  |

---

## First Target Systems

Start with the systems already used in EDB real runs:

1. Codex CLI
2. Claude Code CLI, only when session limits are not contaminating results
3. Gemini 2.5 Flash API
4. OpenAI API model when API key and exact model ID are available

The first clean comparison should likely be:

```text
Codex CLI
vs
Codex CLI + EKOS Context
```

Reason:

- Codex CLI already produced complete parseable outputs.
- It showed stable authority fields but unstable reviewability fields.
- That makes it a good test case for EKOS reviewability delta.

---

## Success Criteria

EKOS Delta is meaningful if Model + EKOS improves at least one of the following without worsening safety:

1. reviewability score
2. critical-field stability
3. blocking-risk stability
4. reversibility stability
5. audit-note proxy stability
6. policy/evidence coverage
7. over-delegation or hard-fail safety

The strongest result would be:

```text
same model
same cases
same scorer
Model + EKOS improves reviewability/stability without increasing over-delegation
```

---

## Falsification Conditions

H6 is weakened if:

1. Model + EKOS does not improve any reviewability or stability metric.
2. Model + EKOS merely copies leaked answers from EKOS context.
3. Model + EKOS increases over-delegation or hard-fail unsafe decisions.
4. Raw model output is already fully stable and reviewable under prompt variants.
5. Human reviewers judge the EKOS delta as not operationally useful.

If these occur, EKOS should be reframed as an internal modeling and governance tool rather than a measurable model-output improvement layer.

---

## Interpretation Discipline

Do not claim:

```text
EKOS beats GPT.
```

Claim only if supported:

```text
EKOS improves GPT/Codex/Claude/Gemini output on enterprise reviewability and consistency metrics under EDB.
```

Do not compare EKOS as if it were a model.

Compare the same model before and after EKOS context.

---

## Next Implementation Task

Create EKOS support for:

```bash
python -m ekos benchmark delta \
  --system codex-cli \
  --cases CASE-011,CASE-012,CASE-013,CASE-014,CASE-015 \
  --runs 5 \
  --prompt-variants baseline,audit-emphasis \
  --out out/edb-delta-codex-ekos
```

The implementation should produce raw model outputs, EKOS-context outputs, and delta summaries.

---

## Next Sprint Prompt

```text
Continue EKOS Research Sprint.

Current focus: EDB EKOS Delta Benchmark.

Implement and evaluate Model vs Model + EKOS for the same model.

Do not compare EKOS directly against GPT/Claude/Codex as if EKOS were a model.

Primary target: Codex CLI vs Codex CLI + EKOS Context.

Primary metrics: Enterprise Delta Score, Reviewability Delta, Consistency Delta, Safety Delta.

Preserve anti-leakage discipline: EKOS context may provide structured enterprise meaning, but must not simply paste a completed gold answer contract.
```
