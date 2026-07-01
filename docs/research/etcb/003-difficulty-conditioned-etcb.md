# ETCB Research Note 003 — Difficulty-Conditioned ETCB

**Date:** 2026-07-01

**Status:** Active research track

---

## Context

ETCB began by asking whether EKOS reduces enterprise task completion cost.

The first token-cost measurement was negative for first-turn efficiency:

```text
EKOS improved quality and hard failures, but used substantially more tokens.
```

The workflow repair-cost follow-up added a more nuanced signal:

```text
Hard-failed subset only:
model-only first answer + one repair = 72,107 tokens, 0/9 safe success
EKOS first answer = 35,827 tokens, 9/9 safe success

Full 60-record set:
model-only + one repair = 181,640 tokens, 51/60 safe success
EKOS first answer = 222,466 tokens, 60/60 safe success
```

This means EKOS is not yet proven cheaper overall, but the failed subset shows
that simple repair did not cheaply recover hard failures.

The research question therefore changes again.

---

## Research Decision

Start a difficulty-conditioned ETCB track.

Implementation issue:

```text
2000silpeed/ekos-sap-knowledge-os#14
```

Issue title:

```text
Difficulty-Conditioned ETCB: identify when EKOS becomes economically preferable
```

---

## Hypothesis H10c — EKOS Is Economically Justified for Difficult Tasks

EKOS is economically justified primarily for difficult enterprise tasks that
otherwise require repair or fail.

Expected pattern:

```text
Easy tasks: model-only may be cheaper
Medium tasks: mixed / threshold-dependent
Hard or failure-prone tasks: EKOS may become cheaper per safe success
```

This hypothesis explains why aggregate ETCB can be negative while the
hard-failed subset strongly favors EKOS.

---

## Research Question

The question is no longer:

```text
Is EKOS always cheaper?
```

The correct question is:

```text
At what task difficulty does EKOS become economically preferable?
```

This fits the enterprise framing better. EKOS should not be positioned as a
cheap wrapper for trivial prompts. Its strongest value may be difficult,
policy-heavy, authority-sensitive, reviewability-sensitive, or failure-prone
decisions.

---

## Difficulty Proxies

Use existing EDB/ETCB artifacts first.

Candidate difficulty proxies:

```text
model-only hard failure
low model-only score
low model-only reviewability
incorrect max-safe decision
inconsistency across runs
repair required
policy/authority ambiguity
conflicting evidence
```

Initial buckets:

```text
easy
medium
hard
failure-prone
```

Bucket definitions must be explicit and reproducible.

---

## Required Metrics by Bucket

For each difficulty bucket, compare:

```text
model-only first answer
model-only first answer + repair if applicable
EKOS first answer
```

Report:

```text
total tokens
safe successes
tokens per safe success
score per 1k tokens
reviewability per 1k tokens
hard failures per 1k tokens
repair success rate
interpretation by difficulty bucket
```

---

## Expected Outcome

The expected result is not that EKOS wins everywhere.

The expected result is a threshold curve:

```text
easy tasks        -> model-only cheaper
medium tasks      -> mixed
hard tasks        -> EKOS competitive
failure-prone     -> EKOS may dominate
```

If confirmed, the EKOS value proposition becomes sharper:

```text
EKOS is not for every prompt.
EKOS is for enterprise tasks where failure, repair, audit, or authority mistakes
are expensive.
```

---

## Relationship to H11 Semantic Compression

H11 remains important, but H10c should come first because it can be tested with
existing artifacts.

If H10c confirms that EKOS is only economically preferable for hard tasks, then
semantic compression should target those tasks first.

Do not compress EKOS blindly. Compress the cases where EKOS is valuable but
token-heavy.

---

## Claim Discipline

Allowed after defining this track:

```text
EKOS may be economically justified only for difficult or failure-prone enterprise
tasks; this remains unproven until bucketed ETCB analysis is completed.
```

Not allowed:

```text
EKOS is cheaper overall.
EKOS saves money.
EKOS has ROI.
EKOS should be used for every enterprise prompt.
```

---

## Next Sprint Prompt

```text
Continue EKOS Research Sprint.

Current focus: Difficulty-Conditioned ETCB.

Issue: #14 — identify when EKOS becomes economically preferable.

Do not create a broad new benchmark family.
Use existing Provider API R3, ETCB token-cost, and repair-cost artifacts first.
Define reproducible difficulty buckets using model-only score, hard failure,
reviewability, max-safe correctness, inconsistency, and repair requirement.
Compute ETCB metrics by bucket.
State where EKOS is cheaper, more expensive, or inconclusive.
Do not claim ROI.
Record results in North Star.
```
