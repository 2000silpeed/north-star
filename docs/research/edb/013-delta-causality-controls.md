# EDB Research Note 013 — Delta Causality Controls

**Date:** 2026-06-30

**Status:** Active research control

---

## Context

The Codex R5 EKOS Delta run strengthened the EDB direction:

```text
Measure Δ(Model + EKOS, Model)
```

rather than model-vs-model differences.

The run showed positive movement in score, reviewability, consistency proxies, and max-safe accuracy without increasing over-delegation.

However, the result does not yet prove that the improvement is uniquely caused by EKOS-specific enterprise meaning.

---

## Principal Reviewer Challenge

A reviewer can still ask:

> Would any well-structured JSON context have produced the same improvement?

This is the main remaining causality gap.

The next EDB work must separate:

```text
Availability effect
Prompt enrichment effect
Generic structured-context effect
EKOS-specific semantic effect
```

Until those are separated, the strongest supported claim remains:

> Same-model outputs improved when EKOS structured context was added under the tested EDB conditions.

The stronger claim is not yet proven:

> EKOS-specific enterprise semantics caused the improvement beyond generic structured context.

---

## Required Controls

## 1. Negative Control

Add a middle condition:

```text
A. Model only
B. Model + generic structured JSON context
C. Model + EKOS structured context
```

The negative-control context may include source facts, object IDs, timestamps, and simple key-value fields.

It must not include EKOS-specific relationship-path semantics, evidence-to-authority mapping, policy boundary interpretation, completed DelegationDecisionContract fields, or gold labels.

Interpretation:

- If B ties C, EKOS has not yet proven unique value.
- If C beats B on reviewability, consistency, and safety without leakage, the EKOS-specific claim becomes stronger.

EKOS implementation issue:

- `2000silpeed/ekos-sap-knowledge-os#8`

---

## 2. Ablation Study

Test whether each EKOS context component matters.

Candidate variants:

```text
full EKOS context
no evidence
no policy
no relationship paths
no process state
no source timestamp / freshness
no capability / authority boundary
```

Interpretation:

- If removing a component hurts performance, that component has empirical support.
- If removing a component does not hurt performance, that component is not yet proven necessary under current EDB cases.

EKOS implementation issue:

- `2000silpeed/ekos-sap-knowledge-os#9`

---

## 3. Provider API Delta

CLI-agent evidence is useful, but not clean model evidence.

Codex CLI includes product prompts, wrappers, sandbox behavior, policy surfaces, and output constraints.

A clean provider API delta runner should repeat:

```text
Model API only
Model API + EKOS context
```

and eventually:

```text
Model API only
Model API + generic structured context
Model API + EKOS context
```

EKOS implementation issue:

- `2000silpeed/ekos-sap-knowledge-os#10`

---

## 4. Availability-Aware Delta

The R5 run included three before-condition runner errors.

Future reports must always separate:

```text
all-pair delta
clean-pair delta excluding runner/parse-error pairs
availability/parse delta
```

This prevents accidental overclaiming when after-condition availability is better than before-condition availability.

This belongs under the parent validity-control issue:

- `2000silpeed/ekos-sap-knowledge-os#7`

---

## Research Priority

Priority order:

1. Negative control
2. Ablation study
3. Availability-aware reporting hardening
4. Provider API delta runner

Reason:

Negative control and ablation attack the main causal question directly.

Provider API is important, but it should not replace the causality controls.

---

## Claim Discipline

Allowed after current R5 evidence:

> Same Codex CLI plus EKOS context improved EDB outputs under the tested schema and scorer, without increasing unsafe over-delegation.

Not yet allowed:

> EKOS has proven unique causal value beyond generic structured context.

> EKOS improves enterprise ROI.

> EKOS is production-ready.

> EKOS is an authoritative policy or execution control system.

---

## Next Sprint Prompt

```text
Continue EKOS Research Sprint.

Current focus: EDB Delta causality controls.

Start with issue #8: implement negative-control structured context so the benchmark compares Model, Model + generic structured context, and Model + EKOS context.

Do not claim EKOS-specific causal value until generic structured context is tested.
Do not start ERB yet.
Keep safety gate first: unsafe over-delegation must not increase.
```
