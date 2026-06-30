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

Implementation result:

```text
Status: implemented in EKOS
Command: python -m ekos benchmark negative-control
```

The EKOS implementation now supports a three-condition benchmark:

```text
A. before  = model-only EDB prompt
B. control = model + generic structured source-note context
C. after   = model + EKOS structured context
```

The generic control intentionally stays below EKOS semantics. It can contain
object references, source records, source snippets, simple counts, and generic
case facts. It must not contain EKOS authority constraints, evidence-to-authority
mapping, policy-boundary interpretation, blocking-risk interpretation,
reversibility classification, completed DelegationDecisionContract fields, or
gold labels.

The runner exports:

```text
before/results.json
before/results.csv
control/results.json
control/results.csv
after/results.json
after/results.csv
negative_control_delta.json
negative_control_delta.csv
summary.md
raw/
```

The report explicitly separates:

```text
Model -> Generic Context delta
Generic Context -> EKOS Context delta
Model -> EKOS Context delta
```

It also emits one of three interpretation labels:

```text
EKOS-specific win
structured-context-only win
inconclusive
```

Mock validation result:

```text
581 passed, 2 skipped
```

The mock smoke path produced aligned pairs and the expected ordering:

```text
Model < Generic Context < EKOS Context
```

Important limitation:

The mock result validates mechanics only. It does not prove EKOS-specific
causal value. The next evidence step is to run the real Codex CLI negative
control and inspect whether `Generic Context -> EKOS Context` remains positive
without increasing over-delegation or hard failures.

Real Codex CLI R5 result:

```bash
python -m ekos benchmark negative-control \
  --system codex-cli \
  --cases CASE-011,CASE-012,CASE-013,CASE-014,CASE-015 \
  --prompt-variants baseline,audit-emphasis \
  --runs 5 \
  --out out/edb-negative-control-codex-2026-06-30-r5
```

Local result artifact:

```text
out/edb-negative-control-codex-2026-06-30-r5/
```

Pair alignment:

```text
50/50 aligned triplets
```

All-triplet result:

| Metric | Model | Generic Context | EKOS Context |
| --- | ---: | ---: | ---: |
| Total score | 18.520 | 18.260 | 19.060 |
| Max-safe accuracy | 1.000 | 0.980 | 0.980 |
| Over-delegation rate | 0.000 | 0.000 | 0.000 |
| Blocking risk stability | 0.800 | 0.900 | 1.000 |
| Reversibility stability | 0.900 | 0.700 | 1.000 |
| Audit proxy stability | 0.300 | 0.300 | 0.400 |
| Critical-field stability | 0.889 | 0.878 | 0.933 |

Delta comparisons:

| Comparison | Score delta | Reviewability delta | Consistency delta | Safety delta | Utility delta | Enterprise delta |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Model -> Generic Context | -0.260 | -0.008 | -0.011 | -0.013 | -0.020 | -0.013 |
| Generic Context -> EKOS Context | 0.800 | 0.067 | 0.056 | 0.000 | 0.000 | 0.032 |
| Model -> EKOS Context | 0.540 | 0.058 | 0.044 | -0.013 | -0.020 | 0.019 |

Availability and parsing:

| Condition | Records | Parse failures | Runner errors |
| --- | ---: | ---: | ---: |
| Model | 50 | 0 | 0 |
| Generic Context | 50 | 0 | 1 |
| EKOS Context | 50 | 0 | 1 |

Runner errors:

| Condition | Case | Variant | Run | Error |
| --- | --- | --- | ---: | --- |
| Generic Context | CASE-015 | baseline | 3 | timeout after 300 seconds |
| EKOS Context | CASE-011 | baseline | 4 | timeout after 300 seconds |

Clean-triplet view excluding any triplet with a parse or runner error:

| View | Triplets | Model | Generic Context | EKOS Context |
| --- | ---: | ---: | ---: | ---: |
| All triplets | 50 | 18.520 | 18.260 | 19.060 |
| Clean triplets | 48 | 18.562 | 18.583 | 19.417 |

Clean-triplet score deltas:

| Comparison | Delta |
| --- | ---: |
| Model -> Generic Context | 0.021 |
| Generic Context -> EKOS Context | 0.833 |
| Model -> EKOS Context | 0.854 |

Interpretation:

```text
The real Codex CLI negative-control run strengthens the EKOS-specific delta
hypothesis. Generic structured context did not explain the improvement; in the
all-triplet view it slightly underperformed the model-only condition. EKOS
context improved over generic context on total score, reviewability, and
critical-field stability without increasing over-delegation.
```

The claim still needs discipline:

```text
This is CLI-agent evidence, not clean provider API evidence. It supports a
stronger EKOS-specific hypothesis under the tested Codex CLI conditions, but it
does not yet prove provider-independent causal value.
```

### 1.1 Negative Control Robustness

The first negative-control result answered one objection:

```text
Generic JSON source notes did not explain the EKOS delta.
```

It did not answer the stronger objection:

```text
Maybe a different non-EKOS structured format would produce the same gain.
```

EKOS implementation issue:

- `2000silpeed/ekos-sap-knowledge-os#11`

Implementation result:

```text
Status: implemented in EKOS
Command: python -m ekos benchmark negative-control-robustness
```

The EKOS implementation now compares:

```text
Model only
Model + generic-json
Model + generic-yaml
Model + generic-markdown
Model + generic-table
Model + generic-tree
Model + EKOS structured context
```

The generic controls are deliberately not EKOS. They may organize source facts,
object references, snippets, simple key-value facts, and generic summaries.
They must not contain EKOS authority mapping, evidence-to-authority mapping,
policy-boundary interpretation, completed DelegationDecisionContract fields,
blocking-risk interpretation, reversibility classification, or gold labels.

The runner exports:

```text
before/results.json
<generic-variant>/results.json
<generic-variant>/results.csv
after/results.json
after/results.csv
robustness_delta.json
robustness_delta.csv
summary.md
raw/
```

It reports:

```text
Model -> each Generic Context delta
each Generic Context -> EKOS Context delta
best Generic Context -> EKOS Context delta
average Generic Context -> EKOS Context delta
over-delegation comparison
parse/runner errors
interpretation label
```

Mock validation result:

```text
589 passed, 2 skipped
```

Mock smoke result:

```text
Model < each Generic Context < EKOS Context
```

The mock result validates mechanics only. It does not prove real-model behavior.

Real Codex CLI R3 result:

```bash
python -m ekos benchmark negative-control-robustness \
  --system codex-cli \
  --cases CASE-011,CASE-012,CASE-013,CASE-014,CASE-015 \
  --runs 3 \
  --prompt-variants baseline,audit-emphasis \
  --out out/edb-negative-control-robustness-codex-r3
```

Local result artifact:

```text
out/edb-negative-control-robustness-codex-r3/
```

Pair alignment:

```text
30/30 aligned condition sets
```

All-pair result:

| Metric | Model | generic-json | generic-yaml | generic-markdown | generic-table | generic-tree | EKOS |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Total score | 18.767 | 17.833 | 18.433 | 17.933 | 18.500 | 18.467 | 19.467 |
| Max-safe accuracy | 1.000 | 0.967 | 1.000 | 0.967 | 1.000 | 1.000 | 1.000 |
| Over-delegation rate | 0.000 | 0.000 | 0.000 | 0.000 | 0.000 | 0.000 | 0.000 |
| Blocking risk stability | 0.900 | 0.800 | 0.900 | 1.000 | 0.900 | 0.900 | 1.000 |
| Reversibility stability | 0.800 | 0.800 | 0.800 | 0.800 | 0.800 | 0.700 | 1.000 |
| Audit proxy stability | 0.400 | 0.800 | 0.200 | 0.000 | 0.400 | 0.200 | 0.300 |
| Critical-field stability | 0.900 | 0.933 | 0.878 | 0.867 | 0.900 | 0.867 | 0.922 |
| Parse failure rate | 0.000 | 0.000 | 0.000 | 0.000 | 0.000 | 0.000 | 0.000 |
| Runner error rate | 0.000 | 0.033 | 0.000 | 0.033 | 0.000 | 0.000 | 0.000 |

Generic-to-EKOS deltas:

| Generic control | Score delta | Reviewability delta | Safety delta | Enterprise delta |
| --- | ---: | ---: | ---: | ---: |
| generic-json | 1.633 | 0.114 | 0.022 | 0.048 |
| generic-yaml | 1.033 | 0.086 | 0.000 | 0.036 |
| generic-markdown | 1.533 | 0.106 | 0.022 | 0.059 |
| generic-table | 0.967 | 0.081 | 0.000 | 0.030 |
| generic-tree | 1.000 | 0.083 | 0.000 | 0.038 |

Best and average generic comparisons:

| Comparison | Score delta | Reviewability delta | Consistency delta | Safety delta | Utility delta | Enterprise delta |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Best Generic -> EKOS (`generic-table`) | 0.967 | 0.081 | 0.022 | 0.000 | 0.000 | 0.030 |
| Average Generic -> EKOS | 1.233 | 0.094 | 0.033 | 0.009 | 0.013 | 0.042 |

Availability and parsing:

| Condition | Records | Parse failures | Runner errors |
| --- | ---: | ---: | ---: |
| Model | 30 | 0 | 0 |
| generic-json | 30 | 0 | 1 |
| generic-yaml | 30 | 0 | 0 |
| generic-markdown | 30 | 0 | 1 |
| generic-table | 30 | 0 | 0 |
| generic-tree | 30 | 0 | 0 |
| EKOS | 30 | 0 | 0 |

Runner errors:

| Condition | Case | Variant | Run | Error |
| --- | --- | --- | ---: | --- |
| generic-json | CASE-014 | audit-emphasis | 1 | timeout after 300 seconds |
| generic-markdown | CASE-015 | audit-emphasis | 0 | timeout after 300 seconds |

Clean-pair view excluding any condition set with a parse or runner error:

| View | Pairs | Model | generic-json | generic-yaml | generic-markdown | generic-table | generic-tree | EKOS |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| All pairs | 30 | 18.767 | 17.833 | 18.433 | 17.933 | 18.500 | 18.467 | 19.467 |
| Clean pairs | 28 | 18.679 | 18.393 | 18.393 | 18.357 | 18.500 | 18.464 | 19.464 |

Clean-pair Generic-to-EKOS score deltas:

| Generic control | Delta |
| --- | ---: |
| generic-json | 1.071 |
| generic-yaml | 1.071 |
| generic-markdown | 1.107 |
| generic-table | 0.964 |
| generic-tree | 1.000 |

Interpretation:

```text
The Codex CLI R3 robustness run supports an EKOS-specific win against the
evaluated generic structured-context baselines. EKOS outperformed every generic
format in both the all-pair view and the clean-pair view, with no unsafe
over-delegation increase.
```

The claim remains bounded:

```text
This is still CLI-agent evidence, not provider-independent model evidence.
The result supports the phrase "EKOS-specific win against the evaluated generic
structured-context baselines." It does not prove universal EKOS superiority,
production readiness, or provider-independent causal value.
```

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

Implementation result:

```text
Status: implemented in EKOS
Command: python -m ekos benchmark delta-ablation
```

The EKOS implementation now supports component ablation over EKOS structured
context. The default variants are:

```text
full
no-evidence
no-policy
no-relationship-paths
no-process-state
no-authority-boundary
```

The implementation also made `relationship_paths` an explicit field in full
EKOS context so the `no-relationship-paths` variant can remove a real component
rather than a placeholder.

The runner exports:

```text
full/results.json
full/results.csv
<ablation-variant>/results.json
<ablation-variant>/results.csv
ablation_delta.json
ablation_delta.csv
summary.md
raw/
```

The report records:

```text
score_drop_from_full
reviewability_drop_from_full
consistency_drop_from_full
safety_drop_from_full
over_delegation_rate_increase
hard_fail_rate_increase
not_yet_supported_components
```

Mock validation result:

```text
585 passed, 2 skipped
```

The mock smoke path verifies mechanics only:

```text
full > no-evidence
full == no-process-state
```

This is deliberate. It proves that the report can both detect a component whose
removal hurts performance and flag a component whose removal has no measurable
effect under the tested cases.

Important limitation:

The ablation runner is a falsification tool. A component listed under
`not_yet_supported_components` is not proven useless in general. It means the
current EDB cases and runner did not show measurable degradation when that
component was removed.

Real Codex CLI R3 result:

```bash
python -m ekos benchmark delta-ablation \
  --system codex-cli \
  --cases CASE-011,CASE-012,CASE-013,CASE-014,CASE-015 \
  --runs 3 \
  --prompt-variants baseline,audit-emphasis \
  --out out/edb-delta-ablation-codex-r3
```

Local result artifact:

```text
out/edb-delta-ablation-codex-r3/
```

Pair alignment:

```text
30/30 aligned pairs
```

All-pair result:

| Metric | Full | No evidence | No policy | No relationship paths | No process state | No authority boundary |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Total score | 19.433 | 19.533 | 19.367 | 19.500 | 19.500 | 19.567 |
| Max-safe accuracy | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Over-delegation rate | 0.000 | 0.000 | 0.000 | 0.000 | 0.000 | 0.000 |
| Blocking risk stability | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Reversibility stability | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Audit proxy stability | 0.300 | 0.600 | 0.700 | 0.600 | 0.400 | 0.700 |
| Critical-field stability | 0.922 | 0.956 | 0.967 | 0.956 | 0.933 | 0.967 |

Component contribution:

| Removed component | Score drop | Reviewability drop | Consistency drop | Safety drop | Over-delegation increase | Hard-fail increase |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| No evidence | -0.100 | -0.008 | -0.033 | -0.000 | 0.000 | 0.000 |
| No policy | 0.067 | 0.006 | -0.044 | -0.000 | 0.000 | 0.000 |
| No relationship paths | -0.067 | -0.006 | -0.033 | -0.000 | 0.000 | 0.000 |
| No process state | -0.067 | -0.006 | -0.011 | -0.000 | 0.000 | 0.000 |
| No authority boundary | -0.133 | -0.011 | -0.044 | -0.000 | 0.000 | 0.000 |

Availability and parsing:

| Condition | Records | Parse failures | Runner errors |
| --- | ---: | ---: | ---: |
| Full | 30 | 0 | 0 |
| No evidence | 30 | 0 | 0 |
| No policy | 30 | 0 | 0 |
| No relationship paths | 30 | 0 | 0 |
| No process state | 30 | 0 | 0 |
| No authority boundary | 30 | 0 | 0 |

Clean-pair result:

```text
Clean pairs: 30/30
Full mean score: 19.433
No evidence drop: -0.100
No policy drop: 0.067
No relationship paths drop: -0.067
No process state drop: -0.067
No authority boundary drop: -0.133
```

Interpretation:

```text
This R3 ablation run does not show a meaningful component drop. The only
positive score drop is no-policy at 0.067 points, while removing several other
components slightly improved the average score. There were no parse failures,
runner errors, unsafe over-delegation increases, or hard-fail increases.
```

Sprint decision:

```text
Do not expand this exact ablation to R5 yet. The current R3 result is more
consistent with component-insensitive Codex CLI behavior on CASE-011~015 than
with a clear single-component causal signal. R5 becomes useful after adding
more discriminative cases, tightening the score components that should depend
on the removed context, or targeting the small no-policy signal with a narrower
follow-up run.
```

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

Implementation result:

```text
Status: implemented in EKOS
Command: python -m ekos benchmark provider-delta
EKOS commit: 4ffb5e9
```

The EKOS implementation now supports a clean provider API delta runner:

```text
A. before = provider API model with the raw EDB prompt
B. after  = provider API model with EKOS structured context added
```

This runner reuses the existing OpenAI, Anthropic, and Gemini provider adapters
from the live-model runner. It uses the same `DelegationDecisionContract` JSON
schema and the same delegation scorer as the CLI delta, negative-control,
robustness, and ablation runners.

The point is not to prove EKOS superiority by implementation. The point is to
remove one major confound from future evidence:

```text
Codex CLI product wrapper effect
```

The runner records:

```text
exact provider model IDs
provider display names
temperature
prompt variant
run index
prompt version
prompt SHA-256 hashes
raw provider responses
redacted provider requests
token usage where available
parse errors
runner errors
per-model deltas
skipped provider/model configs
```

It exports:

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

The runner deliberately treats unavailable providers as benchmark state rather
than benchmark failure. If an API key is missing, placeholder-like, or if the
configured model ID is still a placeholder such as `replace-with-gemini-model`,
the model is recorded under `skipped_models` and the provider is not called.

Mock validation:

```text
593 passed, 2 skipped
```

Mock smoke command:

```bash
python -m ekos benchmark provider-delta \
  --mock \
  --models "" \
  --model openai:mock-openai:OpenAI-mock \
  --cases CASE-011 \
  --temperatures 0.0 \
  --prompt-variants baseline \
  --runs 1 \
  --out out/edb-provider-api-delta-smoke-2
```

Mock smoke result:

```text
Mode: mock
Models run: 1
Models skipped: 0
Pair alignment: 1/1
Total score: 18.000 -> 20.000
Score delta: +2.000
Enterprise Delta Score: 0.053
Parse failure rate: 0.000 -> 0.000
Runner error rate: 0.000 -> 0.000
Over-delegation rate: 0.000 -> 0.000
```

Provider config skip smoke command:

```bash
python -m ekos benchmark provider-delta \
  --models models/live-models.example.json \
  --cases CASE-011 \
  --temperatures 0.0 \
  --prompt-variants baseline \
  --runs 1 \
  --out out/edb-provider-api-delta-skip-smoke-2
```

Provider config skip smoke result:

```text
Mode: provider-api
Models run: 0
Models skipped: 3
Pair alignment: 0/0
Skipped reason: missing or placeholder provider model ID
```

This skip smoke matters because local environments may contain some provider
API keys. The example config must not accidentally spend API calls against
placeholder model IDs.

Current limitation:

```text
No real provider API delta evidence has been collected yet for Issue #10.
```

The implementation removes the CLI-product-wrapper confound for future runs,
but it does not itself establish provider-independent EKOS value. The next
evidence step requires real, explicitly selected provider model IDs and API
keys supplied outside git.

Recommended real-provider first run:

```bash
python -m ekos benchmark provider-delta \
  --models models/live-models.local.json \
  --cases CASE-011,CASE-012,CASE-013,CASE-014,CASE-015 \
  --temperatures 0.0 \
  --prompt-variants baseline,audit-emphasis \
  --runs 3 \
  --out out/edb-provider-api-delta-r3
```

Interpretation discipline:

```text
Provider API delta evidence can avoid CLI-agent wrapper effects, but it still
depends on exact provider model IDs, schema support, API parameters, provider
availability, and response behavior. It does not prove business ROI,
production readiness, or universal EKOS superiority.
```

### 3.1 Z.ai / GLM Provider Extension

After the initial Provider API Delta runner, EKOS added a Z.ai provider adapter:

```text
EKOS commit: b24485e
Provider id: zai
Target model id tested: glm-5.2
Default key env: ZHIPU_API_KEY
Endpoint: https://api.z.ai/api/paas/v4/chat/completions
```

Why this matters:

```text
Z.ai GLM is another provider-side model surface that can help test whether the
EKOS delta survives outside Codex CLI and outside the first three provider
families.
```

Implementation notes:

```text
The Z.ai adapter uses the OpenAI-compatible Chat Completions API, not OpenAI's
Responses API. It requests JSON output with response_format=json_object,
disables thinking for benchmark determinism, and records raw responses through
the same EDB result schema.
```

The local machine had a stored `ZHIPU_API_KEY` in another project:

```text
/Users/sungwoon/ai-projects/youtube-auto/remotion-auto-video/.env
```

The key value was not recorded. Only its existence and length were inspected.

Validation:

```text
594 passed, 2 skipped
```

Live smoke attempts:

```text
1. zai:glm-5.2 against https://api.z.ai/api/paas/v4
2. zai:glm-4.7-flash against https://open.bigmodel.cn/api/paas/v4
```

Both attempts failed with:

```text
401 Unauthorized
```

Interpretation:

```text
The adapter is implemented and unit-tested, but the currently stored key is not
usable for live Z.ai Provider API Delta evidence. This is not evidence for or
against GLM-5.2 behavior. It is an authentication/configuration blocker.
```

Next action:

```text
Provide or refresh a valid Z.ai API key, preferably under ZHIPU_API_KEY or
Z_AI_API_KEY, then rerun the CASE-011 smoke before any R3 provider run.
```

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

Original priority order before the causality-control sprint:

1. Negative control
2. Ablation study
3. Availability-aware reporting hardening
4. Provider API delta runner

Reason:

Negative control and ablation attack the main causal question directly.

Provider API is important, but it should not replace the causality controls.

Updated priority after Issues #8, #9, #10, and #11:

1. Run real Provider API Delta R3 with explicitly selected provider model IDs.
2. If provider API delta is positive, add provider-side generic structured-context controls.
3. If provider API delta is weak or negative, inspect whether CLI-agent gains were wrapper-specific.
4. Add more discriminative ablation cases before repeating the same ablation at R5.
5. Keep availability-aware reporting in every result summary.

Reason:

The CLI-agent causality controls are now stronger than the provider API
evidence. The remaining large weakness is no longer implementation mechanics;
it is whether EKOS context still helps when the product wrapper is removed.

---

## Claim Discipline

Allowed after current CLI-agent evidence:

> Same Codex CLI plus EKOS context improved EDB outputs under the tested schema and scorer, without increasing unsafe over-delegation.

Allowed after the robustness run:

> EKOS-specific win against the evaluated generic structured-context baselines under the tested Codex CLI conditions.

Allowed after Issue #10 implementation:

> EKOS now has a clean provider API delta runner whose mock and skip paths are validated.

Not yet allowed:

> EKOS has proven provider-independent causal value beyond generic structured context.

> EKOS improves enterprise ROI.

> EKOS is production-ready.

> EKOS is an authoritative policy or execution control system.

---

## Next Sprint Prompt

```text
Continue EKOS Research Sprint.

Current focus: EDB Provider API Delta evidence.

Issues #8, #9, #10, and #11 are implemented.

Next run:
python -m ekos benchmark provider-delta \
  --models models/live-models.local.json \
  --cases CASE-011,CASE-012,CASE-013,CASE-014,CASE-015 \
  --temperatures 0.0 \
  --prompt-variants baseline,audit-emphasis \
  --runs 3 \
  --out out/edb-provider-api-delta-r3

Record exact provider model IDs.
Record skipped providers as skipped, not failed.
Do not claim provider-independent EKOS value until real provider API data exists.
Do not start ERB yet.
Keep safety gate first: unsafe over-delegation must not increase.
```

Postscript on 2026-07-01:

```text
Before collecting full Provider API R3 evidence, the sprint added a
post-implementation cross-CLI replication run across Codex CLI and Claude Code
CLI using the existing delta benchmark without new benchmark logic. That result
is recorded in docs/research/edb/014-cross-cli-agent-evidence-r3.md.

Provider API Delta remains the cleaner path for removing CLI-product-wrapper
confounds. The cross-CLI result strengthens evaluated CLI-agent evidence, not
provider-independent causal proof.
```
