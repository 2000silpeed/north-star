# ChatGPT Review Packet — EKOS Action Playbook Implementation

Date: 2026-07-08
Status: Review packet for ChatGPT web
Source repositories:

- North Star: `/Users/sungwoon/ai-projects/north-star`
- EKOS implementation: `/Users/sungwoon/ai-projects/2000silpeed/ekos-sap-knowledge-os`
- UI prototype/design system: `/Users/sungwoon/ai-projects/design-ontology-harness`

## Why this packet exists

The user is running a three-party workflow:

```text
User + ChatGPT web + Hermes
```

ChatGPT previously identified a product gap:

```text
"조치 권고만 가능" is not a useful operational answer.
It is only an authority boundary.
The user still needs to know what to do next.
```

Hermes implemented the first EKOS-side step toward that model.

## Implemented EKOS change

Commit in EKOS:

```text
f67e7d9 Add configured action playbook next actions
```

Changed files:

```text
configs/onboarding/delivery_delay_confirmation.yaml
configs/onboarding/delivery_delay_confirmation_fragments.yaml
configs/onboarding/delivery_delay_confirmation_source_package.yaml
configs/onboarding/idoc_material_lock_reprocessing.yaml
ekos/runners/configured_onboarding.py
tests/test_configured_onboarding.py
```

## What changed conceptually

Before:

```text
configured source data
-> evidence objects
-> blocking risks
-> authority boundary
-> decision packet
-> report
```

After:

```text
configured source data
-> evidence objects
-> blocking risks
-> authority boundary
-> decision packet
-> action playbook resolution
-> next_actions
-> report with "Next Actions"
```

The decision packet can now include:

```json
"next_actions": [
  {
    "action_id": "request_current_carrier_confirmation",
    "action_label": "Request current carrier confirmation for Delivery 80001241",
    "owner_role": "logistics_operations_user",
    "required_inputs": [
      "Current carrier ETA",
      "Carrier confirmation timestamp",
      "Confirmation source or contact"
    ],
    "source_system_or_screen": "Carrier portal, 3PL email/phone log, or LE Tracking update screen",
    "completion_condition": "Carrier confirmation is current within the configured policy window and confirmation_status is recorded as confirmed.",
    "expected_result": "EKOS can re-run the configured onboarding check with fresh carrier confirmation.",
    "blocked_until_done": "Prepare approval-ready correction workflow",
    "next_allowed_transition": "Re-run Check Mode; if blocking risks clear, prepare correction workflow packet for approval.",
    "escalation_condition": "Escalate to logistics lead if the carrier cannot confirm ETA before the customer commitment cutoff.",
    "depends_on_evidence": [
      "carrier_update_stale",
      "policy_confirmation_required"
    ],
    "depends_on_blocking_risks": [
      "stale_external_event",
      "insufficient_confirmation_for_execution_plan"
    ]
  }
]
```

## Current smoke output

Delivery-delay workflow:

```text
delivery 2 ['request_current_carrier_confirmation', 'verify_delay_owner']
```

IDoc material-lock workflow:

```text
idoc 3 ['request_lock_clearance', 'check_incoming_idoc_queue', 'review_retry_threshold']
```

The report now includes a `## Next Actions` section. Example delivery action:

```text
### 1. Request current carrier confirmation for Delivery 80001241

- Owner: logistics_operations_user
- Source/system/screen: Carrier portal, 3PL email/phone log, or LE Tracking update screen
- Required inputs:
  - Current carrier ETA
  - Carrier confirmation timestamp
  - Confirmation source or contact
- Completion condition: Carrier confirmation is current within the configured policy window and confirmation_status is recorded as confirmed.
- Expected result: EKOS can re-run the configured onboarding check with fresh carrier confirmation.
- Next allowed transition: Re-run Check Mode; if blocking risks clear, prepare correction workflow packet for approval.
- Blocked until done: Prepare approval-ready correction workflow
- Escalation condition: Escalate to logistics lead if the carrier cannot confirm ETA before the customer commitment cutoff.
```

## Verification already run by Hermes

Focused configured onboarding tests:

```text
36 passed in 0.38s
```

Relevant test bundle:

```text
67 passed in 0.52s
```

Full EKOS test suite after commit:

```text
736 passed, 2 skipped in 8.97s
```

## Claim boundaries

This implementation:

- does not call providers;
- does not use unblind keys or original scorer artifacts;
- does not connect to live SAP;
- does not claim production readiness;
- does not approve execution;
- only converts configured evidence/risk/authority state into concrete next-action guidance.

## Review questions for ChatGPT

Please review this as a product/architecture reviewer, not as a code executor.

1. Does `next_actions` solve the user complaint that `조치 권고만 가능` is too vague?
2. Are the fields sufficient for a real operations-facing first version?
3. Is the split right: EKOS emits next actions, while UI prototype renders them?
4. Should `next_actions` live directly inside `decision_packet.json`, or should it be a separate `action_plan.json` artifact linked from the packet?
5. What should be the next implementation step?
   - A. improve EKOS action schema and validation;
   - B. connect `next_actions` into `design-ontology-harness` Korean prototype;
   - C. add a user-review fixture that simulates completing an action and re-running EKOS;
   - D. write North Star ADR/product note about Action Playbook as a formal layer.
6. What are the biggest risks of this design?
   - too much workflow-specific copy in config;
   - action guidance becoming fake operational authority;
   - UI overclaiming readiness;
   - missing role/source-system granularity;
   - inability to validate completion conditions.

## Desired response format

Please respond in Korean with:

```markdown
## Verdict

## What is strong

## What is weak or risky

## Recommended next step

## Specific changes to ask Hermes to make

## What not to do yet
```
