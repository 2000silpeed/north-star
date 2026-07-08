# EKOS Action Playbook Model

Status: Product architecture model
Repository role: North Star product strategy and UX architecture
Implementation repository: `2000silpeed/ekos-sap-knowledge-os`
Related documents:

- `docs/product/ekos-user-workflow-and-deployment-model.md`
- `docs/product/ekos-design-system-user-journey-plan.md`
- `docs/product/ekos-configurable-onboarding-layer.md`
- `docs/product/ekos-product-state-2026-07.md`

This document defines how EKOS should translate governed decision boundaries
into concrete next actions for enterprise users.

It does not implement code, modify EKOS, create a benchmark, claim production
readiness, claim live SAP integration, or claim autonomous approval.

## 1. Executive Summary

The current EKOS product model correctly separates allowed actions from blocked
actions. That boundary is necessary, but it is not enough.

When a report says:

```text
Allowed action: Recommendation only
```

or:

```text
현재 해도 되는 일: 조치 권고만 가능
```

the user still has to ask:

```text
그래서 뭘 하라는 건데?
```

That question reveals a product gap.

`allowed_action` is an authority boundary. It says how far EKOS may go. It does
not, by itself, tell the operator which concrete task to do next, who owns the
task, which input is missing, how completion is checked, or when the case may
move to the next state.

EKOS therefore needs an Action Playbook layer.

The Action Playbook should translate:

```text
evidence + blocking risk + policy + authority boundary
```

into:

```text
concrete next action + owner + required input + completion condition
+ blocker resolved + next allowed transition
```

This model should be configured per workflow. It should not be hard-coded per
screen.

## 2. Why `allowed_action` Is Not Enough

`allowed_action` answers the governance question:

```text
What may EKOS do right now?
```

It does not answer the operations question:

```text
What should the human operator do next?
```

For example, in the IDoc material lock workflow, a boundary such as:

```text
Action category: recommend
Allowed action: 조치 권고만 가능
Blocked action: Prepare approval-ready IDoc reprocessing plan
```

is correct as an authority statement. It prevents EKOS from preparing a
reprocessing plan while a lock or queue collision risk remains active.

But it does not tell the SAP operations user:

- who should check the lock;
- where to check it;
- what value proves the lock is cleared;
- whether batch reprocessing should be paused;
- when the case can be re-evaluated;
- what escalation path applies if retries exceed policy.

The same problem exists in the delivery-delay workflow. Saying that EKOS may
only recommend does not tell the logistics manager whether to call the carrier,
upload a confirmation extract, verify the delay owner, or ask the approval
matrix owner to update missing governance data.

The product lesson is:

```text
Authority boundary is not operational instruction.
```

EKOS needs both.

## 3. Action Playbook Definition

An EKOS Action Playbook is a workflow-specific catalog of next actions that can
be triggered by evidence, blocking risks, policy constraints, source package
failures, or review states.

It should answer six user-facing questions:

1. What is the conclusion?
2. What should be done now?
3. Who should do it?
4. What input is required?
5. How do we know it is complete?
6. What transition becomes possible after completion?

The playbook does not approve execution. It translates the governed decision
packet into concrete human work.

## 4. Relationship To Existing EKOS Concepts

The Action Playbook sits after decision packet construction and before or inside
the human-readable report.

```text
source package
-> evidence objects
-> blocking risks
-> authority boundary
-> decision packet
-> action playbook resolution
-> human-readable report
-> review workflow
```

The playbook should use existing EKOS artifacts rather than replace them.

| Existing concept | Role | Action Playbook relationship |
| --- | --- | --- |
| Evidence object | Explains what is known | Can trigger or justify an action. |
| Blocking risk | Explains what blocks preparation, approval, or execution | Can trigger a required mitigation action. |
| Policy constraint | Explains required preconditions | Defines completion conditions and escalation rules. |
| Authority boundary | Defines what EKOS may or may not do | Limits which actions can be offered. |
| Decision packet | Reviewable decision artifact | Supplies the input context for playbook resolution. |
| Governance status | Review state | Determines whether actions route to operations, manager review, or audit. |

## 5. Required Model Fields

Each playbook action should include the following fields.

| Field | Meaning |
| --- | --- |
| `action_id` | Stable machine-readable identifier for the action. |
| `action_label` | Business-facing label shown to the user. |
| `trigger_evidence` | Evidence IDs or evidence classes that activate the action. |
| `trigger_blocking_risk` | Blocking risks that require or prioritize the action. |
| `owner_role` | Role expected to perform or own the action. |
| `required_inputs` | Values, files, confirmations, notes, or source records needed to complete the action. |
| `source_system_or_screen` | System, extract, screen, queue, report, or manual artifact where the user should get or enter the input. |
| `completion_condition` | Observable condition that marks the action complete. |
| `expected_result` | What EKOS can do after the action is completed. |
| `blocked_until_done` | The action, transition, or authority level blocked until completion. |
| `next_allowed_transition` | The next EKOS state or product step that becomes available. |
| `escalation_condition` | When the action should be escalated to a manager, support role, audit/control role, or another team. |
| `audit_note` | What should be recorded for review and audit reconstruction. |

The field names should be stable. The labels and wording can be localized per
workflow and user role.

## 6. Configuration Principle

Action Playbooks should be configured per workflow.

They should not be hard-coded into one screen, one report template, or one
Python branch.

The same workflow configuration boundary already exists for source mapping,
evidence rules, policy constraints, blocking risks, authority boundary, and
report labels. Action Playbooks should extend that configurable model.

Conceptually, a workflow configuration should be able to define:

```text
workflow_id
source_aliases
evidence_rules
blocking_risks
authority_boundary
decision_packet_shape
report_labels
action_playbook
```

The Action Playbook should not make a new decision independently. It should
resolve actions from the decision packet state.

For example:

```text
carrier_update_stale
+ policy_confirmation_required
+ approval-preparation blocked
-> show "Request current carrier confirmation"
```

and:

```text
material_lock_active
+ incoming_idoc_queue_active
+ reprocessing plan preparation blocked
-> show "Pause batch reprocessing"
-> show "Check lock owner"
-> show "Check incoming IDoc queue"
```

The action list should be deterministic and reviewable. A reviewer should be
able to reconstruct why each action appeared.

## 7. Action Types

EKOS should distinguish between at least five action types.

| Action type | Meaning | Example |
| --- | --- | --- |
| `collect_input` | Obtain missing or stale input. | Request current carrier confirmation. |
| `verify_owner` | Confirm who owns the issue or next review. | Verify delay owner. |
| `pause_or_hold` | Stop preparation or execution while a blocker remains active. | Pause batch reprocessing. |
| `rerun_check` | Re-run validation or packet generation after new input arrives. | Re-run source package validation. |
| `escalate` | Route to a higher role or support owner when a policy condition is exceeded. | Escalate if retry count exceeds policy. |

These action types should not grant approval. They only tell the user what
work is needed before EKOS may move to the next state.

## 8. Delivery Delay Action Playbook

Workflow:

```text
Delivery Delay / Carrier Confirmation
```

Current typical boundary:

```text
Allowed boundary: 조치 권고만 가능
Blocked action: Prepare approval-ready correction workflow
Governance status: review_required
```

The report should not stop there. It should show concrete actions.

### A. Request Current Carrier Confirmation

| Field | Value |
| --- | --- |
| `action_id` | `request_current_carrier_confirmation` |
| `action_label` | Request current carrier confirmation / 운송사 최신 확인 요청 |
| `trigger_evidence` | `carrier_update_stale`, `carrier_confirmation_missing` |
| `trigger_blocking_risk` | `stale_external_event`, `insufficient_confirmation_for_execution_plan` |
| `owner_role` | 3PL coordinator or logistics operations user |
| `required_inputs` | `delivery_id`, `carrier_id`, latest ETA, confirmation timestamp, confirmation status, update source |
| `source_system_or_screen` | LE tracking table, carrier portal extract, 3PL update file, or governed manual carrier update upload |
| `completion_condition` | A carrier confirmation record exists within the configured confirmation window and is tied to the delivery. |
| `expected_result` | EKOS can determine whether carrier confirmation is current. |
| `blocked_until_done` | Approval-ready correction workflow preparation remains blocked. |
| `next_allowed_transition` | Re-run source package validation; then re-run decision packet generation. |
| `escalation_condition` | No carrier response within the operations SLA or repeated conflicting carrier ETA updates. |
| `audit_note` | Record who confirmed the update, when it was confirmed, and which source supplied the ETA. |

### B. Verify Delay Owner

| Field | Value |
| --- | --- |
| `action_id` | `verify_delay_owner` |
| `action_label` | Verify delay owner / 지연 담당자 확인 |
| `trigger_evidence` | `delay_owner_unverified` or missing delay owner field |
| `trigger_blocking_risk` | `unverified_delay_owner` |
| `owner_role` | Logistics operations lead |
| `required_inputs` | delay owner, responsible team, owner verification timestamp, reviewer note if manual |
| `source_system_or_screen` | delivery exception worklist, operations tracker, team assignment table, or governed manual review note |
| `completion_condition` | Delay owner and responsible team are recorded and reviewable. |
| `expected_result` | EKOS can route the next review or data request to a responsible owner. |
| `blocked_until_done` | Approval-preparation and escalation routing remain blocked or degraded. |
| `next_allowed_transition` | Route the packet to the correct reviewer or re-run decision packet generation with owner evidence. |
| `escalation_condition` | Delay ownership is disputed, unknown, or assigned to the wrong operational team. |
| `audit_note` | Record the owner assignment source and whether it was system-derived or manually confirmed. |

### C. Check Approval Matrix Owner

| Field | Value |
| --- | --- |
| `action_id` | `check_approval_matrix_owner` |
| `action_label` | Check approval matrix owner / 승인 기준표 담당자 확인 |
| `trigger_evidence` | missing or outdated approval matrix evidence |
| `trigger_blocking_risk` | `missing_approval_role`, missing authority mapping |
| `owner_role` | Operations lead, workflow owner, or audit/control owner |
| `required_inputs` | workflow ID, requested action, required approver role, approval matrix version, effective dates |
| `source_system_or_screen` | governed approval matrix file, workflow customizing export, SOP table, GRC role mapping, or governance-maintained CSV |
| `completion_condition` | A valid approval matrix row exists for the workflow and requested action. |
| `expected_result` | EKOS can show who may review or approve preparation. |
| `blocked_until_done` | Approval-preparation routing remains blocked. |
| `next_allowed_transition` | Re-run source package validation and route to the mapped reviewer. |
| `escalation_condition` | Approval ownership conflicts across SOP, workflow customizing, and team-level matrix. |
| `audit_note` | Record the approval matrix version and owner who supplied or validated it. |

### D. Re-run Source Package Validation

| Field | Value |
| --- | --- |
| `action_id` | `rerun_source_package_validation` |
| `action_label` | Re-run source package validation / 필요 데이터 다시 확인 |
| `trigger_evidence` | new carrier update, new policy mapping, updated approval matrix, verified delay owner |
| `trigger_blocking_risk` | any previously active source package blocker |
| `owner_role` | SAP operations user or EKOS analyst |
| `required_inputs` | updated source package or supplemental source files |
| `source_system_or_screen` | EKOS source package status screen |
| `completion_condition` | Required aliases pass the workflow minimum source package policy. |
| `expected_result` | EKOS may proceed to decision packet generation. |
| `blocked_until_done` | Decision packet generation should not proceed from incomplete source data. |
| `next_allowed_transition` | Generate or refresh decision packet. |
| `escalation_condition` | Required aliases repeatedly fail because source ownership is unclear. |
| `audit_note` | Record validation result, missing aliases, and timestamp of re-validation. |

### E. Re-run Decision Packet Generation

| Field | Value |
| --- | --- |
| `action_id` | `rerun_decision_packet_generation` |
| `action_label` | Re-run decision packet generation / 판단 결과 다시 생성 |
| `trigger_evidence` | source package passed after updated inputs |
| `trigger_blocking_risk` | blocker state may have changed |
| `owner_role` | SAP operations user or EKOS analyst |
| `required_inputs` | validated source package, workflow config version, policy version, approval matrix version |
| `source_system_or_screen` | EKOS decision workflow screen |
| `completion_condition` | A new decision packet is generated with updated evidence and provenance. |
| `expected_result` | EKOS updates allowed action, blocked action, next actions, and review state. |
| `blocked_until_done` | The old packet should not be treated as current after source data changes. |
| `next_allowed_transition` | Review updated decision report. |
| `escalation_condition` | Packet result conflicts with operations expectation or policy owner interpretation. |
| `audit_note` | Record previous packet ID, new packet ID, changed evidence, and changed blocker state. |

## 9. IDoc Material Lock Action Playbook

Workflow:

```text
IDoc Material Lock / Reprocessing Collision Readiness
```

Current typical boundary:

```text
Allowed boundary: 조치 권고만 가능
Blocked action: Prepare approval-ready IDoc reprocessing plan
Governance status: review_required
```

This boundary should be translated into operational next actions.

### A. Check Lock Owner

| Field | Value |
| --- | --- |
| `action_id` | `check_lock_owner` |
| `action_label` | Check lock owner / Lock 소유자 확인 |
| `trigger_evidence` | `material_lock_active` |
| `trigger_blocking_risk` | `active_material_lock`, `policy_precondition_unmet` |
| `owner_role` | SAP interface operations user or SAP Basis support |
| `required_inputs` | object key, material, plant, lock owner, lock start time, lock reason |
| `source_system_or_screen` | lock monitor, SM12-like export, interface operations dashboard, or governed lock event extract |
| `completion_condition` | Lock owner and lock reason are known and recorded. |
| `expected_result` | EKOS can route the blocker to the correct owner and determine whether waiting, escalation, or manual intervention is required. |
| `blocked_until_done` | Approval-ready reprocessing plan remains blocked. |
| `next_allowed_transition` | Wait for lock clearance or escalate to the lock owner. |
| `escalation_condition` | Lock owner is unknown, lock age exceeds policy, or lock reason indicates a stuck update. |
| `audit_note` | Record lock owner, source system, timestamp, and whether the lock is expected or abnormal. |

### B. Wait For Lock Clearance

| Field | Value |
| --- | --- |
| `action_id` | `wait_for_lock_clearance` |
| `action_label` | Wait for lock clearance / Lock 해제 대기 |
| `trigger_evidence` | `material_lock_active`, `lock_age_exceeds_policy` if active |
| `trigger_blocking_risk` | `active_material_lock`, `unsafe_reprocessing_collision` |
| `owner_role` | SAP interface operations user |
| `required_inputs` | current lock status, lock cleared timestamp, confirmation that no conflicting update remains active |
| `source_system_or_screen` | lock monitor or refreshed lock event extract |
| `completion_condition` | The material/plant/object lock is no longer active at the decision time. |
| `expected_result` | EKOS can remove or downgrade the active lock blocker if no other blocker remains. |
| `blocked_until_done` | Reprocessing plan preparation remains blocked. |
| `next_allowed_transition` | Re-run source package validation and decision packet generation. |
| `escalation_condition` | Lock remains active beyond policy threshold or blocks multiple retry cycles. |
| `audit_note` | Record lock cleared timestamp and source that proved clearance. |

### C. Pause Batch Reprocessing

| Field | Value |
| --- | --- |
| `action_id` | `pause_batch_reprocessing` |
| `action_label` | Pause batch reprocessing / 배치 재처리 보류 |
| `trigger_evidence` | `material_lock_active`, `incoming_idoc_queue_active`, `retry_count_high` |
| `trigger_blocking_risk` | `unsafe_reprocessing_collision`, `retry_storm_risk` |
| `owner_role` | SAP interface operations lead |
| `required_inputs` | target object key, affected batch job ID, retry batch ID, current job status |
| `source_system_or_screen` | reprocessing job monitor, IDoc operations dashboard, batch job scheduler |
| `completion_condition` | The reprocessing job is paused, not scheduled, or explicitly held from approval-preparation while blockers remain active. |
| `expected_result` | EKOS prevents the case from being treated as approval-ready while collision risk remains active. |
| `blocked_until_done` | Approval-ready reprocessing plan and execution remain blocked. |
| `next_allowed_transition` | Check incoming queue and lock clearance before any reprocessing plan preparation. |
| `escalation_condition` | A reprocessing job is already running or cannot be paused by the operations user. |
| `audit_note` | Record whether the job was paused, who paused it, and which job or retry batch was affected. |

### D. Check Incoming IDoc Queue

| Field | Value |
| --- | --- |
| `action_id` | `check_incoming_idoc_queue` |
| `action_label` | Check incoming IDoc queue / 신규 IDoc Queue 확인 |
| `trigger_evidence` | `incoming_idoc_queue_active` |
| `trigger_blocking_risk` | `unsafe_reprocessing_collision` |
| `owner_role` | SAP interface operations user |
| `required_inputs` | object key, material, plant, incoming count, newest IDoc created time, queue status |
| `source_system_or_screen` | IDoc queue monitor, interface monitoring export, queue status CSV |
| `completion_condition` | Queue is empty, inactive, or confirmed safe for the target object and timeframe. |
| `expected_result` | EKOS can determine whether manual or batch reprocessing would collide with new inbound processing. |
| `blocked_until_done` | Reprocessing plan preparation remains blocked while queue risk is active. |
| `next_allowed_transition` | Re-run decision packet generation after queue status changes. |
| `escalation_condition` | Incoming queue remains active while retry count or SLA pressure increases. |
| `audit_note` | Record queue count, newest IDoc timestamp, source, and check time. |

### E. Escalate If Retry Count Exceeds Policy

| Field | Value |
| --- | --- |
| `action_id` | `escalate_retry_count_exceeds_policy` |
| `action_label` | Escalate if retry count exceeds policy / 재처리 횟수 초과 시 에스컬레이션 |
| `trigger_evidence` | `retry_count_high` |
| `trigger_blocking_risk` | `retry_storm_risk` |
| `owner_role` | SAP interface operations lead or integration support owner |
| `required_inputs` | IDoc number, retry count, max safe retry count, last processed time, status text |
| `source_system_or_screen` | IDoc monitor, reprocessing job history, policy table |
| `completion_condition` | The responsible lead has reviewed the retry state and selected hold, manual correction, or governed reprocessing preparation. |
| `expected_result` | EKOS prevents repeated blind retries and routes the case to the correct support owner. |
| `blocked_until_done` | Further reprocessing preparation should not proceed. |
| `next_allowed_transition` | Human review decides whether to request correction, continue hold, or allow preparation after blockers clear. |
| `escalation_condition` | Retry count exceeds policy, or failure repeats after a confirmed safe retry. |
| `audit_note` | Record policy threshold, observed retry count, owner decision, and review note. |

## 10. UI Requirement

The Decision Report must not stop at:

```text
현재 해도 되는 일: 조치 권고만 가능
```

It must show the concrete action plan.

Every decision report should include, in this order:

1. `결론`
2. `지금 할 일`
3. `누가 해야 하나`
4. `필요한 값`
5. `완료 조건`
6. `다음 단계로 넘어가는 조건`

The first screen should answer:

```text
What should I do now?
```

not only:

```text
What is EKOS allowed to do?
```

### Recommended UI Shape

The report should show a compact action table or action cards.

| 지금 할 일 | 담당 | 필요한 값 | 완료 조건 | 다음 단계 |
| --- | --- | --- | --- | --- |
| 운송사 최신 확인 요청 | 3PL / 물류 운영 | ETA, 확인 시각, 출처 | 확인 기준 시간 안의 carrier update 존재 | 필요 데이터 재검토 |
| 지연 담당자 확인 | 물류 운영 리드 | delay owner, responsible team | 담당자와 팀이 기록됨 | 검토 요청 또는 판단 재생성 |
| Lock 소유자 확인 | SAP 인터페이스 운영 / Basis | lock owner, lock start time, reason | Lock 담당과 사유가 기록됨 | Lock 해제 대기 또는 에스컬레이션 |
| 신규 IDoc Queue 확인 | SAP 인터페이스 운영 | incoming count, queue status | Queue가 비활성 또는 안전 상태로 확인됨 | 판단 결과 재생성 |

The action table should not look like approval. It should be visually distinct
from any approval or execution control.

## 11. Review And Audit Requirements

Every action shown to a user should be reviewable.

For audit and control users, EKOS should preserve:

- which evidence triggered the action;
- which blocking risk required it;
- which policy or approval boundary made it necessary;
- who owned the action;
- what input was supplied;
- when completion was recorded;
- which blocker was resolved or remained active;
- which transition became available afterward.

Without this trace, Action Playbooks become another opaque checklist. The value
of EKOS is not simply presenting a checklist. It is presenting a checklist whose
presence, priority, and completion condition are grounded in evidence, policy,
authority, and governance state.

## 12. What Should Not Happen

EKOS should not:

- show only `Recommendation only` or `조치 권고만 가능` and stop there;
- say that it "recommends a risk";
- imply that a warning is an operational instruction;
- fabricate an approval from completed playbook actions;
- allow playbook completion to bypass human review when review is required;
- hard-code playbook actions into one report screen;
- let RAG-generated text create next actions without EKOS grounding;
- hide owner, input, completion condition, or transition state.

The key wording discipline is:

```text
EKOS does not recommend that a risk exists.
EKOS identifies a risk and recommends the safe next action.
```

## 13. Product Architecture Implication

The Action Playbook becomes a product layer between:

```text
authority boundary
```

and:

```text
human task execution
```

It should be configurable, versioned, and audited like evidence rules and policy
constraints.

This also clarifies the EKOS v1 product story:

```text
EKOS does not execute enterprise work.
EKOS tells the enterprise what is currently allowed, what is blocked, why, and
which concrete human actions can resolve the blocker.
```

## 14. Open Questions

Questions that remain open:

- Should Action Playbook completion be recorded inside the decision packet, or
  as a linked review/work item artifact?
- Should playbook actions have priority order, dependency order, or both?
- How should EKOS distinguish required actions from optional context-improving
  actions?
- Who owns playbook configuration: process owner, SAP consultant, governance
  owner, or EKOS platform owner?
- How should playbook actions be localized without changing their stable
  machine-readable IDs?
- Can action completion automatically trigger source package validation, or
  should the user always explicitly re-run validation?
- What is the minimum audit record needed for action completion in v1?

## 15. Claim Boundary

This is a product architecture model.

It does not implement Action Playbooks.

It does not claim production readiness.

It does not claim live SAP integration.

It does not claim autonomous approval.

It does not claim that completed next actions are equivalent to human approval.

It only defines why EKOS needs a configurable next-action model and what fields
the model should contain.
