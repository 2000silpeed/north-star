# EDB Research Note 016 — EKOS Failure Boundary Analysis

**Date:** 2026-07-01

**Status:** Active boundary-condition research track

---

## Context

EDB now has multiple positive evidence streams:

```text
Codex CLI delta
Claude Code CLI replication
negative-control R5
negative-control robustness across generic structured-context baselines
Provider API Delta R3 over Anthropic and Gemini
```

This changes the research question.

The question is no longer only:

```text
Does EKOS help?
```

The next stronger question is:

```text
When does EKOS not help?
```

This is necessary for scientific scope control. A serious enterprise AI claim should define its boundary conditions, not only collect positive cases.

---

## Research Decision

Start a failure-boundary / boundary-condition track in EKOS.

Implementation issue:

```text
2000silpeed/ekos-sap-knowledge-os#12
```

Issue title:

```text
EDB Failure Boundary Analysis: identify where EKOS does not help
```

---

## Principal Reviewer Challenge

After the positive CLI-agent and provider API evidence, a reviewer will likely ask:

```text
Where does EKOS fail?
When is EKOS unnecessary?
When does EKOS add overhead without improving delegation quality?
```

This is a stronger and healthier question than adding more favorable examples.

---

## Boundary Hypotheses

## H1 — Already-Sufficient Prompt

If the raw prompt already contains all relevant evidence, policy, approval boundary, reversibility, and blocking risks, then EKOS delta should be small.

Expected outcome:

```text
Score delta near zero
Reviewability delta small
Safety unchanged
```

Interpretation:

```text
EKOS is most valuable when the raw prompt lacks enterprise structure.
```

## H2 — No Meaningful EKOS Context

If EKOS context adds little or no additional enterprise meaning, EKOS should not appear to help.

Expected outcome:

```text
Score delta near zero
No meaningful reviewability gain
```

Interpretation:

```text
EKOS should not be credited for gains when it contributes no meaningful evidence or structure.
```

## H3 — Policy-Free Delegation

If the case has no real policy ambiguity or approval-boundary ambiguity, EKOS policy context should matter less.

Expected outcome:

```text
Policy-related delta small
Safety unchanged
```

Interpretation:

```text
EKOS policy/value is conditional on policy or authority ambiguity.
```

## H4 — Low-Stakes Reversible Action

If the correct decision is obviously low-risk and reversible, EKOS may add audit clarity but should not produce a large score or safety delta.

Expected outcome:

```text
Utility unchanged
Safety unchanged
Reviewability may improve slightly
```

Interpretation:

```text
EKOS may be useful for auditability even when decision difficulty is low.
```

## H5 — Irrelevant Context / Context Overload

If EKOS context includes irrelevant enterprise facts, the model may become noisier or slower.

Expected outcome:

```text
Score delta shrinks or becomes negative
Reviewability may degrade if evidence selection worsens
```

Interpretation:

```text
EKOS context must be selective. More context is not automatically better.
```

## H6 — Conflicting Evidence

If source facts disagree, or policy and operational evidence point in different directions, EKOS should help only if it clarifies authority and evidence precedence.

Expected outcome:

```text
EKOS helps only when authority ordering is explicit
Generic context may not be enough
```

Interpretation:

```text
The strongest EKOS value may be evidence precedence and authority resolution, not context volume.
```

---

## Required Evidence Shape

The boundary track should not create a new benchmark family unless strictly necessary.

Prefer:

```text
small case additions
case variants
existing runners
```

Reusable runners:

```text
benchmark delta
benchmark negative-control
benchmark negative-control-robustness
benchmark provider-delta
```

Each boundary case must include:

```text
case ID
boundary type
expected outcome
falsification condition
metrics to inspect
```

---

## Success Criteria

This track succeeds if it identifies at least one of the following:

```text
EKOS strong zone
EKOS weak zone
EKOS unnecessary zone
EKOS harmful/noisy zone
```

A weak or negative EKOS result is valuable if it clarifies scope.

---

## Claim Discipline

Allowed:

```text
EKOS appears strongest when enterprise evidence, policy, authority, reversibility, or reviewability are under-specified in the raw prompt.
```

Allowed if supported by boundary cases:

```text
EKOS has limited marginal value when the raw prompt already contains sufficient enterprise structure.
```

Not allowed:

```text
EKOS always helps.
EKOS should always add more context.
EKOS is production-ready.
EKOS proves enterprise ROI.
```

---

## Next Sprint Prompt

```text
Continue EKOS Research Sprint.

Current focus: EDB Failure Boundary Analysis.

Issue: #12 — EDB Failure Boundary Analysis: identify where EKOS does not help.

Do not create a new benchmark family unless strictly necessary.
Reuse existing EDB runners.
Define at least 3 boundary cases or variants with falsifiable expected outcomes.
Prefer cases where EKOS should produce near-zero or negative delta.
Do not optimize prompts to force EKOS to win.
Treat weak or negative EKOS results as useful scope evidence.
Record results in North Star.
```
