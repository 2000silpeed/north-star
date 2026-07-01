# ETCB Research Note 001 — Enterprise Task Cost Benchmark

**Date:** 2026-07-01

**Status:** Preliminary token-cost evidence after first measurement

---

## Context

EDB has produced positive evidence that EKOS can improve delegation quality,
reviewability, safety, and consistency under evaluated CLI-agent and provider
API conditions.

However, enterprise adoption depends on more than quality.

The next business-critical question is:

```text
Does EKOS reduce the total cost required to complete an enterprise task?
```

This should not be reduced to single-prompt input token count.

EKOS may add structured context and therefore increase first-turn input tokens.
That does not automatically mean EKOS is more expensive at the task level. A
more relevant enterprise metric is:

```text
successful task completion cost
```

Issue #13 opened ETCB, the Enterprise Task Cost Benchmark, to test whether EKOS
is cost-efficient, not only higher quality.

---

## Research Decision

Start ETCB from existing evidence instead of creating a broad new benchmark
framework.

Implementation issue:

```text
2000silpeed/ekos-sap-knowledge-os#13
```

Issue title:

```text
ETCB: measure EKOS impact on enterprise task completion cost
```

The first experiment inspects the existing Provider API Delta R3 artifact:

```text
out/edb-provider-api-delta-anthropic-gemini-r3-2026-07-01/
```

The ETCB preliminary artifact was generated in the EKOS implementation
repository:

```text
out/etcb-provider-api-delta-anthropic-gemini-r3-2026-07-01/
```

Generated files:

```text
summary.md
token_cost_delta.json
token_cost_delta.csv
token_cost_delta_by_group.csv
```

---

## Original Hypothesis H10 — EKOS Reduces Enterprise Task Completion Cost

Original H10 assumed:

```text
EKOS may increase first-prompt input tokens, but reduce total task cost by
improving first-pass success and reducing retries, hard failures, output
verbosity, and human correction effort.
```

Expected pattern before measurement:

```text
Single-turn input tokens: EKOS may be higher
Total successful-task tokens: EKOS should be lower
Retry count: EKOS should be lower
First-pass success: EKOS should be higher
Human corrections: EKOS should be lower
Hard failures: EKOS should be lower
```

This was the short-term cost hypothesis.

---

## Research Correction After First Measurement

The first ETCB preliminary measurement did **not** support the strong version of
H10.

Observed result:

```text
EDB quality improved.
Hard failures decreased.
Reviewability improved.
First-turn token usage increased significantly.
Quality-normalized token efficiency decreased.
```

Interpretation:

```text
Current EKOS improves quality, safety, and reviewability, but does not yet
improve first-turn token efficiency.
```

This is not treated as a project failure. It is treated as research evidence
that corrects the hypothesis.

The ETCB objective therefore changes from:

```text
Does EKOS reduce task cost?
```

to:

```text
Under what conditions can EKOS improve quality while also reducing enterprise
task cost?
```

The preliminary ETCB evidence falsified the initial expectation that EKOS would
immediately reduce task cost under first-turn token accounting. This redirects
the research toward semantic compression and workflow-level repair-cost
measurement rather than larger prompt augmentation.

---

## Revised Hypothesis H10a — Current EKOS Cost Profile

**Status:** Supported by preliminary ETCB evidence.

Current EKOS increases first-turn token consumption while improving quality.

Supported by this measurement:

```text
EKOS increased first-turn input tokens.
EKOS increased mean score.
EKOS increased mean reviewability.
EKOS reduced hard failures.
EKOS did not improve score per 1k total tokens.
EKOS did not improve reviewability per 1k total tokens.
```

H10a replaces the earlier assumption that EKOS would immediately reduce task
cost in the first measured provider API run.

---

## Revised Hypothesis H10b — Workflow-Level Cost Remains Open

**Status:** Open.

The first ETCB measurement was a single-turn provider API token analysis. It did
not measure downstream repair turns, human review time, workflow handoff cost,
or the operational cost of hard failures.

H10b asks:

```text
Can EKOS reduce total enterprise task completion cost once retries, hard-failure
repair, human correction, and elapsed workflow time are included?
```

This remains unmeasured.

A future H10b experiment should compare:

```text
model-only failed or incomplete answer
-> repair / retry / human correction path

vs

EKOS answer
-> review / approval path
```

The cost unit should include tokens, time, and correction burden, not tokens
alone.

---

## Hypothesis H11 — EKOS Enables Semantic Compression

**Status:** Open and now elevated in priority.

The current ETCB result suggests semantic compression has become a primary
research objective.

Longer-term, EKOS may reduce prompt tokens through semantic references.

Instead of copying all enterprise context into the prompt, a model or agent
could receive compact references such as:

```text
EKOS://SalesOrder/4711
EKOS://Delivery/80000123
EKOS://Policy/ApprovalBoundary/FreightDiversion
```

The model or agent can then resolve those references through tools, MCP, or a
controlled EKOS retrieval layer.

Expected pattern:

```text
Prompt tokens decrease
Meaning is preserved
Evidence remains traceable
Tool/context retrieval supplies enterprise facts on demand
```

This is the long-term compression hypothesis. It was not tested in the first
ETCB run.

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
-> incorrect or incomplete answer
-> user correction
-> follow-up prompt
-> revised answer
-> more correction
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

## Cost Views

ETCB should separate at least four views:

| View | Question |
| --- | --- |
| Single-turn token cost | How many input/output/total tokens does the first answer use? |
| Successful-task cost | How many total tokens are required until the task reaches a correct, safe, reviewable answer? |
| Quality-normalized cost | How much quality is produced per token? |
| Semantic compression cost | Can EKOS preserve enterprise meaning with shorter semantic references instead of copied context? |

This first measurement covers the first three views. Semantic compression was
not tested.

If token metadata is unavailable, record it as:

```text
unknown
```

Do not record unavailable token data as zero.

---

## Method

This is a post-hoc analysis over existing Provider API Delta R3 records.

No new benchmark family was added. No prompt was optimized. No EKOS context was
compressed to make the after condition cheaper.

The comparison remains:

```text
Model API only
vs
Model API + EKOS context
```

The evaluated models are the same provider API models from EDB Research Note
015:

```text
Anthropic: claude-sonnet-4-6
Gemini: gemini-2.5-flash
```

Definitions:

| Term | Definition |
| --- | --- |
| Single-turn token cost | Provider-reported input, output, and total tokens for one benchmark call. |
| Quality-normalized cost | Score and reviewability points per 1k total tokens, plus tokens per score/reviewability point. |
| Successful-task cost | Safe successful completions per 1k total tokens, hard failures per 1k total tokens, and tokens per safe success. |
| Safe success | Parsed output with no runner error, no parse error, no hard fail, no over-delegation, and exact maximum-safe delegation accuracy. |

Missing token metadata was not treated as zero.

---

## Token Metadata Coverage

Provider usage metadata exists for all 120 records.

| Provider | Condition | Records | Input | Output | Total | Cached | Notes |
| --- | --- | ---: | ---: | ---: | ---: | ---: | --- |
| Anthropic | before | 30 | 30 | 30 | 30 | 30 | `total_tokens` derived as `input_tokens + output_tokens`. |
| Anthropic | after | 30 | 30 | 30 | 30 | 30 | Cache fields exist; values were zero in this run. |
| Gemini | before | 30 | 30 | 30 | 30 | 12 | `totalTokenCount` reported; cached metadata partial. |
| Gemini | after | 30 | 30 | 30 | 30 | 11 | `thoughtsTokenCount` present in all Gemini records. |

Provider-specific fields observed:

| Provider | Usage fields |
| --- | --- |
| Anthropic | `input_tokens`, `output_tokens`, `cache_creation_input_tokens`, `cache_read_input_tokens`, `cache_creation`, `service_tier`, `inference_geo` |
| Gemini | `promptTokenCount`, `candidatesTokenCount`, `totalTokenCount`, `thoughtsTokenCount`, `promptTokensDetails`, `serviceTier`, sometimes `cachedContentTokenCount`, `cacheTokensDetails` |

Important comparability limit:

Gemini `totalTokenCount` includes provider-reported thinking tokens when
present. Anthropic total token count was derived from input and output fields.
This means provider-to-provider totals are not perfectly comparable. The most
reliable comparison is before vs after within each provider.

---

## Overall Result

| Metric | Model API | Model API + EKOS | Delta |
| --- | ---: | ---: | ---: |
| Mean input tokens | 1,389.550 | 2,893.850 | +1,504.300 |
| Mean output tokens | 346.450 | 375.667 | +29.217 |
| Mean total tokens | 2,210.517 | 3,707.767 | +1,497.250 |
| Total tokens | 132,631 | 222,466 | +89,835 |
| Mean score | 18.483 | 19.750 | +1.267 |
| Mean reviewability | 10.483 | 11.750 | +1.267 |
| Hard failures | 9 | 0 | -9 |
| Safe successes | 51 | 60 | +9 |

Quality-normalized token efficiency:

| Metric | Model API | Model API + EKOS | Delta |
| --- | ---: | ---: | ---: |
| Score per 1k total tokens | 8.362 | 5.327 | -3.035 |
| Reviewability per 1k total tokens | 4.742 | 3.169 | -1.573 |
| Tokens per score point | 119.595 | 187.735 | +68.140 |
| Tokens per reviewability point | 210.860 | 315.555 | +104.695 |

Successful-task token efficiency:

| Metric | Model API | Model API + EKOS | Delta |
| --- | ---: | ---: | ---: |
| Hard failures per 1k total tokens | 0.068 | 0.000 | -0.068 |
| Safe successes per 1k total tokens | 0.385 | 0.270 | -0.115 |
| Tokens per safe success | 2,600.608 | 3,707.767 | +1,107.159 |

---

## Per-Provider Result

| Provider | Mean total tokens delta | Score/1k delta | Reviewability/1k delta | Hard failures/1k delta | Safe successes/1k delta |
| --- | ---: | ---: | ---: | ---: | ---: |
| Anthropic `claude-sonnet-4-6` | +1,559.233 | -4.733 | -2.679 | +0.000 | -0.257 |
| Gemini `gemini-2.5-flash` | +1,435.267 | -1.938 | -0.855 | -0.115 | -0.021 |
| Overall | +1,497.250 | -3.035 | -1.573 | -0.068 | -0.115 |

Anthropic improved score modestly, but had no hard failures in either condition.
The EKOS after condition therefore added token cost without creating a measured
safe-success advantage for Anthropic in this run.

Gemini is more nuanced. EKOS removed 9 hard failures and improved score, which
supports the EDB quality and safety result. However, the strict single-turn
token-normalized ETCB view is still negative because EKOS context increased
total tokens more than the safe-success count increased.

---

## Interpretation

ETCB preliminary interpretation label:

```text
negative
```

This result should be recorded plainly:

```text
The evaluated Provider API R3 evidence supports EKOS quality and safety
improvement, but it does not support EKOS token-cost efficiency in a first-turn
ETCB measurement.
```

The reason is mechanical and important:

1. EKOS after calls include additional structured context.
2. That context increases input tokens substantially.
3. The quality gain is real but not large enough to offset the token increase
   when measured as score per 1k total tokens.
4. Hard failures improve, especially for Gemini, but safe successes per 1k
   total tokens still decline overall.

This does not falsify the EDB result. It falsifies a stronger cost-efficiency
claim under the current measurement:

```text
EKOS improves evaluated provider API output quality.
EKOS did not improve first-turn token efficiency in this ETCB preliminary run.
```

---

## ETCB Roadmap After First Measurement

Stage 1 — Current first-turn token-cost measurement

```text
Status: completed
Result: negative for first-turn token efficiency
```

Stage 2 — Quality-normalized cost measurement

```text
Status: completed preliminarily
Result: negative in the first Provider API R3 analysis
```

Stage 3 — Workflow-level repair-cost measurement

```text
Status: planned
Goal: measure whether hard-failure reduction reduces retries, repair tokens,
human correction, or elapsed time.
```

Stage 4 — Semantic compression prototype

```text
Status: planned
Goal: reduce prompt tokens while preserving EKOS meaning, traceability, and
authority boundaries.
```

Stage 5 — Enterprise ROI Bridge

```text
Status: future work
Goal: connect ETCB evidence to ERB only after task-level cost is measured.
```

---

## Rejected Interpretations

Rejected:

```text
EKOS saves money.
```

Reason:

No provider pricing table was applied, token totals are not directly comparable
across providers, and the measured first-turn token efficiency is negative.

Rejected:

```text
EKOS has proven enterprise ROI.
```

Reason:

ROI would require workflow-level cost, human review time, retry cost, production
failure cost, and adoption cost. None of those were measured.

Rejected:

```text
The token-cost result invalidates EKOS.
```

Reason:

The same run still shows improved score, reviewability, and hard-failure
reduction. The accurate conclusion is narrower: EKOS is not yet shown to be
cost-efficient under first-turn token accounting.

---

## Claim Discipline Update

A higher first-turn token count is not, by itself, evidence against EKOS.

Enterprise task cost must be evaluated across the entire task lifecycle,
including:

```text
retries
hard failures
repair prompts
human correction effort
elapsed time
successful task completion
```

Allowed:

```text
In the evaluated Provider API R3 run, EKOS improved quality and reduced hard
failures, but increased first-turn token cost.
```

Allowed:

```text
The preliminary ETCB result is negative for first-turn token efficiency.
```

Allowed:

```text
Further ETCB evidence is needed before claiming EKOS reduces enterprise task
completion cost.
```

Not allowed:

```text
EKOS saves money.
EKOS proves ROI.
EKOS is production-cost efficient.
EKOS is universally more efficient than model-only prompting.
```

---

## Minimum Next Experiment

Do not add a new benchmark family.

The minimum useful next experiment is not another favorable quality benchmark.
It is one of two targeted measurements:

1. Workflow-level repair-cost measurement for hard-failed model-only records.
2. Semantic compression prototype measurement using compact EKOS references.

If replication is needed first:

```text
Repeat the same Provider API Delta run at R5, then rerun the same post-hoc ETCB
token-cost analysis.
```

If the R5 result stays directionally similar, the next research decision should
not be another benchmark expansion. It should be whether EKOS needs semantic
compression before cost-efficiency claims are attempted.

If H10b needs to be tested more directly, the minimum additional evidence should
measure task repair cost for the same hard-failed records, using the existing
runner contract and unchanged prompts. That should be treated as a separate
workflow-cost measurement, not as proof of ROI.

---

## Future AI Context

Future sessions should not smooth this result into a positive ETCB claim.

The correct state after Issue #13 is:

```text
EDB quality evidence: positive for evaluated models.
ETCB first-turn token efficiency: negative in the preliminary Provider API R3
analysis.
H10 original: corrected.
H10a: supported — current EKOS improves quality but increases first-turn token cost.
H10b after this note: open — workflow-level repair-cost savings are unmeasured.
H11: open — semantic compression is untested and now higher priority.
```

The research opportunity is now clear:

EKOS has evidence that it can improve enterprise delegation quality. It now has
pressure to become more compact, or to prove that its extra upfront context
reduces downstream repair and review cost enough to justify the token increase.

---

## Follow-Up Measurement

The minimum repair-cost follow-up described above was executed and recorded in:

```text
docs/research/etcb/002-workflow-repair-cost.md
```

That result does not reverse the first-turn token-cost finding. It shows that a
single repair turn failed to recover the 9 Gemini hard failures, while EKOS
first answers produced 9/9 safe successes for that failed subset. The result is
preliminary H10b evidence only and does not test H11 semantic compression. The
updated state after that follow-up is recorded in ETCB Research Note 002.
