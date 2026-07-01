# EDB Research Note 015 — Provider API Delta R3

**Date:** 2026-07-01

**Status:** Preliminary provider API evidence

---

## Context

The CLI-agent results strengthened the EKOS delta hypothesis, but they still
contained a major confound:

```text
CLI product wrapper behavior
```

Codex CLI, Claude Code CLI, and similar tools may add product prompts, local
policies, output handling, sandbox assumptions, or hidden behavior that is not
the underlying model API.

Issue #10 implemented the Provider API Delta Runner to test:

```text
Provider API model
vs
Provider API model + EKOS context
```

This run collected the first full R3 provider API evidence after excluding
providers that could not complete reliable live calls.

---

## Provider Availability

Included:

```text
Anthropic: claude-sonnet-4-6
Gemini: gemini-2.5-flash
```

Excluded:

```text
OpenAI: API key returned insufficient_quota during smoke checks.
Z.ai / GLM: both Z_AI_API_KEY and ZHIPU_API_KEY returned 401 Authentication Failed.
```

Z.ai was checked against the GLM Coding Plan quick-start contract:

```text
OpenAI-compatible base URL: https://api.z.ai/api/coding/paas/v4
Anthropic-compatible base URL: https://api.z.ai/api/anthropic
Models checked: glm-4.7, glm-5.2, glm-5.2[1m]
```

Both stored keys failed authentication under those combinations. Therefore Z.ai
was excluded from this evidence run rather than counted as model behavior.

---

## Method

Benchmark command:

```bash
python -m ekos benchmark provider-delta \
  --models models/live-models.local.json \
  --cases CASE-011,CASE-012,CASE-013,CASE-014,CASE-015 \
  --temperatures 0.0 \
  --prompt-variants baseline,audit-emphasis \
  --runs 3 \
  --out out/edb-provider-api-delta-anthropic-gemini-r3-2026-07-01
```

Consistency reports:

```bash
python -m ekos benchmark consistency \
  --input out/edb-provider-api-delta-anthropic-gemini-r3-2026-07-01/before/results.json \
  --out out/edb-provider-api-delta-anthropic-gemini-r3-2026-07-01/_before_consistency

python -m ekos benchmark consistency \
  --input out/edb-provider-api-delta-anthropic-gemini-r3-2026-07-01/after/results.json \
  --out out/edb-provider-api-delta-anthropic-gemini-r3-2026-07-01/_after_consistency
```

Configuration:

```text
Cases: CASE-011 through CASE-015
Prompt variants: baseline, audit-emphasis
Runs: 3
Temperature: 0.0
Conditions: Model API only vs Model API + EKOS context
Scorer: existing EDB delegation scorer
Schema: existing DelegationDecisionContract
```

Local artifact path:

```text
out/edb-provider-api-delta-anthropic-gemini-r3-2026-07-01/
```

---

## Overall Result

Pair alignment:

```text
60/60 aligned provider API pairs
```

| Metric | Model API | Model API + EKOS | Delta |
| --- | ---: | ---: | ---: |
| Total score | 18.483 | 19.750 | +1.267 |
| Max-safe accuracy | 1.000 | 1.000 | +0.000 |
| Over-delegation rate | 0.000 | 0.000 | +0.000 |
| Hard-fail rate | 0.150 | 0.000 | -0.150 |
| Parse failure rate | 0.000 | 0.000 | +0.000 |
| Runner error rate | 0.000 | 0.000 | +0.000 |
| Critical-field stability | 0.967 | 0.978 | +0.011 |
| Enterprise delta score | - | - | +0.046 |

Component deltas:

| Component | Delta |
| --- | ---: |
| Reviewability | +0.106 |
| Consistency | +0.011 |
| Safety | +0.050 |
| Utility | +0.000 |

---

## Per-Provider Result

| System | Records | Score | Score delta | Reviewability delta | Consistency delta | Safety delta | Enterprise delta | Over-delegation |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Anthropic `claude-sonnet-4-6` | 30 | 19.133 -> 19.733 | +0.600 | +0.050 | +0.000 | +0.000 | +0.016 | 0.000 -> 0.000 |
| Gemini `gemini-2.5-flash` | 30 | 17.833 -> 19.767 | +1.933 | +0.161 | +0.022 | +0.100 | +0.076 | 0.000 -> 0.000 |

Availability and parsing:

| System | Parse failures | Runner errors |
| --- | ---: | ---: |
| Anthropic `claude-sonnet-4-6` | 0 | 0 |
| Gemini `gemini-2.5-flash` | 0 | 0 |

Hard failures:

| System | Model API | Model API + EKOS |
| --- | ---: | ---: |
| Anthropic `claude-sonnet-4-6` | 0/30 | 0/30 |
| Gemini `gemini-2.5-flash` | 9/30 | 0/30 |

---

## Consistency Result

Before consistency:

```text
Records: 60
Groups: 20
Parse failure rate: 0.00
Runner error rate: 0.00
Over-delegation rate: 0.00
Mean critical-field stability: 0.97
Fully stable group rate: 0.75
```

After consistency:

```text
Records: 60
Groups: 20
Parse failure rate: 0.00
Runner error rate: 0.00
Over-delegation rate: 0.00
Mean critical-field stability: 0.98
Fully stable group rate: 0.80
```

Per-system consistency:

| System | Before critical stability | After critical stability | Before fully stable groups | After fully stable groups |
| --- | ---: | ---: | ---: | ---: |
| Anthropic `claude-sonnet-4-6` | 0.99 | 0.99 | 0.90 | 0.90 |
| Gemini `gemini-2.5-flash` | 0.94 | 0.97 | 0.60 | 0.70 |

---

## Interpretation

This is the first provider API R3 evidence that removes CLI-agent product
wrappers from the tested path.

The result supports this bounded claim:

> EKOS improved the evaluated provider API models under identical EDB R3
> conditions without increasing unsafe over-delegation.

It also supports:

> EKOS improved reviewability and reduced hard failures in the evaluated
> provider API run.

The result is especially visible for Gemini 2.5 Flash, where hard failures
dropped from 9/30 to 0/30 and total score increased by +1.933 points. Anthropic
also improved, but more modestly, with a +0.600 score delta and no hard failures
in either condition.

---

## Claim Discipline

Allowed:

```text
EKOS improved the evaluated provider API models, Anthropic claude-sonnet-4-6
and Gemini gemini-2.5-flash, under the tested EDB R3 conditions.
```

Allowed:

```text
The evaluated provider API run showed improved reviewability without increased
unsafe over-delegation.
```

Still not allowed:

```text
Universal model superiority
Provider-independent proof across all providers
Enterprise ROI
Production readiness
```

Reason:

Only two provider API models completed the R3 run. OpenAI was quota-blocked and
Z.ai was authentication-blocked. The provider API evidence is stronger than CLI
evidence against the product-wrapper confound, but it is not universal provider
coverage.

---

## Open Questions

1. Would OpenAI show the same direction after quota is restored?
2. Would Z.ai / GLM show the same direction after a valid GLM Coding Plan key
   is provided?
3. Is Gemini's hard-failure reduction stable at R5?
4. Would provider-side generic structured-context controls preserve the
   EKOS-specific advantage seen in CLI-agent negative-control robustness?

---

## Minimum Next Experiment

Do not add a new benchmark family.

The minimum useful next experiment is:

```text
Run provider-side negative-control robustness for Anthropic and Gemini only if
the existing implementation already supports it, or repeat this provider API
delta at R5 if the immediate goal is replication rather than new controls.
```

If OpenAI quota or a valid Z.ai Coding Plan key becomes available, the cleaner
next step is to rerun the same Provider API Delta R3 with those providers added,
without changing the benchmark contract.
