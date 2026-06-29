# Research Note - EKOS Issue #2 Implementation Readiness

Date: 2026-06-29

Status: Readiness assessment

Repository boundary: North Star records the research and execution-readiness judgment. Runnable benchmark code belongs in `2000silpeed/ekos-sap-knowledge-os`.

## Context

The question was whether EKOS Issue #2, "Implement delegation validation benchmark CASE-011 through CASE-015," is truly ready to be handed to an implementation agent.

The claim being evaluated:

> CASE-011 through CASE-015 are already designed, so implementation instructions can be given immediately.

## Evidence Read

The following sources were checked:

- GitHub issue `2000silpeed/ekos-sap-knowledge-os#2`
- `origin/main:docs/design-review/006-delegation-validation-benchmark.md`
- `origin/main:docs/research/delegation-validation-benchmark-spec.md`
- `origin/main:docs/research/edb/001-gap-analysis.md`
- `origin/main:docs/research/edb/002-taxonomy.md`
- `origin/main:docs/research/edb/003-metrics.md`
- `origin/main:docs/research/edb/004-protocol.md`
- `origin/main:docs/research/edb/005-benchmark-cases.md`
- EKOS implementation repository at `/Users/sungwoon/ai-projects/Enterprise-knowledge-Operating-System`

The local North Star branch was behind `origin/main` by ten commits at the time of review. The delegation benchmark research exists on `origin/main`, not in the local working tree before merging.

The EKOS implementation repository was on `master`, aligned with `origin/master`, with no local working-tree changes at the time of review.

Current benchmark smoke tests were run in the EKOS implementation repository:

```text
tests/test_benchmark.py tests/test_capabilities.py
37 passed
```

## Readiness Judgment

Issue #2 is implementation-ready in the narrow sense:

- the benchmark objective is clear
- the Delegation Decision Contract is defined
- the required systems under test are named
- metrics and hard-fail conditions are specified
- CASE-011 through CASE-015 have scenario descriptions and gold labels
- the existing implementation pattern for CASE-001 through CASE-010 is already in place

It is not a zero-ambiguity coding ticket:

- the case scenarios still need concrete synthetic fixture records
- each baseline still needs deterministic output behavior
- scoring weights and report shape need implementation choices
- `model-only-direct-prompt` does not appear to exist yet in the current benchmark harness
- the current benchmark module is large and monolithic, so extending it is feasible but will add more complexity unless carefully scoped

## Recommended Implementation Scope

The implementation should be treated as a focused benchmark-engineering task, not as new EKOS architecture.

Recommended scope:

1. Add delegation constants, gold labels, and contract validation.
2. Add deterministic synthetic records for CASE-011 through CASE-015.
3. Add deterministic baseline outputs for all required systems, including `model-only-direct-prompt`.
4. Add delegation scoring with hard-fail detection, over-delegation rate, conservative failure rate, and Level 2/3 enablement.
5. Add CLI commands `benchmark case-011` through `benchmark case-015`.
6. Add tests for contract compliance, hard-fail conditions, summary fields, and CLI JSON output.
7. Run the benchmark and only then update public claims.

## Key Caution

The strongest risk is not technical feasibility. The strongest risk is accidentally turning the benchmark into a scripted EKOS win.

To avoid that, non-EKOS baselines should remain strong:

- `full-context-prompt` should receive all relevant plain-text facts
- `typed-tool` should receive realistic structured fields
- `agentic-retrieval-tool` should be allowed to detect some boundaries
- `model-only-direct-prompt` should be weak but not absurdly crippled

The result should be allowed to show that a baseline ties or beats EKOS. If that happens, the EKOS claim must be narrowed.

## Conclusion

Yes, Issue #2 can be implemented now.

The correct instruction is not "invent CASE-011 through CASE-015."

The correct instruction is:

> Implement the existing EDB specification from North Star `origin/main` into the EKOS benchmark harness, preserving strong baselines and falsification rules.

## Next Action

Before implementation starts:

1. Pull or merge the latest North Star `origin/main` so the EDB documents are local.
2. Create a feature branch in `2000silpeed/ekos-sap-knowledge-os`.
3. Implement Issue #2 in the EKOS repository only.
4. Record benchmark results back in North Star after tests pass.
