# EDB Research Note 014 — Cross-CLI Agent Evidence R3

**Date:** 2026-07-01

**Status:** Preliminary multi-CLI evidence

---

## Context

Issues #7 through #11 completed the implementation phase for the EDB Delta
Benchmark and its major causality controls:

- Delta validity controls
- Negative control
- Ablation study
- Provider API Delta Runner
- Negative-control robustness

At this point, the bottleneck moved from benchmark mechanics to empirical
evidence.

The immediate research question became:

> Does EKOS consistently improve multiple enterprise coding agents under
> identical benchmark conditions?

This run intentionally did not add benchmark families, scoring logic, schema
changes, prompt changes, or context redesign.

---

## Method

The same EDB Delta Benchmark was repeated against multiple CLI-agent surfaces.

Benchmark configuration:

```text
Cases: CASE-011, CASE-012, CASE-013, CASE-014, CASE-015
Prompt variants: baseline, audit-emphasis
Runs: 3
Conditions: Model only vs Model + EKOS context
Scorer: existing EDB delegation scorer
Schema: existing DelegationDecisionContract
Context: existing EKOS structured context
Methodology: unchanged EDB delta benchmark
```

Evaluated CLI agents:

```text
Codex CLI: codex-cli 0.142.3
Claude Code CLI: 2.1.196
Gemini CLI: unavailable locally; `gemini` command not found
```

Commands:

```bash
python -m ekos benchmark delta \
  --system codex-cli \
  --cases CASE-011,CASE-012,CASE-013,CASE-014,CASE-015 \
  --prompt-variants baseline,audit-emphasis \
  --runs 3 \
  --out out/edb-cross-cli-codex-r3-2026-07-01
```

```bash
python -m ekos benchmark delta \
  --system claude-cli \
  --cases CASE-011,CASE-012,CASE-013,CASE-014,CASE-015 \
  --prompt-variants baseline,audit-emphasis \
  --runs 3 \
  --out out/edb-cross-cli-claude-r3-2026-07-01
```

Combined local report:

```text
out/edb-cross-cli-r3-2026-07-01/cross_cli_summary.md
```

Each per-agent output directory contains:

```text
before/results.json
before/results.csv
after/results.json
after/results.csv
delta.json
delta.csv
summary.md
raw/
_before_consistency/
_after_consistency/
```

---

## Results

| System | Pair alignment | Score | Score delta | Reviewability delta | Consistency delta | Safety delta | Utility delta | Enterprise delta | Over-delegation | Parse failures | Runner failures |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Codex CLI | 30/30 | 18.500 -> 19.300 | +0.800 | +0.067 | +0.022 | +0.000 | +0.000 | +0.026 | 0.000 -> 0.000 | 0.000 -> 0.000 | 0.000 -> 0.000 |
| Claude Code CLI | 30/30 | 19.100 -> 19.800 | +0.700 | +0.058 | +0.044 | +0.000 | +0.000 | +0.028 | 0.000 -> 0.000 | 0.000 -> 0.000 | 0.000 -> 0.000 |

Consistency detail:

| System | Evidence stability delta | Policy stability delta | Blocking-risk stability delta | Reversibility stability delta | Audit-proxy stability delta | Critical-field stability delta |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Codex CLI | +0.000 | +0.000 | +0.100 | +0.200 | -0.100 | +0.022 |
| Claude Code CLI | +0.000 | +0.000 | +0.000 | +0.100 | +0.400 | +0.044 |

---

## Interpretation

The R3 cross-CLI result supports this bounded conclusion:

> EKOS consistently improves the evaluated CLI agents under identical EDB
> conditions.

It also supports:

> EKOS improves reviewability without increasing unsafe delegation.

The support is not universal. It covers Codex CLI and Claude Code CLI under the
tested CASE-011 through CASE-015 suite, two prompt variants, three runs, and the
current EDB scorer.

The result is meaningful because the positive delta is not limited to Codex CLI.
Claude Code CLI also improved under the same benchmark contract, with no parse
failures, no runner failures, and no over-delegation increase.

---

## Agent-Specific Behavior

The two agents did not improve in exactly the same way.

Codex CLI:

```text
Score delta: +0.800
Reviewability delta: +0.067
Critical-field stability delta: +0.022
Blocking-risk stability delta: +0.100
Reversibility stability delta: +0.200
Audit-proxy stability delta: -0.100
```

Claude Code CLI:

```text
Score delta: +0.700
Reviewability delta: +0.058
Critical-field stability delta: +0.044
Blocking-risk stability delta: +0.000
Reversibility stability delta: +0.100
Audit-proxy stability delta: +0.400
```

This difference should not be averaged away. A better interpretation is:

- Codex CLI gained most visibly on blocking-risk and reversibility stability,
  while audit-proxy stability weakened.
- Claude Code CLI gained most visibly on audit-proxy stability and
  critical-field stability.

The shared signal is not identical submetric movement. The shared signal is
positive total score, positive reviewability delta, positive critical-field
consistency delta, and no unsafe over-delegation increase.

---

## Rejected Claims

These conclusions are not supported by this run:

```text
Universal model superiority
Provider-independent proof
Enterprise ROI
Production readiness
```

Reason:

This remains CLI-agent evidence. CLI agents include product wrappers, system
prompts, local execution assumptions, output handling, and vendor-specific
behavior outside the clean provider API surface.

The Provider API Delta Runner from Issue #10 exists specifically to test
whether EKOS deltas survive after removing CLI-product-wrapper effects. That
evidence is still separate.

---

## Open Questions

1. Would Gemini CLI show the same direction if installed and available locally?
2. Would the same positive cross-CLI signal hold at R5?
3. Would clean provider API models show the same direction without CLI-agent
   wrappers?
4. Why does Codex CLI lose audit-proxy stability while Claude Code CLI gains it?

---

## Minimum Next Experiment

Do not create a new benchmark family.

The minimum strengthening experiment is one of:

```text
1. Enable Gemini CLI locally and run the same existing R3 delta benchmark.
2. Repeat Codex CLI and Claude Code CLI at R5 if CLI-level robustness is the
   immediate priority.
3. Run Provider API Delta R3 with explicitly selected provider model IDs if the
   priority is removing CLI-product-wrapper confounds.
```

The strongest falsification path is provider API evidence. The strongest
multi-agent CLI replication path is adding Gemini CLI or another available CLI
agent without changing the benchmark contract.

---

## Current Claim Discipline

Allowed:

> EKOS consistently improves the evaluated CLI agents under identical R3 EDB
> conditions.

Allowed:

> EKOS improves reviewability without increasing unsafe delegation in the
> evaluated CLI-agent runs.

Not allowed:

> EKOS has proven provider-independent causal value.

Not allowed:

> EKOS is production-ready or has proven enterprise ROI.
