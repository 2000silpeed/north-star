# Research Backlog — Enterprise ROI Benchmark (ERB)

**Date:** 2026-06-30

**Status:** Backlog — Do Not Start Yet

**Priority:** After EDB EKOS Delta Benchmark

---

## Purpose

Enterprise ROI Benchmark (ERB) is a future research track for testing whether improvements in enterprise reviewability, consistency, and governance translate into measurable business value.

This backlog item should not interrupt the current EDB work.

Current priority remains:

```text
EDB EKOS Delta Benchmark
```

ERB should start only after EDB has enough before/after evidence showing whether EKOS improves the same model's enterprise-grade outputs.

---

## Research Question

> Does improved enterprise reviewability and consistency translate into measurable business ROI?

In other words:

```text
Better reviewability / consistency
        ↓
Less review effort, fewer errors, lower risk, faster exception handling
        ↓
Measurable business value
```

---

## Why This Matters

EDB measures whether AI delegation decisions are safe, reviewable, and stable.

But enterprise buyers ultimately ask different questions:

- How much time does this save?
- How much risk does this reduce?
- How many errors does this prevent?
- How much faster can work be approved?
- How much audit effort is reduced?

ERB is the bridge from research quality to business value.

---

## Candidate Business KPIs

### Review and Approval KPIs

- average approval review time
- approval rework or rejection rate
- number of clarification cycles before approval
- time from AI recommendation to human approval
- reviewer confidence score

### Risk and Control KPIs

- policy violation count
- unsafe over-delegation count
- incorrect approval boundary count
- missing evidence or policy citation count
- audit finding count
- exception escalation accuracy

### Operations KPIs

- exception handling cycle time
- logistics delay classification time
- shipment exception resolution time
- IDoc reprocessing triage time
- master-data correction cycle time
- customer-impact review time

### Workforce KPIs

- new 담당자 onboarding time
- time to reach expert-like decision quality
- reviewer workload reduction
- handoff quality between teams

### Audit KPIs

- audit preparation time
- evidence collection time
- policy traceability completeness
- repeatability of decision records

---

## Candidate Experiment Design

### Condition A — Current Workflow

```text
AI or analyst output
        ↓
human review
        ↓
clarification / rework
        ↓
approval or escalation
```

### Condition B — EKOS-Assisted Workflow

```text
AI + EKOS reviewable output
        ↓
human review
        ↓
approval or escalation
```

Measure the delta:

```text
ROI Delta = KPI(Condition B) - KPI(Condition A)
```

Examples:

- review time reduction
- fewer rework cycles
- fewer missing evidence cases
- fewer policy-boundary mistakes
- faster exception closure

---

## Possible First Domain

The first ERB pilot should likely use SAP logistics exception handling because EKOS and EDB already have synthetic logistics-style cases.

Candidate workflows:

1. delivery delay classification
2. customer commitment change approval packet
3. shipment exception status update review
4. master-data housekeeping approval
5. IDoc failure triage

---

## Success Criteria

ERB becomes valuable if it can show measurable improvement in at least one business KPI without increasing operational risk.

Possible first-pass success threshold:

```text
review time reduced by 30%+
AND
no increase in unsafe approval or policy-boundary errors
```

Longer-term success threshold:

```text
review time reduced
policy violations reduced
rework cycles reduced
audit completeness improved
```

---

## Required Prerequisites

Do not start ERB until the following are substantially complete:

1. EDB deterministic benchmark
2. EDB real-model comparison
3. EDB consistency stress testing
4. EKOS Delta Benchmark
5. enough before/after evidence to justify business KPI testing
6. access to realistic workflow data or domain reviewer simulation

---

## Deferral Reason

ERB is important, but starting it now would distract from the current research path.

The current open question is still:

```text
Does EKOS improve the same model's reviewability and consistency under EDB?
```

Only after that is answered should we ask:

```text
Does that improvement save time, reduce risk, or create measurable ROI?
```

---

## Interpretation Discipline

Do not claim business ROI from EDB alone.

EDB can support:

```text
EKOS improves reviewability / consistency metrics.
```

ERB is required before claiming:

```text
EKOS reduces enterprise operating cost or risk in real workflows.
```

---

## Backlog Status

```text
Backlog only.
Do not start before EDB EKOS Delta Benchmark.
```
