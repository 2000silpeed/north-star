# ETCB Research Note 001 — Enterprise Task Cost Benchmark

**Date:** 2026-07-01

**Status:** Active research track

---

## Context

EDB has produced positive evidence that EKOS can improve delegation quality, reviewability, safety, and consistency under evaluated CLI-agent and provider API conditions.

However, enterprise adoption depends on more than quality.

The next business-critical question is:

```text
Does EKOS reduce the total cost required to complete an enterprise task?
```

This should not be reduced to single-prompt input token count.

EKOS may add structured context and therefore increase first-turn input tokens. That does not automatically mean EKOS is more expensive at the task level.

A more relevant enterprise metric is:

```text
successful task completion cost
```

---

## Research Decision

Start an Enterprise Task Cost Benchmark track.

Implementation issue:

```text
2000silpeed/ekos-sap-knowledge-os#13
```

Issue title:

```text
ETCB: measure EKOS impact on enterprise task completion cost
```

---

## Hypothesis H10 — EKOS Reduces Enterprise Task Completion Cost

EKOS may increase first-prompt input tokens, but reduce total task cost by improving first-pass success and reducing retries, hard failures, output verbosity, and human correction effort.

Expected pattern:

```text
Single-turn input tokens: EKOS may be higher
Total successful-task tokens: EKOS may be lower
Retry count: EKOS should be lower
First-pass success: EKOS should be higher
Human corrections: EKOS should be lower
Hard failures: EKOS should be lower
```

This is the short-term cost hypothesis.

---

## Hypothesis H11 — EKOS Enables Semantic Compression

Longer-term, EKOS may reduce prompt tokens through semantic references.

Instead of copying all enterprise context into the prompt, a model or agent could receive compact references such as:

```text
EKOS://SalesOrder/4711
EKOS://Delivery/80000123
EKOS://Policy/ApprovalBoundary/FreightDiversion
```

The model or agent can then resolve those references through tools, MCP, or a controlled EKOS retrieval layer.

Expected pattern:

```text
Prompt tokens decrease
Meaning is preserved
Evidence remains traceable
Tool/context retrieval supplies enterprise facts on demand
```

This is the long-term compression hypothesis.

---

## Why Token Count Alone Is Not Enough

A naive comparison may show:

```text
Model-only input tokens < EKOS input tokens
```

That is expected because EKOS adds structured context.

But an enterprise task often includes retries:

```text
raw prompt
→ incorrect or incomplete answer
→ user correction
→ follow-up prompt
→ revised answer
→ more correction
```

EKOS may be economically better if it produces:

```text
more correct first answer
fewer retries
fewer hard failures
shorter correction loop
higher reviewability per token
```

Therefore ETCB must measure task-level cost, not just first-turn prompt length.

---

## Core Metrics

ETCB should track at minimum:

```text
input tokens
output tokens
total tokens
retry count
first-pass success rate
task completion tokens
parse failures
runner failures
tool calls
wall-clock elapsed time
human correction count or proxy
score per 1k tokens
reviewability per 1k tokens
safe successful task completion rate
Enterprise Task Cost Score
```

If token metadata is unavailable, record it as:

```text
unknown
```

Do not record unavailable token data as zero.

---

## Cost Views

## 1. Single-Turn Token Cost

Question:

```text
How many input/output/total tokens does the first answer use?
```

Expected result:

```text
EKOS may lose on input tokens.
```

This is useful but insufficient.

## 2. Successful-Task Cost

Question:

```text
How many total tokens are required until the task reaches a correct, safe, reviewable answer?
```

Expected result:

```text
EKOS should win if it reduces retries or hard failures.
```

## 3. Quality-Normalized Cost

Question:

```text
How much quality is produced per token?
```

Candidate metrics:

```text
score / total tokens
reviewability / total tokens
safe-success / total tokens
hard-failure reduction / total tokens
```

## 4. Semantic Compression Cost

Question:

```text
Can EKOS preserve enterprise meaning with shorter semantic references instead of copied context?
```

Comparison:

```text
full EKOS context prompt
vs
EKOS semantic reference + tool retrieval
```

---

## Initial Experiment Plan

Do not create a broad new benchmark framework first.

Prefer a measurement layer over existing EDB provider outputs.

First experiment:

```text
Inspect existing provider-delta raw outputs for token metadata.
Compute before vs after token/cost metrics from the Provider API Delta R3 run.
```

Use existing evidence from:

```text
out/edb-provider-api-delta-anthropic-gemini-r3-2026-07-01/
```

If token metadata exists, compute:

```text
input tokens before/after
output tokens before/after
total tokens before/after
score per 1k tokens
reviewability per 1k tokens
hard failures per 1k tokens
safe successful task completion per 1k tokens
```

If token metadata is missing or partial, record coverage.

---

## Expected Result Before Running

My pre-experiment expectation:

```text
EKOS loses or is more expensive on first-turn input tokens.
EKOS may win on quality-normalized cost.
EKOS may win on successful-task cost if hard failures or retries are reduced.
```

The Provider API Delta R3 already showed hard-failure reduction for Gemini, so the cost hypothesis is plausible but not yet proven.

---

## Claim Discipline

Allowed after defining ETCB:

```text
EKOS cost impact is an open research question.
```

Allowed if preliminary data supports it:

```text
EKOS improves quality-normalized cost under evaluated provider API conditions.
```

Not allowed yet:

```text
EKOS saves money.
EKOS reduces token usage universally.
EKOS improves ROI.
EKOS is cheaper in production.
```

---

## Relationship to EDB and ERB

EDB answers:

```text
Does EKOS improve delegation quality, safety, reviewability, and consistency?
```

ETCB answers:

```text
Does EKOS reduce the cost of reaching a successful enterprise task outcome?
```

ERB should come later and answer:

```text
Does EKOS create business-level ROI?
```

ETCB is therefore a bridge between EDB and ERB.

---

## Next Sprint Prompt

```text
Continue EKOS Research Sprint.

Current focus: ETCB — Enterprise Task Cost Benchmark.

Issue: #13 — measure EKOS impact on enterprise task completion cost.

Do not build a broad new benchmark framework first.
Inspect the existing Provider API Delta R3 raw outputs for token metadata.
If token metadata exists, compute before/after token and quality-normalized cost metrics.
If token metadata is partial or missing, record coverage explicitly.
Separate single-turn token cost from successful-task cost.
Do not claim EKOS saves money until measured.
Record findings in North Star.
```
