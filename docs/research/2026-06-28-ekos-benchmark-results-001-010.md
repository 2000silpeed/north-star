# Research Note - EKOS Benchmark Results CASE-001 Through CASE-010

Date: 2026-06-28

Status: Initial result interpretation

Repository boundary: North Star records the research interpretation. Runnable benchmark code, fixtures, CLI commands, tests, and result-generation logic belong in `2000silpeed/ekos-sap-knowledge-os`.

Related EKOS implementation commit: `911beee feat: add CASE-010 stability benchmark`

## Context

North Star created `docs/research/ekos-benchmark-specification.md` to turn the EKOS thesis into executable research claims. The implementation repository then added runnable smoke benchmarks for CASE-001 through CASE-010.

This note records the first cumulative interpretation of those executable benchmark runs.

The purpose is not to claim that EKOS is proven. The purpose is to preserve what the current evidence says, what it does not say, and what research should happen next.

## Original Discussion And Reasoning

The research program moved from white paper claims into executable smoke tests because the strongest hostile-review objection was that EKOS could remain persuasive but untested.

The guiding question was:

> Does each research cycle make EKOS stronger?

For this cycle, the answer is yes, but only in a narrow sense:

- EKOS now has executable benchmark coverage for the ten cases specified in North Star.
- The EKOS implementation repository can run those cases through the CLI.
- The test suite passes after adding CASE-010.
- North Star now has enough run evidence to begin result interpretation.

The key reasoning shift is that benchmark results should not be treated as marketing proof. They should be treated as evidence that either strengthens, narrows, or challenges EKOS claims.

## Initial Assumptions

The benchmark cycle assumed:

- Synthetic SAP logistics fixtures are acceptable for first-pass smoke validation.
- Deterministic baselines are acceptable for establishing repeatable evaluation mechanics.
- EKOS should be compared against retrieval-only, full-context prompt, typed-tool, and agentic retrieval-tool baselines.
- EKOS wins should be interpreted relative to the strongest baseline, not the weakest.
- Independent utility metrics are necessary because EKOS-shaped metrics alone could reward the architecture rather than enterprise value.

These assumptions remain provisional.

## Current Run Evidence

The following command was run in `2000silpeed/ekos-sap-knowledge-os` after CASE-010 was added:

```text
.venv/bin/python -m pytest -q
```

Result:

```text
540 passed, 2 skipped
```

Each benchmark case was also executed through the implementation benchmark functions to collect the scores below.

| Case | Purpose | EKOS primary score | Strongest baseline | Delta vs strongest baseline | Smoke result |
| --- | --- | ---: | --- | ---: | --- |
| CASE-001 | Clean cross-pack delivery delay | 10/10 | agentic-retrieval-tool | +1 | PASS |
| CASE-002 | High-similarity distractor | 10/10 | agentic-retrieval-tool | +1 | PASS |
| CASE-003 | Messy identity variants | 10/10 | agentic-retrieval-tool | +1 | PASS |
| CASE-004 | Process-state paired cases | 10/10 | agentic-retrieval-tool | +1 | PASS |
| CASE-005 | Evidence conflict and staleness | 10/10 | typed-tool, agentic-retrieval-tool | +1 | PASS |
| CASE-006 | Policy and execution boundary trap | 12/12 | agentic-retrieval-tool | +1 | PASS |
| CASE-007 | Typed-tool sufficiency case | 12/12 | agentic-retrieval-tool | +1 | PASS |
| CASE-008 | Full-context sufficiency case | 12/12 | agentic-retrieval-tool | +1 | PASS |
| CASE-009 | Seeded semantic error / false-confidence case | 12/12 | full-context-prompt, agentic-retrieval-tool | +2 | PASS |
| CASE-010 | Model-change or prompt-change stability case | 14/14 | typed-tool | +1 | PASS |

## Additional Utility Evidence

Independent utility evidence currently exists only for CASE-008, CASE-009, and CASE-010.

| Case | Utility dimension | Retrieval-only | Full-context | Typed-tool | Agentic | EKOS |
| --- | --- | ---: | ---: | ---: | ---: | ---: |
| CASE-008 | Reviewability utility | 2/10 | 7/10 | 4/10 | 7/10 | 10/10 |
| CASE-009 | Seeded error detection | 1/10 | 8/10 | 7/10 | 8/10 | 10/10 |
| CASE-010 | Stable field invariance | 1/10 | 8/10 | 9/10 | 8/10 | 10/10 |

These utility metrics are useful but not yet independent human-review evidence. They are deterministic proxy metrics inside the EKOS implementation repository.

## Interpretation

The current evidence supports a cautious claim:

> EKOS currently outperforms the implemented baselines on the ten deterministic synthetic smoke cases.

The current evidence does not support a broad claim:

> EKOS is proven to be a production-ready enterprise intelligence layer.

The distinction matters. The benchmark suite is now useful because it narrows what EKOS can honestly claim.

### What Looks Stronger

EKOS is strongest where the answer must preserve several enterprise fields at once:

- Object identity.
- Relationship path.
- Evidence references.
- Process state.
- Policy or execution boundary.
- Fact, hypothesis, and recommendation separation.

CASE-008, CASE-009, and CASE-010 are especially useful because they test whether EKOS adds value beyond having more text, beyond trusting structure blindly, and beyond a single prompt or model form.

### What Still Looks Weak

The strongest baseline is often close to EKOS:

- Agentic retrieval-tool is strongest or tied strongest in most cases.
- Full-context prompt ties agentic on CASE-009 error-detection proxy.
- Typed-tool is the strongest baseline in CASE-010 stability.

This means the current evidence does not yet prove that EKOS is always necessary. It shows that EKOS has a consistent margin in this benchmark slice, but many margins are small.

If a future workflow-specific baseline ties EKOS, the EKOS thesis must be narrowed.

## Competing Ideas

### Option A: Treat CASE-001 Through CASE-010 As Proof

This would support immediate public narrative expansion.

Rejected for now. The evidence is synthetic, deterministic, and still missing human-review validation. Treating it as proof would overclaim.

### Option B: Treat CASE-001 Through CASE-010 As Mechanism Validation

This treats the current results as evidence that the benchmark harness and EKOS comparison mechanics work.

This is the strongest interpretation today. The benchmark is now capable of producing repeatable signals, but the signals still need stronger baselines and live model or human-review validation.

### Option C: Treat The Results As Mostly Circular

This argues that EKOS wins because the benchmark rewards EKOS-shaped structure.

Partially valid. CASE-008 through CASE-010 reduce this risk by adding reviewability, false-confidence, and stability proxy metrics, but the risk is not eliminated until independent reviewers and workflow-specific baselines are added.

## Turning Points

Two turning points matter:

1. CASE-008 made "full context is enough" testable. EKOS beat the full-context baseline on reviewability, but only in a deterministic proxy setting.
2. CASE-010 completed the first registered case suite and showed that typed tools are a serious baseline for stability.

CASE-010 is particularly important because typed-tool scored 9/10 on stability. That suggests tool/API design may solve a large part of the stability problem without requiring the full EKOS thesis.

## Current Hypothesis

Current hypothesis:

> EKOS adds the most value when enterprise reasoning requires simultaneous preservation of identity, relationship, evidence, state, and boundary semantics across adversarial or ambiguous conditions.

This is narrower and stronger than saying:

> EKOS is generally better than retrieval, full-context prompting, tools, or agents.

The narrower hypothesis is more falsifiable and better aligned with the current benchmark evidence.

## Rejected Alternatives

Rejected: Update the white paper immediately with benchmark-win claims.

Reason: The results are not yet strong enough for public narrative escalation. They should first be interpreted, stress-tested, and narrowed.

Rejected: Build additional runnable evaluation code inside North Star.

Reason: Runnable benchmarks belong in `2000silpeed/ekos-sap-knowledge-os`.

Rejected: Start CASE-011 immediately.

Reason: North Star has only registered CASE-001 through CASE-010. Adding CASE-011 before interpreting CASE-001 through CASE-010 would create benchmark sprawl.

## Open Questions

- Would EKOS still beat a workflow-specific context assembler baseline?
- Would EKOS still win if live LLM calls replaced deterministic answer packets?
- Would domain reviewers agree that EKOS answers are more reviewable?
- Does EKOS reduce review time, or only improve trace richness?
- Are the current scoring weights too favorable to explicit semantic structure?
- Which claim should be narrowed if typed-tool or agentic baselines tie EKOS?
- How much of EKOS's advantage comes from graph structure versus disciplined answer contracts?

## Context For Future AI Collaborators

Do not treat the current benchmark pass as production proof.

The implementation repository currently demonstrates a repeatable smoke benchmark suite. North Star should now use the results to guide stronger falsification, not to produce more persuasive writing.

Future work should preserve the repository boundary:

- Add or modify runnable benchmark code in `2000silpeed/ekos-sap-knowledge-os`.
- Record interpretation, claim revision, and benchmark strategy in North Star.

## Next Research

Highest-leverage next research:

> Define the next validation cycle for missing baseline and independence gaps before revising public EKOS claims.

Recommended order:

1. Add or specify the missing workflow-specific context assembler baseline.
2. Convert CASE-010 from deterministic prompt/model variants into a live-model or recorded-run protocol.
3. Design a reviewer protocol for CASE-008 and CASE-009 to measure review time, error detection, and confidence calibration.
4. Revisit the validation matrix and mark which claims are supported only by deterministic smoke evidence.
5. Update the white paper only after the stronger evidence changes the claim boundary.
