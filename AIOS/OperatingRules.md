# AIOS Operating Rules

Version: 1.0

Status: Draft

## Entry Protocol
Before strategic, architectural, or documentation work in North Star:

1. Read `AIOS/Constitution.md`.
2. Read `AIOS/MasterPrompt.md`.
3. Read `README.md`.
4. Read `AGENTS.md`.
5. Read the latest relevant ADR.
6. Read the latest relevant diary entry or research note.

## Repository Boundary Check
Before creating or editing anything, ask:

> Does this belong in North Star?

If the work is research, strategy, glossary, evaluation criteria, ADR, public narrative, or continuity, it can belong in North Star.

If the work is source code, prototype, runnable demo, test harness, sample data, CLI, API, application, or implementation module, it belongs in `2000silpeed/ekos-sap-knowledge-os`.

## No Implementation Drift
Do not create implementation structures inside North Star.

Forbidden unless explicitly instructed:

- `prototype/`
- `app/`
- `demo/`
- `data/`
- `runtime/`
- `cli/`
- Test harness
- API server
- Runnable notebook
- Sample implementation

## Research Note Trigger
Create a Research Note before continuing if:

- A major discussion begins.
- A new architectural insight appears.
- A major project hierarchy changes.
- A repository responsibility changes.
- A concept gains a new meaning.
- A rejected direction explains an important future risk.
- Multiple AI systems may later need the reasoning.

Major discussions include conversations that affect North Star identity, AIOS rules, EKOS thesis, repository boundaries, research methodology, evaluation philosophy, public narrative, glossary meaning, or cross-repository strategy.

Minor edits, typo fixes, and narrow formatting changes do not require a Research Note.

## Decision Protocol
When there are multiple possible directions:

1. Name the options.
2. Explain the assumptions behind each option.
3. Compare trade-offs.
4. State the recommended direction.
5. Explain why.
6. Record rejected alternatives.
7. Record open questions.

## Truth Protocol
Do not optimize for agreement.

Optimize for truth.

When evidence or reasoning conflicts with the user preference, current narrative, previous document, or planned roadmap:

1. Say the conflict directly.
2. Identify which claim, assumption, or decision is affected.
3. Separate facts, hypotheses, preferences, and unknowns.
4. State what evidence would resolve the disagreement.
5. Record the uncomfortable conclusion instead of smoothing it away.

Do not defend EKOS by default. EKOS must remain falsifiable.

## Document Creation Protocol
Create a document only when it preserves reasoning that would otherwise be lost.

Do not create documents only to appear productive.

## Update Protocol
When a document changes the way future AI systems should behave:

1. Update the relevant durable document.
2. Update `AGENTS.md` only if boot behavior changes.
3. Update diary or research notes if context would otherwise be lost.
4. Commit the change with a message that states the decision.

## Commit Rule
Commit durable research changes when the work is coherent and validated.

Do not push unless the user asks.

## Validation Rule
Before committing:

1. Run markdown or basic diff checks available in the repo.
2. Search for conflict markers.
3. Search for forbidden implementation paths when repository boundary is relevant.
4. Review the staged diff.

## Output Rule
Final responses should state:

- What changed
- Where the durable artifact lives
- Whether validation ran
- Whether a commit was created
- Whether the work was pushed
