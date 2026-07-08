# Hermes + ChatGPT EKOS Continuation Workflow

Date: 2026-07-08
Status: Working research note

## Context

The user was previously developing `north-star` and EKOS with a three-party workflow:

```text
User + ChatGPT web + Codex
```

The user now wants Hermes to replace Codex while keeping ChatGPT web as an interaction partner. The work remains GitHub-repository based, with `north-star` and EKOS as the two primary repositories.

New workflow:

```text
User + ChatGPT web + Hermes
```

This note preserves the transition because it changes how future AI collaborators should interpret cross-chat reasoning, GitHub repository state, and intermediate user decisions.

## Repository Boundary Confirmed

The boundary from ADR-0002 still holds.

- `north-star` is the research memory, strategy, public narrative, product architecture, ADR, glossary, and portfolio headquarters.
- `2000silpeed/ekos-sap-knowledge-os` is the EKOS implementation repository.
- North Star should not recreate EKOS implementation code, runnable demos, prototype data, CLI, API, or runtime structures.

## Current Local Repository Baseline

### North Star

Path:

```text
/Users/sungwoon/ai-projects/north-star
```

Observed Git state at takeover:

```text
## main...origin/main
?? docs/architecture/
?? docs/research/2026-07-02-ekos-sap-dev-typescript-port-review.md
```

Relevant untracked North Star work already present:

- `docs/architecture/existing-sap-ingestion-inventory.md`
- `docs/research/2026-07-02-ekos-sap-dev-typescript-port-review.md`

The architecture inventory argues that EKOS already has a substantial SAP asset-ingestion, fragment, pack, and store foundation. It recommends not adding more hard-coded P0 logistics fields yet, and instead creating a YAML-driven configurable mapping prototype in EKOS that re-expresses the existing SAP-like delivery-delay workflow without adding new fields.

### EKOS implementation repository

Path:

```text
/Users/sungwoon/ai-projects/2000silpeed/ekos-sap-knowledge-os
```

Observed Git state at takeover:

```text
## master...origin/master
?? docs/demo/enterprise-workflow-demo-v2.md
```

Relevant untracked EKOS work already present:

- `docs/demo/enterprise-workflow-demo-v2.md`

The v2 demo guide keeps the underlying enterprise workflow artifacts unchanged, but changes presentation to lead with the enterprise control question:

```text
Why was Level 2 blocked?
```

The demo message is that EKOS is not ordinary RAG because it preserves the control chain from business object, evidence, policy, authority boundary, human review, and governance state.

## Verified Commands During Takeover

In EKOS:

```bash
.venv/bin/python -m pytest tests/test_sap_logistics_onboarding.py tests/test_enterprise_workflow_demo.py -q
```

Observed result:

```text
22 passed in 0.27s
```

The `demo --enterprise-workflow` CLI help was inspected. Relevant options include:

- `--enterprise-workflow`
- `--decision-packet`
- `--evidence-objects`
- `--quality-check`
- `--human-review`
- `--scenario`
- `--response`
- `--review-persona`
- `--out`

After explicit user approval, Hermes also inspected:

```bash
.venv/bin/python -m ekos capabilities --search demo --json
```

That confirmed two relevant command surfaces:

- `demo`
- `data-onboard sap-logistics-demo`

Hermes then ran two smoke paths to separate the existing artifact demo from the fresh SAP-like onboarding path.

Default existing-artifact workflow:

```bash
.venv/bin/python -m ekos demo --enterprise-workflow --out /tmp/ekos-hermes-takeover/default-workflow
```

Observed summary:

```text
Mode: deterministic_artifact_demo
Selected response: B
Final status: approved
Human Review: internal_audit_reviewer observed B (supports_selected_response)
```

Fresh SAP-like onboarding to enterprise workflow:

```bash
.venv/bin/python -m ekos data-onboard sap-logistics-demo \
  --out /tmp/ekos-hermes-takeover/onboarding \
  --json >/tmp/ekos-hermes-takeover/onboarding.json

.venv/bin/python -m ekos demo --enterprise-workflow \
  --decision-packet /tmp/ekos-hermes-takeover/onboarding/decision_packet.json \
  --evidence-objects /tmp/ekos-hermes-takeover/onboarding/evidence_objects.json \
  --out /tmp/ekos-hermes-takeover/workflow
```

Observed summary:

```text
Mode: sap_like_csv_to_enterprise_workflow_demo
Primary object: Delivery 80001241
Decision packet: DP-80001241-sap-logistics-demo
Final governance status: review_required
Human Review: not_available_for_synthetic_onboarding_demo
```

Interpretation:

- The existing default artifact demo can end in `approved` because it includes a compatible existing human-review observation.
- A fresh SAP-like onboarding demo without a compatible explicit review artifact correctly remains `review_required`.
- Future demo documentation must keep these two modes distinct to avoid overclaiming human validation or governance approval.

## ChatGPT Web Conversation Inclusion Protocol

The user explicitly asked that ChatGPT web conversation content be included in this workflow.

Future handling rule:

1. Treat ChatGPT web content as an external reviewer or collaborator contribution, not as authoritative repository truth.
2. Preserve useful ChatGPT reasoning in North Star when it affects strategy, architecture, product positioning, repository boundary, or implementation direction.
3. Preserve implementation-specific ChatGPT suggestions in EKOS only after Hermes verifies them against the actual repository state and tests.
4. Keep a clear separation between:
   - user decisions;
   - ChatGPT claims or proposals;
   - Hermes verification from local repository state;
   - accepted decisions;
   - rejected alternatives;
   - open questions.
5. Do not paste secrets, credentials, private company data, customer data, or sensitive code into ChatGPT web without explicit user approval.
6. If ChatGPT content is long, summarize it with enough quoted excerpts to preserve the reasoning path and avoid losing important nuance.

Suggested capture format:

```markdown
## ChatGPT Conversation Excerpt

Source: ChatGPT web
Date captured: YYYY-MM-DD
User-supplied excerpt: yes/no
Sensitivity review: public-safe / private / unknown

### Raw or lightly edited excerpt

> ...

### Extracted claims

1. ...
2. ...

### Hermes verification

- Confirmed by repository state: ...
- Not confirmed: ...
- Contradicted by repository state: ...

### Decision

- Adopt: ...
- Reject: ...
- Defer: ...
```

## Imported ChatGPT Share — 2026-07-08

The user provided this ChatGPT share URL:

```text
https://chatgpt.com/share/6a4db51d-c59c-83e8-b6be-143bc57eafb5
```

Hermes fetched the public share HTML, decoded the embedded conversation payload, filtered it to visible user/assistant text, and stored the transcript at:

```text
docs/research/sources/2026-07-08-chatgpt-share-ekos-research-continuation-transcript.md
```

Extraction facts:

- ChatGPT title: `EKOS 연구 지속`
- Visible text messages extracted: `389`
- Internal thoughts, reasoning recaps, tool-call JSON, and redacted plugin outputs were excluded.
- Secret scan found no OpenAI keys, GitHub tokens, AWS keys, private keys, or obvious password/token assignments.

Key imported reasoning from the share:

1. The workflow had already moved beyond the earlier question of whether configurable onboarding should exist. ChatGPT/Codex work indicates that North Star now has product documents for source package validation, user workflow, design-system journey, and action playbooks.
2. The EKOS implementation repository contains configured onboarding commands and configs, including:
   - `configs/onboarding/delivery_delay_confirmation.yaml`
   - `configs/onboarding/delivery_delay_confirmation_fragments.yaml`
   - `configs/onboarding/delivery_delay_confirmation_source_package.yaml`
   - `configs/onboarding/idoc_material_lock_reprocessing.yaml`
3. The UI/product track moved into `design-ontology-harness` on branch `codex/product-surface-grammar`, with a Korean operations UX prototype and subsequent prototype refinements.
4. The latest user critique in the imported conversation is not just visual design. It is a product-model problem: `조치 권고만 가능` is an authority boundary, not a concrete operational instruction.
5. The next product concept produced in the ChatGPT conversation is the **EKOS Action Playbook / Next Action Model**: EKOS must translate allowed/blocked decision boundaries into concrete next actions with owner, required input, completion condition, blocked transition, and next allowed transition.

## Current Cross-Repository Product Direction

After importing the ChatGPT share and fetching current repository state, the current frontier is no longer “create the first YAML configurable mapping prototype.” That appears to have been implemented upstream already.

Current verified state:

- North Star latest commits include:
  - `4e1e63e Define EKOS action playbook model`
  - `8c03b87 Define EKOS design system user journey plan`
  - `eab835f Define EKOS user workflow and deployment model`
- EKOS latest commits include:
  - `bc62c87 Add supplemental source package onboarding path`
  - `c398a3d Add minimum source package validation`
  - `c988ad7 Add SAP asset projection prototype`
  - `a4461df Add configured IDoc lock onboarding workflow`
- design-ontology-harness latest relevant commits on `codex/product-surface-grammar` include:
  - `6aa2961 Clarify EKOS action recommendation wording`
  - `46a1aa7 Deepen EKOS operations review prototype`
  - `2d2b175 Localize EKOS prototype for Korean operations UX`
  - `c3b6768 Build EKOS static workflow prototype`

Hermes verified the current EKOS configured onboarding command surface:

```bash
.venv/bin/python -m ekos data-onboard configured --help
```

and confirmed these configs exist:

```text
configs/onboarding/delivery_delay_confirmation.yaml
configs/onboarding/delivery_delay_confirmation_fragments.yaml
configs/onboarding/delivery_delay_confirmation_source_package.yaml
configs/onboarding/idoc_material_lock_reprocessing.yaml
```

Hermes also smoke-ran configured onboarding for delivery-delay and IDoc material-lock workflows into `/tmp/ekos-configured-smoke/*`; both emitted:

```text
decision_packet.json
evidence_objects.json
onboarding_report.md
```

Therefore the current highest-value direction is:

```text
Move from configured decision-packet generation to action-playbook resolution.

EKOS should not stop at allowed_action / blocked_action.
It should produce concrete next actions: owner, required input, source/screen,
completion condition, blocker resolved, and next allowed transition.
```

Reasoning:

- Configurable onboarding and multi-workflow packet generation now exist at least in synthetic/configured form.
- User-facing prototype feedback says the product still feels too vague when the result is only “조치 권고만 가능.”
- North Star already defines an Action Playbook model; the next implementation question is whether EKOS should materialize that model in configs, packet output, report output, and eventually the static prototype.
- This is still consistent with the earlier architecture inventory: no more scenario-specific hard-coded field expansion unless it serves a reusable configurable model.

## Action Playbook Implementation Update — 2026-07-08

The user chose the recommended path: implement Action Playbook / `next_actions` first in the EKOS implementation repository before refining the UI prototype.

EKOS implementation commit:

```text
f67e7d9 Add configured action playbook next actions
```

Implemented behavior:

- configured onboarding configs can define `action_playbook` entries;
- active evidence, blocking risks, and authority action category resolve matching playbook entries;
- `decision_packet.json` can include `next_actions`;
- `onboarding_report.md` renders a `## Next Actions` section;
- delivery-delay workflow emits:
  - `request_current_carrier_confirmation`
  - `verify_delay_owner`
- IDoc material-lock workflow emits:
  - `request_lock_clearance`
  - `check_incoming_idoc_queue`
  - `review_retry_threshold`

Verification after commit:

```text
736 passed, 2 skipped in 8.97s
```

A ChatGPT review packet was created at:

```text
docs/research/2026-07-08-chatgpt-review-packet-action-playbook-implementation.md
```

This packet asks ChatGPT to review whether `next_actions` solves the “그래서 뭘 하라는 건데?” product gap, whether the schema is sufficient, and whether the next step should be EKOS schema hardening, UI prototype integration, action-completion rerun fixtures, or a North Star ADR/product note.

## Immediate Next Actions

1. Share the ChatGPT review packet with ChatGPT web and import the response back into this workflow.
2. If the review does not identify a blocker, connect EKOS `next_actions` into the Korean UI prototype in `design-ontology-harness`.
3. Keep executable implementation in EKOS or `design-ontology-harness`; keep strategic decisions and review synthesis in North Star.

## Open Questions For User

1. Should Hermes make **Action Playbook / Next Action Model implementation** the next active task?
2. Should implementation begin in EKOS config/report output, or in the UI prototype first?
3. Should the imported ChatGPT transcript be committed as a source artifact, or kept uncommitted until you review it?
4. Should the old untracked `existing-sap-ingestion-inventory.md` and TypeScript-port review be reconciled with the newer upstream commits before committing anything?
