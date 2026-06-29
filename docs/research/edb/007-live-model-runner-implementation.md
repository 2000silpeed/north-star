# EDB-007 - Live Model Runner Implementation Result

**Date:** 2026-06-29

**Status:** Implemented in EKOS

**Related EKOS issue:** `2000silpeed/ekos-sap-knowledge-os#3`

**EKOS commit:** `2a75e6c feat: add live model benchmark runner`

---

## Result

EKOS Issue #3 was implemented and pushed to `master`.

The implementation adds a live model benchmark runner for EDB Phase 2:

- fixed CASE-011 through CASE-015 prompt generation
- fixed `DelegationDecisionContract` JSON schema generation
- provider adapters for OpenAI, Anthropic, and Gemini
- mock mode for CI and local validation without paid API calls
- temperature sweep and repeated-run support
- automatic scoring through the existing EKOS `score_delegation_answer` scorer
- exports for `results.json`, `results.csv`, `summary.md`, and `raw/`
- README and `.env.example` guidance

The GitHub issue was closed after the EKOS commit was pushed.

---

## Why This Matters

EDB-006 defined the need to move beyond EKOS versus internal deterministic baselines.

The new runner makes that next evidence step executable. It does not yet prove that EKOS outperforms commercial LLMs. It creates the measurement path needed to test that claim under a fair protocol:

1. one prompt
2. one schema
3. same CASE-011 through CASE-015 cases
4. same scorer
5. recorded temperature and repeated-run metadata

This is important because the research claim becomes falsifiable against external model behavior rather than only against project-owned baselines.

---

## Implementation Notes

The EKOS implementation added:

```text
ekos/runners/live_model_runner.py
ekos/runners/openai_runner.py
ekos/runners/anthropic_runner.py
ekos/runners/gemini_runner.py
models/live-models.example.json
tests/test_live_model_runner.py
```

The CLI entrypoint is:

```bash
python -m ekos benchmark live-models
```

Mock smoke example:

```bash
python -m ekos benchmark live-models \
  --mock \
  --cases CASE-011 \
  --temperatures 0.0 \
  --runs 1 \
  --out out/live-model-benchmark-smoke
```

Live mode requires configured provider model IDs and API keys supplied through environment variables or an uncommitted `.env`.

The implementation intentionally does not hardcode marketing model names.

---

## Verification

EKOS local verification:

```text
.venv/bin/python -m pytest -q
553 passed, 2 skipped
```

CLI mock smoke was also run against a temporary output directory and generated:

```text
results.json
results.csv
summary.md
raw/
```

---

## Limits

Paid live provider calls were not run during this implementation step.

Reason:

- provider API keys are intentionally external and uncommitted
- exact model IDs should be selected at run time
- the runner must not smuggle private provider/account metadata into the repository

Therefore the current evidence is:

```text
runner implemented and tested
mock execution verified
provider request shapes covered by tests
live benchmark results not yet collected
```

The next research artifact should be a dated live run result that records the exact provider/model IDs, temperatures, run count, scores, parse failures, and stability deltas.
