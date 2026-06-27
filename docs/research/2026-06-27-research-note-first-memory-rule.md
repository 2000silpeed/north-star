# Research Note First Memory Rule

Date: 2026-06-27

Status: Research note

## Context
After creating AIOS v1, the operating model for North Star became clearer:

- AIOS is the research operating system.
- North Star is the research lab, strategy headquarters, and collective memory.
- EKOS is the first major research result and implementation product.

The user then added a stronger rule:

> From now on, every major discussion must create a Research Note preserving the full reasoning process, not only the conclusion. North Star is a collective research memory, not a documentation repository.

This is not a minor writing preference. It changes the default behavior of future AI collaborators.

## Original Discussion and Reasoning
The previous AIOS rule already said that new architectural insights require a Research Note before continuing.

That was useful, but too narrow.

It only triggered on new insights or architecture shifts. The new rule broadens the responsibility: every major discussion should create a Research Note, even when the discussion is not yet an architecture decision.

The reason is that North Star's value is not the number of documents it contains. Its value is the reasoning trail that lets future AI systems continue research without depending on hidden chat history.

## Initial Assumptions
Before this rule, the working assumptions were:

- Research Notes are required for new architectural insights.
- ADRs record durable decisions.
- Diary entries summarize session state.
- White papers and glossary documents preserve public-facing or specification content.

These assumptions remain valid, but they missed a gap.

Major discussions can contain important reasoning before they become ADRs, glossary terms, white paper sections, or implementation mappings. If that reasoning remains in chat only, North Star loses part of its research memory.

## Competing Ideas
### Option A: Keep the Existing Research Note Trigger
Benefits:

- Simpler.
- Avoids creating too many notes.
- Keeps Research Notes for major architecture shifts only.

Costs:

- Important reasoning may remain in chat.
- Future AI systems may see conclusions without knowing how they emerged.
- North Star can drift toward being a documentation repository instead of a collective research memory.

### Option B: Require Research Notes Only for Final Decisions
Benefits:

- Keeps the repository concise.
- Avoids recording unfinished discussions.
- Makes Research Notes feel more polished.

Costs:

- Loses exploratory reasoning.
- Hides rejected alternatives and turning points.
- Encourages premature closure.

### Option C: Require Research Notes for Every Major Discussion
Benefits:

- Preserves the full reasoning process.
- Makes future continuation possible across AI systems.
- Keeps North Star aligned with its identity as collective research memory.
- Prevents conclusions from becoming detached from their context.

Costs:

- Adds more writing overhead.
- Requires judgment about what counts as major.
- Can become noisy if applied to trivial work.

## Turning Point
The key sentence was:

> North Star is a collective research memory, not a documentation repository.

This clarifies that Research Notes are not optional supporting documents. They are the main memory mechanism for major discussions.

## Final Decision
Adopt the Research Note First Memory Rule:

> Every major discussion must create a Research Note preserving the full reasoning process, not only the conclusion.

This rule applies to discussions that affect:

- North Star identity
- AIOS rules
- EKOS thesis or architecture
- Repository boundaries
- Research methodology
- Evaluation philosophy
- Public narrative
- Glossary meaning
- Cross-repository strategy

Minor edits, formatting changes, and narrow typo fixes do not require a Research Note.

## Rejected Alternatives
Rejected: Preserve only the final conclusion.

Reason:

- Future AI systems need the path, not just the endpoint.
- Conclusions without context are easy to misapply.

Rejected: Use diary entries instead of Research Notes for major discussions.

Reason:

- Diary entries preserve session continuity.
- Research Notes preserve durable reasoning.
- Major discussions need the latter.

Rejected: Create Research Notes for every tiny change.

Reason:

- The rule is for major discussions.
- Overuse would make the research memory noisy.

## Open Questions
- Should Research Notes link directly to later ADRs when a discussion becomes a decision?
- Should there be a lightweight index of Research Notes?
- What threshold should distinguish a major discussion from a minor update?
- Should every future AI session begin by checking whether the current user message triggers a Research Note?

## Context for Future AI Collaborators
If you are a future AI system working in North Star, do not treat Research Notes as optional summaries.

For major discussions, the Research Note is the memory.

Preserve:

- Why the discussion started
- Initial assumptions
- Competing ideas
- Rejected alternatives
- Turning points
- Final decision or current hypothesis
- Open questions
- Next research

Do not compress the reasoning into a conclusion-only changelog.

## Next Research
1. Update AIOS so the Research Note First Memory Rule is explicit.
2. Update `AGENTS.md` so future agents know major discussions require Research Notes.
3. Consider adding a Research Note index after a few more notes exist.
