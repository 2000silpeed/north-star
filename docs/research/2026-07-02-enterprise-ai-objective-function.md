# EKOS Research Review - Enterprise AI Objective Function

**Date:** 2026-07-02

**Status:** H21 candidate research review

**Scope:** Review of the EKOS research trajectory from H11 through the H21
objective-function question.

**Mode:** Research review only. No new benchmark family, code change, EKOS
protocol change, or experiment is introduced here.

---

## Evidence Base And Claim Boundary

This note uses repository evidence only:

- `docs/research/etcb/004-semantic-compression.md` through
  `docs/research/etcb/014-human-review-pilot-ranking.md`
- `docs/research/edb/003-metrics.md`
- `docs/research/edb/010-consistency-stress-test-plan.md`
- `docs/research/edb/012-ekos-delta-benchmark.md`
- `docs/research/edb/015-provider-api-delta-r3.md`
- `docs/research/edb/016-failure-boundary-analysis.md`
- `docs/design-review/002-enterprise-trust.md`
- `docs/design-review/004-human-cognitive-leverage.md`
- `docs/design-review/005-enterprise-ai-adoption-ladder.md`
- `docs/design-review/006-delegation-validation-benchmark.md`
- `docs/design-review/007-ekos-delta-benchmark-review.md`
- `docs/reviews/falsification-strategy.md`
- `docs/research/2026-06-27-ekos-falsification-criteria.md`
- North Star Issue #2 comments for H20-B persona simulation results

This note does not treat LLM persona simulation as human validation. It does not
claim production ROI, production readiness, universal provider coverage, proven
model bias, or proven role-aware simulation.

The central conclusion is a research recommendation, not a validated law:

```text
Enterprise AI should optimize a constrained multi-objective function:

maximize operationally useful, reviewable, authority-correct decision support
that improves human cognitive leverage and downstream enterprise outcomes,
subject to hard safety and governance constraints.
```

Human preference, correctness, reviewability, business outcome, safety, and
governance are not interchangeable signals. Each matters. Each fails if treated
as the only target.

---

## 1. Problem Statement

The H11 through H20 research sequence moved EKOS away from a simple claim that
"more enterprise context makes answers better."

H11 and H12 showed that semantic compression could preserve the evaluated
failure-prone safety benefit while improving signal density. H13 through H19
then weakened any simple compression story: safety was more stable than
reviewability, context-shape effects were smaller than prompt and provider
effects, and prompt framing could stabilize compressed reviewability only in
bounded cells.

H20 added the human-reviewability challenge. The H20 protocol exists because
reviewability had been scorer-defined. The first human pilot showed that a
domain reviewer could rank blinded responses and that comparative ranking may
be easier than absolute scoring. H20-B then added LLM persona simulation as an
auxiliary method only. In Scenario 03, Gemini 2.5 Flash persona evaluation
matched the human audit preference for B but did not match the human operations
preference for A. Cross-model H20-B evidence remains unavailable because only
Gemini had usable credentials in the attempted run.

This creates the H21 candidate question:

```text
What should Enterprise AI optimize?
```

A single answer such as "human preference," "correctness," "reviewability," or
"business outcome" is not supported by the evidence. The research record has
already separated multiple axes:

- preference: what a human or simulated persona selects under a role frame
- correctness: whether facts, delegation level, authority boundary, and answer
  content match the case requirements
- reviewability: whether a reviewer can reconstruct the decision from evidence,
  policy, authority, and audit cues
- operational outcome: whether the answer improves task completion, repair
  cost, escalation, customer impact, elapsed time, or business KPI
- governance: whether semantic changes, policy boundaries, approvals, audit
  records, and ownership can be controlled over time
- safety: whether the system avoids unsafe over-delegation, prohibited actions,
  missing approvals, irreversible execution, and false authority

The correct objective must keep these axes distinct while specifying how they
interact.

---

## 2. Why Human Preference Is Insufficient

Human preference matters because enterprises are socio-technical systems.
Recommendations are used by operators, auditors, managers, governance reviewers,
and executives. If the output is not usable by the responsible human, the system
does not create practical leverage.

But preference is insufficient as the optimization target.

First, the evidence shows that preference is role-conditioned. H20-B Scenario
03 produced a split: the human operations baseline was A, the human audit
baseline was B, and Gemini personas selected B for operations and audit. This
does not prove model bias, but it does show that "the preferred answer" is not a
stable scalar without role, task, and responsibility context.

Second, humans do not always have the same decision responsibility. An
operations practitioner may value immediate next action and unmet condition
clarity. An audit reviewer may value reproducibility, evidence traceability, and
policy grounding. An executive decision owner may value escalation need and
business impact. Optimizing for an averaged preference can erase the authority
boundary between these roles.

Third, a user can prefer an answer that is fluent, concise, or action-oriented
but unsafe. EDB explicitly treats over-delegation as the most important safety
metric. A response that makes the user comfortable while authorizing a
prohibited, approval-gated, or irreversible action is not enterprise-safe.

Fourth, the H20 human pilot itself warned against reducing reviewability to
simple liking. The reviewer preferred authority, policy, evidence, and coherent
reasoning flow, and did not treat concision as inherently improving
reviewability.

If Enterprise AI optimizes only human preference, the likely failure mode is:

```text
role-flattened helpfulness that satisfies the immediate reviewer while hiding
authority, policy, safety, or audit risk.
```

This also means that RLHF-style preference optimization is not automatically
appropriate for enterprise AI. Human preference can be useful evidence, but only
when collected under explicit role, responsibility, task, and safety boundaries.

---

## 3. Why Correctness Alone Is Insufficient

Correctness matters because wrong enterprise recommendations can cause wrong
shipments, wrong approvals, incorrect escalations, compliance exposure, customer
damage, or unsafe execution. EDB therefore includes maximum safe delegation
accuracy, action category accuracy, approval boundary accuracy, evidence-policy
coverage, and reversibility classification.

Correctness alone is insufficient for three reasons.

First, EDB already rejects "answer correctness only" as the target. The metrics
document states that EDB measures whether a system can assign the correct level
of enterprise authority while preserving safety, usefulness, evidence, policy
grounding, and auditability.

Second, the consistency stress evidence showed that frontier systems can keep
the maximum safe delegation level stable while reviewability fields still vary.
In the R5 consistency run, maximum safe delegation level and action category
were stable for every system and case, but audit-note proxy was the most common
instability source. The research conclusion was that delegation correctness and
delegation consistency are separate evaluation axes.

Third, a correct final recommendation can still be operationally weak if it
does not explain why the action is safe, what preconditions must be met, which
evidence supports the decision, who must approve, or what risk blocks higher
delegation. This is why H11 through H19 repeatedly tracked reviewability
separately from safety and max-safe correctness.

If Enterprise AI optimizes only correctness, the likely failure mode is:

```text
right answer, weak control surface.
```

The system may answer correctly in a benchmark but fail enterprise adoption
because humans cannot audit, reproduce, approve, or safely execute the answer.

---

## 4. Why Reviewability Alone Is Insufficient

Reviewability matters because enterprise AI is rarely useful as uninspectable
automation. Reviewers must be able to reconstruct what evidence was used, what
policy applied, what authority boundary constrained the action, and why the
recommended next step is safe.

The positive EKOS evidence is heavily reviewability-related. Provider API R3
showed improved reviewability and reduced hard failures in the evaluated
Anthropic and Gemini conditions without increasing unsafe over-delegation.
ETCB H11 through H19 showed that authority, policy, evidence, audit, blocking
risk, reversibility, and approval cues are central to preserving safe,
reviewable answers under compression.

But reviewability alone is insufficient.

First, reviewability can become an output-style target. H17 showed that
`prompt_variant` explained more observed reviewability variance than
`context_shape`; H19 showed that `audit-emphasis` stabilized `compressed` and
`ultra` in the evaluated matrix. This is useful, but it also raises the risk
that benchmarked reviewability may reward audit-style prose rather than actual
reviewer accuracy or operational value.

Second, falsification notes explicitly warn that evidence links may make
answers look more auditable without improving correctness, reviewer decisions,
or operational outcomes. A structured but wrong answer can be more dangerous
than an unstructured wrong answer because it may look authoritative.

Third, H20-B Scenario 03 is a warning sign. Gemini persona simulation selected
B for both operations and audit, while the human operations baseline was A and
the human audit baseline was B. This does not prove model bias or prompt
failure. It does show that audit-style reviewability may be favored by an LLM
evaluator even when the operations role has a different preference.

If Enterprise AI optimizes only reviewability, the likely failure mode is:

```text
auditable-looking answers that improve rubric scores but do not improve
decisions, reviewer accuracy, review time, escalation quality, or business
outcomes.
```

Reviewability should therefore be treated as a necessary mediator of enterprise
trust, not the final objective.

---

## 5. Why Operational Outcome Alone Is Insufficient

Operational outcome matters because enterprise AI ultimately has to improve
work. A system that is safe, correct, and reviewable but never reduces error,
elapsed time, escalation cost, reviewer burden, customer impact, or audit effort
has not yet proven enterprise value.

The research backlog already identifies this missing bridge. The Enterprise ROI
Benchmark is deferred because current EDB and ETCB results do not prove
business ROI. ETCB H10 also corrected the cost story: EKOS improved quality and
reduced hard failures, but increased first-turn token cost. The one-turn repair
experiment found EKOS favorable on the Gemini hard-failed subset, but still did
not measure human review time, multi-turn repair, escalation, production
workflow cost, or real business KPIs.

Operational outcome alone is insufficient because enterprise outcomes are
delayed, confounded, and gameable.

A narrow business KPI can reward unsafe shortcuts. Faster answer time can come
from omitting evidence. Fewer escalations can come from over-delegation.
Customer-impact reduction can hide compliance risk. Audit preparation time can
fall because the system produces confident-looking packets, even if reviewers
become less accurate at detecting wrong evidence.

If Enterprise AI optimizes only operational outcome, the likely failure mode is:

```text
local KPI improvement that externalizes safety, governance, compliance, or
future audit risk.
```

Operational outcome should be the long-run external validation target, but only
inside hard safety and governance constraints.

---

## 6. Candidate Enterprise Objective Functions

### Candidate 1: Maximize Human Preference

This would optimize for what users or reviewers choose.

Status:

```text
Rejected as sole target.
```

Reason:

Preference is role-conditioned, can conflict across enterprise responsibilities,
and can favor unsafe or hard-to-audit outputs. H20 and H20-B support collecting
preference as evidence, not treating it as a universal reward.

### Candidate 2: Maximize Correctness

This would optimize for correct facts, labels, or delegation levels.

Status:

```text
Rejected as sole target.
```

Reason:

EDB and consistency evidence separate correctness from safety, reviewability,
policy grounding, and adoption utility. Correct delegation level alone does not
make an answer operationally usable.

### Candidate 3: Maximize Reviewability

This would optimize for evidence traceability, policy grounding, audit notes,
and reproducibility.

Status:

```text
Rejected as sole target.
```

Reason:

Reviewability is necessary but can become presentation quality. H17 through H19
showed strong prompt effects. Falsification notes require correlation with
expert correctness, investigation time, escalation accuracy, audit readiness,
or calibrated reviewer confidence.

### Candidate 4: Maximize Operational Outcome

This would optimize for task completion, lifecycle cost, review time,
escalation quality, customer impact, and ROI.

Status:

```text
Rejected as unconstrained target; retained as long-run validation target.
```

Reason:

Business outcomes are the point of enterprise AI, but current evidence does not
yet measure production outcomes. Unconstrained KPI optimization can reward
unsafe shortcuts.

### Candidate 5: Maximize Safety

This would minimize unsafe over-delegation, hard failures, prohibited actions,
and missing approvals.

Status:

```text
Rejected as sole target; retained as hard constraint.
```

Reason:

EDB explicitly prevents systems from winning by always refusing or always
escalating. A perfectly conservative system may be safe but useless for
enterprise adoption.

### Candidate 6: Maximize Governance

This would optimize policy control, semantic versioning, ownership,
auditability, approvals, and rollback.

Status:

```text
Rejected as sole target; retained as hard constraint and operating requirement.
```

Reason:

Governance is required for enterprise deployment, but governance alone can
become stale, slow, or bureaucratic. Falsification criteria state that EKOS
fails if governance cannot update meaning within operational timelines.

### Candidate 7: Constrained Enterprise Objective

This would optimize operationally useful, reviewable, authority-correct
decision support under hard safety and governance constraints.

Status:

```text
Recommended current EKOS objective function.
```

Reason:

It is the only candidate that fits the evidence without collapsing separate
failure modes into one score.

---

## 7. Trade-Off Analysis

| Axis | Why it matters | If optimized alone | Failure mode |
| --- | --- | --- | --- |
| Preference | Captures role usability, reviewer trust, and adoption friction. | Rewards whatever the immediate evaluator likes. | Role-flattened helpfulness, unsafe approval, overfitting to fluent or concise answers. |
| Correctness | Prevents wrong decisions, wrong facts, wrong delegation, and wrong authority. | Treats the final answer as enough. | Correct but unreviewable, unauditable, hard to approve, or unstable under repeated calls. |
| Reviewability | Lets humans reconstruct evidence, policy, authority, and decision path. | Rewards audit-shaped prose or rubric-shaped output. | Auditable-looking wrong answers; reviewability theater. |
| Operational outcome | Connects AI output to enterprise value. | Rewards narrow KPIs without constraints. | Faster but unsafe decisions, hidden compliance risk, local optimization. |
| Safety | Prevents unsafe over-delegation and prohibited execution. | Encourages refusal and escalation. | Safe but useless system that cannot move work forward. |
| Governance | Controls policy, semantic change, ownership, approval, audit, and rollback. | Prioritizes process over task utility. | Stale governance, slow adaptation, compliance paperwork without operational gain. |

The evidence suggests the axes should not be merged too early.

Safety and governance behave like constraints. They define the permissible
space. Correctness is a minimum viability condition. Reviewability is the
human-control surface that makes correctness and safety inspectable. Operational
outcome is the long-run external validation target. Preference is a
role-conditioned diagnostic signal.

This ordering matters. If safety becomes a soft bonus, a high-utility answer can
over-delegate. If governance becomes a soft bonus, the system can drift from
current policy. If reviewability becomes the top-level objective, audit-style
answers can displace operationally useful answers. If human preference becomes
the target, the system can optimize for the wrong human at the wrong decision
boundary.

---

## 8. Recommended EKOS Objective Function

The recommended EKOS objective function is:

```text
Maximize governed operational reliability:

safe, authority-calibrated, evidence-grounded, reviewable decision support
that reduces human verification burden and improves enterprise workflow
outcomes, subject to safety and governance constraints.
```

In more operational terms:

```text
maximize:
  operational usefulness
  reviewer work reduction
  evidence-policy-grounded actionability
  human cognitive leverage

subject to:
  no increase in unsafe over-delegation
  no hard-fail unsafe delegation
  correct authority and approval boundaries
  sufficient factual and delegation correctness
  traceable evidence and policy grounding
  reproducible audit path
  governed semantic change and rollback
```

This means:

- Human preference is evidence, not the reward function.
- Correctness is necessary, not sufficient.
- Reviewability is necessary, not sufficient.
- Operational outcome is the long-run validation target, not an unconstrained
  shortcut metric.
- Safety is a hard gate, not a weighted preference.
- Governance is a hard operating condition, not decorative compliance.

This recommendation matches the design-review value chain:

```text
enterprise knowledge
-> enterprise trust
-> safe enterprise AI adoption
-> human cognitive leverage
```

The strongest evidence-supported version of the recommendation is narrower:

```text
In the evaluated EDB and ETCB settings, EKOS should be optimized for improved
safe delegation, reviewability, consistency, and evidence-policy grounding
without increasing unsafe over-delegation, while future work tests whether
those improvements predict human review quality and operational outcomes.
```

Anything stronger remains a hypothesis.

---

## 9. How Future Benchmarks Should Change

Future benchmarks should move from single-axis scoring to gated multi-objective
evaluation.

### 9.1 Safety Gate

No benchmark should treat an answer as successful if it increases unsafe
over-delegation, authorizes prohibited action, bypasses required approval, or
prepares irreversible execution beyond the allowed boundary.

### 9.2 Correctness Gate

Benchmarks should keep explicit correctness checks for business facts, object
identity, process state, delegation level, approval boundary, reversibility,
and evidence-policy linkage.

### 9.3 Governance Gate

Benchmarks should check whether policy, role, semantic version, approval, audit,
and rollback assumptions are explicit enough for enterprise control. This is
especially important before any production-readiness claim.

### 9.4 Reviewability As Human-Calibrated Mediator

Reviewability metrics should be calibrated against human reviewer behavior:

- blinded ordinal ranking
- reviewer rationale coding
- review time
- error detection on seeded wrong evidence
- calibrated confidence
- audit reconstruction success

The H20 observation that ranking may be easier than absolute scoring should be
preserved as a protocol-design signal.

### 9.5 Preference As Role-Conditioned Signal

Human preference should be collected under explicit responsibility frames:

- operations decision support
- audit reproducibility
- logistics delegation
- governance safety
- executive escalation

Preference should not be averaged into a generic "human likes it" score without
checking which role is responsible for the decision.

### 9.6 Persona Simulation As Auxiliary Diagnostic Only

H20-B should remain separate from H20-A. LLM personas can help generate
hypotheses about role framing, model bias, or explanation-style preference, but
they must not be treated as human validation.

The current H20-B evidence supports only this statement:

```text
One Gemini evaluator matched the Scenario 03 human audit baseline and did not
match the human operations baseline.
```

It does not distinguish model bias, role-independent explanation preference, or
prompt design failure.

### 9.7 Operational Outcome Bridge

Future outcome benchmarks should test whether EDB and ETCB proxy improvements
predict:

- faster investigation
- better escalation accuracy
- fewer unresolved hard failures
- lower repair burden
- better audit readiness
- reduced reviewer effort without reduced reviewer accuracy
- measurable workflow KPI improvement without increased operational risk

Until then, ROI and production business impact remain unproven.

---

## 10. Open Research Questions

1. Do H20 human rankings correlate with scorer-defined reviewability across the
   full blind packet, or only in selected scenarios?
2. Does operations preference systematically differ from audit preference, or
   was the Scenario 03 split a local case artifact?
3. Does `audit-emphasis` improve actual audit reconstruction, or mainly produce
   audit-style prose that scorers and LLM evaluators prefer?
4. Can reviewability gains be shown to reduce human review time without
   reducing error detection?
5. What minimum governance checks are required before an EKOS output can be
   called enterprise-controlled rather than merely well-structured?
6. Which safety failures should remain absolute blockers even if operational
   utility is high?
7. When does EKOS context add useful enterprise meaning, and when does it add
   noise or cost?
8. Can semantic compression preserve human-calibrated reviewability, not only
   scorer-defined reviewability?
9. Can persona simulation predict any role-specific human judgment, or does it
   mostly expose evaluator-model preferences?
10. How should business outcome benchmarks avoid rewarding unsafe shortcuts?
11. What is the correct way to weight reviewer work reduction against evidence
    completeness and audit reproducibility?
12. At what point does governance overhead make EKOS slower than workflow-local
    tools, despite better semantic structure?

---

## Current Strongest Supported Interpretation

The correct optimization target of Enterprise AI is not a single axis.

The current EKOS evidence best supports a constrained multi-objective target:

```text
optimize for governed operational reliability:

operationally useful, reviewable, authority-correct decision support under hard
safety and governance constraints, with human preference used as
role-conditioned evidence and business outcome treated as the long-run
validation target.
```

This is stronger than optimizing for preference, correctness, or reviewability
alone.

It is weaker than claiming that EKOS has proven ROI, proven human validation,
or proven enterprise outcome improvement.
