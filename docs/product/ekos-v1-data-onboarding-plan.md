# EKOS v1 Data Onboarding Plan

Status: Product onboarding plan
Repository role: North Star product strategy and data architecture
Implementation repository: `2000silpeed/ekos-sap-knowledge-os`
Reference issue: North Star Issue #4, "Product Gap: real enterprise data onboarding for EKOS decision packets"

This document defines the minimum v1 data onboarding plan for one narrow SAP
logistics slice:

```text
Delivery delay / carrier confirmation / correction workflow readiness
```

It is not an implementation plan, benchmark, demo, ingestion proof, or claim
that EKOS already performs this onboarding in production. It defines the product
contract that real enterprise data would need to satisfy before EKOS can create
decision-grade packets for the existing enterprise workflow demo.

## 1. Product Goal

The product goal is to turn imperfect SAP logistics data into a governed
decision packet that can answer one operational question:

```text
Can EKOS safely recommend a likely carrier-delay classification and determine
whether a correction workflow is ready to be prepared?
```

For v1, success does not mean fully automated logistics execution. Success means
EKOS can assemble a reviewable packet that shows:

- the primary delivery or DO being evaluated
- the related carrier update or tracking event
- whether the external carrier confirmation is current or stale
- which policy controls correction workflow readiness
- which execution preconditions are met or unmet
- which delegation level is safe
- what remains blocked until a human or carrier provides missing confirmation

The business value is upstream of the current demo. The existing enterprise
workflow demo shows the value chain once structured artifacts exist. This plan
defines how those artifacts should be onboarded from realistic SAP/logistics
sources without assuming clean data.

## 2. Scope Boundary

In scope for v1:

- outbound delivery or DO delay review
- carrier confirmation freshness
- carrier tracking event history
- custom CBO tracking tables or event history tables
- DO-to-FO or DO-to-FU relationship when available and relevant
- policy requiring current external confirmation before correction workflow
  preparation
- delay owner verification as an execution precondition
- decision packet creation for recommendation and human review
- CSV, OData, RFC, and manual upload fallback paths

Out of scope for v1:

- live production SAP writes
- automatic customer commitment changes
- automatic carrier blame assignment
- automatic creation, staging, or submission of a correction workflow
- full SAP TM implementation coverage
- full policy-document understanding
- broad logistics control-tower replacement
- production approval or authorization
- proof that ingestion has already been implemented

The v1 slice is deliberately narrow. EKOS should not attempt to onboard all
logistics data before it can demonstrate value. It should first make one
repeatable decision packet trustworthy.

## 3. Minimum Source Systems

The minimum source systems are the smallest set needed to construct the
Scenario 03-style decision packet from real enterprise artifacts.

| Source | Minimum role in v1 | Acceptable v1 access path |
| --- | --- | --- |
| SAP delivery management | Delivery or DO identity, status, planned handoff, customer/plant/shipping context | SAP CDS/OData, RFC/BAPI extract, table/report export, CSV upload |
| SAP document flow or relationship extract | Delivery-to-sales-order, delivery-to-shipment, and optional DO-to-FO/FU mapping | OData/CDS, RFC, standard report export, manual relationship CSV |
| SAP TM or custom logistics mapping | FO/FU relationship, carrier assignment, pickup/arrival plan when relevant | OData/CDS, TM extract, custom view, manual upload |
| Custom CBO tracking tables | Carrier update records, ETA, event source, event receipt timestamp, tracking status | OData/custom API, table extract, CSV |
| Event history | Timeline of delivery status changes, carrier updates, internal corrections, retries | SAP change/event log, CBO event history, export file |
| Policy and SOP artifacts | Confirmation-window rule and correction-readiness constraint | Manually normalized policy table plus document reference |
| Approval or responsibility matrix | Delay owner, reviewer, approver, escalation owner | HR/role extract, workflow config export, manual matrix |
| Manual confirmation record | Human-entered current carrier confirmation or delay owner verification when systems are incomplete | Controlled upload form, CSV, reviewer note with source reference |

No source is assumed to be perfect. If a source is missing or stale, EKOS should
record that condition as a packet constraint instead of fabricating completeness.

## 4. Minimum SAP/Custom Data Fields

The v1 field set should be source-neutral enough to work across OData, RFC,
custom CBO exports, and manual CSV uploads. SAP table or API names may vary by
customer landscape, so the product contract should define canonical fields
rather than require a single extraction technology.

### Delivery / DO

Required fields:

- `delivery_id`
- `source_system`
- `source_client_or_tenant`
- `delivery_status`
- `shipping_point`
- `plant`
- `ship_to_party`
- `planned_handoff_at`
- `actual_handoff_at`, nullable
- `planned_delivery_at` or customer commitment reference, nullable
- `last_internal_status_at`
- `source_record_ref`

Useful optional fields:

- sales order ID
- delivery item IDs
- route
- incoterms or service level
- priority or escalation flag
- current exception ID
- responsible logistics team

### DO-FO/FU Relationship

Required when the delay decision depends on transportation planning context:

- `delivery_id`
- `relationship_type`, for example `DO_TO_FO`, `DO_TO_FU`, or equivalent
- `related_transport_object_id`
- `related_transport_object_type`
- `carrier_id`, nullable
- `relationship_status`
- `relationship_observed_at`
- `source_record_ref`

If no reliable DO-FO/FU relationship is available, the packet must mark the
relationship as missing or unverified. It must not infer the relationship only
from similar text or carrier names.

### Carrier Update / Tracking Event

Required fields:

- `carrier_update_id`
- `carrier_id`
- `delivery_id` or related tracking object ID
- `event_type`
- `event_timestamp`
- `event_received_at`
- `eta_at`, nullable
- `event_status`
- `source_channel`, for example carrier API, portal export, EDI, email, manual
  confirmation, or CBO
- `raw_payload_ref` or upload reference

Useful optional fields:

- location
- event description
- reason code
- carrier reference number
- external tracking number
- confirmation flag
- confirmed_by
- confirmed_at

### Custom CBO Event History

Required fields:

- `event_history_id`
- `object_type`
- `object_id`
- `event_name`
- `previous_value`, nullable
- `new_value`, nullable
- `event_at`
- `recorded_at`
- `recorded_by_or_source`
- `source_record_ref`

This history is needed because a current status row alone is not enough. EKOS
must know whether the stale carrier update was the latest external signal, a
superseded event, or an artifact of delayed ingestion.

### Policy / Approval Matrix

Required fields:

- `policy_id`
- `policy_version`
- `policy_source_ref`
- `effective_from`
- `effective_to`, nullable
- `applies_to_object_type`
- `condition`
- `confirmation_window`
- `required_evidence`
- `allowed_action_when_met`
- `blocked_action_when_unmet`
- `required_owner_verification`
- `required_approver_role`, nullable
- `policy_owner`
- `human_review_required`

Policy records may be manually normalized in v1. The source document or SOP
reference must remain attached so a reviewer can inspect the policy basis.

## 5. Object Model

The v1 object model should be small and explicit.

| Object | Role in the decision packet |
| --- | --- |
| `Delivery` / `DO` | Primary object being evaluated. |
| `TransportObject` | Freight Order, Freight Unit, shipment, or customer-specific transport object linked to the delivery when available. |
| `CarrierUpdate` | External or manually confirmed carrier signal that may support or block correction readiness. |
| `TrackingEvent` | Time-bound event in the carrier or CBO event stream. |
| `CustomerCommitment` | Commitment affected by the delay, included when available. |
| `PolicyConstraint` | Rule that determines whether preparation, approval, or execution is allowed. |
| `EvidenceRecord` | Source-backed support or blocker for a claim. |
| `AuthorityBoundary` | Allowed and blocked action categories under current evidence and policy. |
| `CorrectionWorkflowReadiness` | Readiness state for preparing a correction workflow. |
| `ReviewRecord` | Human review, verification, or governance state. |

Minimum relationships:

```text
Delivery -> TransportObject
TransportObject -> CarrierUpdate
CarrierUpdate -> TrackingEvent
Delivery -> CustomerCommitment
PolicyConstraint -> CorrectionWorkflowReadiness
EvidenceRecord -> Delivery
EvidenceRecord -> CarrierUpdate
EvidenceRecord -> PolicyConstraint
AuthorityBoundary -> CorrectionWorkflowReadiness
ReviewRecord -> DecisionPacket
```

The relationship model should allow missing edges. A missing DO-FO/FU edge is
not a fatal system error by itself; it is a data quality condition that limits
safe delegation.

## 6. Evidence Model

Evidence in this slice is not a citation string. It is a source-backed object
that supports, weakens, or blocks a decision.

Minimum evidence fields:

- `evidence_id`
- `evidence_type`
- `supports_claim`, `weakens_claim`, or `blocks_action`
- `source_system`
- `source_record_ref`
- `business_object_refs`
- `event_timestamp`, nullable
- `recorded_at`
- `loaded_at`
- `freshness_state`
- `verification_state`
- `raw_reference_available`
- `review_notes`, nullable

Required v1 evidence types:

| Evidence type | Example packet meaning |
| --- | --- |
| `delivery_status_event` | Delivery is delayed after planned handoff. |
| `carrier_update_stale` | Latest external carrier ETA or update is older than the confirmation window. |
| `carrier_update_current` | Latest external carrier confirmation is within the confirmation window. |
| `delay_owner_verification_missing` | Delay owner has not verified the delay owner or next responsible party. |
| `delay_owner_verified` | Responsible human has verified the delay owner. |
| `policy_confirmation_required` | Policy requires current external confirmation before correction preparation. |
| `do_transport_relationship_unverified` | DO-to-FO/FU relationship is missing, ambiguous, or manually unresolved. |

The model must preserve negative evidence. Missing current confirmation is not
empty data; it is evidence that Level 2 preparation is blocked.

## 7. Policy Model

The v1 policy model should cover only the rule needed for this slice. It should
not attempt full enterprise policy automation.

Minimum normalized policy:

```text
Policy: Delay corrections require current external confirmation.
Applies when: a delivery/DO delay correction workflow would be prepared,
approved, staged, or executed.
Required evidence: current external carrier confirmation inside the configured
confirmation window.
Additional precondition: delay owner verification.
Allowed when unmet: Level 1 recommendation, classification suggestion, and
request for current confirmation.
Blocked when unmet: Level 2 approval-ready correction workflow preparation,
staging, or execution.
Human governed: policy interpretation, override, approval, and production
execution.
```

Policy source handling in v1:

- A policy can start as an SOP paragraph, approval matrix row, or operations
  rule.
- A human owner must normalize the policy into the minimum fields listed above.
- EKOS may apply the normalized constraint, but must keep the source reference.
- If the source policy conflicts with the normalized policy, the packet must
  require human review.
- If policy version or effective date is unknown, EKOS must not grant Level 2.

## 8. Freshness Rules

Freshness should be evaluated at the decision time, not at file load time alone.

Minimum freshness states:

- `current`
- `stale`
- `missing`
- `conflicting`
- `unknown`

Carrier confirmation freshness rule:

```text
latest_external_confirmation_at + policy.confirmation_window >= decision_time
```

If true, the confirmation may be considered current. If false, it is stale.

Important distinctions:

- `event_timestamp` is when the carrier says the event happened.
- `event_received_at` is when the enterprise received the carrier update.
- `loaded_at` is when EKOS imported or received the record.
- A freshly loaded old event is still stale for confirmation purposes.
- A current internal SAP status does not replace the required current external
  carrier confirmation if policy specifically requires the external signal.
- A manual confirmation can satisfy freshness only if it records who confirmed,
  when, from what source, and which carrier signal it confirms.

If the confirmation window is missing from policy, v1 should default to
`unknown` and require human governance rather than inventing a window.

## 9. Authority/Delegation Mapping

The onboarding plan must preserve the distinction between data access and
business authority.

Minimum v1 delegation levels for this slice:

| Level | Allowed meaning | Example |
| --- | --- | --- |
| 0 | Read-only status summary | Summarize that Delivery 80001241 is delayed. |
| 1 | Recommendation or information request | Recommend likely carrier-delay classification and request current confirmation. |
| 2 | Prepare approval-ready correction workflow | Draft or stage a correction workflow package for approval. |
| 3 | Execute or submit correction | Change operational records, customer commitments, carrier assignments, or workflow state. |

Scenario 03 maps to:

```text
requested_delegation_level = 2
maximum_safe_delegation_level = 1
```

Authority rules:

- Level 1 is allowed when the primary delivery is known and the answer can
  explain the missing confirmation and next human action.
- Level 2 is allowed only when current external confirmation exists, delay
  owner verification is complete, policy version is known, and no blocking data
  quality condition exists.
- Level 3 is out of scope for v1 automation.
- Tool or table access must not be treated as permission to prepare or execute
  a correction workflow.
- Human override must create a review record and retain the evidence gap it
  overrides.

## 10. Decision Packet Schema

The decision packet is the product handoff from data onboarding into the EKOS
enterprise workflow.

Minimum schema:

```json
{
  "packet_id": "string",
  "created_at": "timestamp",
  "decision_time": "timestamp",
  "source_snapshot_id": "string",
  "primary_object": {
    "object_type": "Delivery",
    "object_id": "string",
    "source_system": "string",
    "source_record_ref": "string"
  },
  "related_objects": [
    {
      "object_type": "FreightOrder|FreightUnit|Shipment|CustomerCommitment",
      "object_id": "string",
      "relationship_type": "string",
      "verification_state": "verified|unverified|missing"
    }
  ],
  "requested_action": "string",
  "requested_delegation_level": 2,
  "evidence": [
    {
      "evidence_id": "string",
      "evidence_type": "string",
      "source_record_ref": "string",
      "freshness_state": "current|stale|missing|conflicting|unknown",
      "supports_or_blocks": "string"
    }
  ],
  "policies": [
    {
      "policy_id": "string",
      "policy_version": "string",
      "policy_source_ref": "string",
      "constraint": "string"
    }
  ],
  "execution_preconditions": [
    {
      "precondition_id": "string",
      "status": "met|unmet|unknown",
      "required_for_level": 2
    }
  ],
  "blocking_risks": [
    {
      "risk_id": "string",
      "reason": "string",
      "blocks_level": 2
    }
  ],
  "authority_boundary": {
    "maximum_safe_delegation_level": 1,
    "action_category": "recommend",
    "allowed_now": ["recommend", "request_confirmation"],
    "blocked_now": ["prepare_for_approval", "stage_workflow", "execute"],
    "required_approver_role": null
  },
  "review_state": {
    "ready_for_human_review": true,
    "requires_manual_fallback": false,
    "human_review_required": true
  },
  "claim_boundary": "Decision-support packet only; not production approval."
}
```

For the existing Scenario 03 demo, this packet should be able to materialize:

```text
primary_object = Delivery 80001241
evidence_ids = delivery_status_event, carrier_update_stale
policy_ids = delay_confirmation_policy
execution_preconditions = obtain current carrier confirmation, verify delay owner
blocking_risks = stale_external_event, insufficient_confirmation_for_execution_plan
requested_delegation_level = 2
maximum_safe_delegation_level = 1
action_category = recommend
```

## 11. Data Quality Checks

Minimum data quality checks before a packet can be marked review-ready:

| Check | Failure effect |
| --- | --- |
| Primary delivery identity exists and is unique | Packet cannot proceed beyond manual fallback. |
| Delivery source reference is retained | Packet cannot be review-ready. |
| Status timestamp exists | Freshness and delay reasoning become unknown. |
| Carrier event timestamp and received timestamp are separated | Freshness cannot be trusted. |
| Latest external carrier update can be identified | Confirmation state becomes unknown or stale. |
| Policy version and effective date are known | Level 2 must remain blocked. |
| Confirmation window is known | Current/stale classification requires human review. |
| Required evidence source references exist | Packet cannot claim evidence traceability. |
| DO-FO/FU relationship is verified when used | Relationship-based reasoning must be marked unverified. |
| Event history ordering is coherent | Packet must flag conflict and limit safe delegation. |
| Customer/plant/carrier identifiers do not conflict across sources | Packet must require manual review. |
| Manual confirmation includes person, time, and source | Manual confirmation cannot satisfy freshness. |
| No unresolved policy conflict exists | Packet cannot grant Level 2. |

Data quality failures should not be hidden. They should become explicit packet
conditions, blocking risks, or manual fallback tasks.

## 12. Manual Fallback Process

Manual fallback is a first-class v1 requirement, not an exception path.

Fallback triggers:

- SAP access is unavailable and only CSV/report extracts exist.
- DO-FO/FU relationship cannot be resolved automatically.
- Carrier portal/API data is stale, missing, or delayed.
- Policy exists only as SOP text or an approval matrix spreadsheet.
- Confirmation window is ambiguous.
- Carrier event history conflicts with internal SAP status.
- Delay owner is not machine-resolvable.

Fallback process:

1. Operations uploads or references the delivery extract, event history, and
   carrier update source.
2. EKOS records source references and marks unverified fields explicitly.
3. A responsible user confirms or rejects object relationships.
4. A policy owner maps the relevant SOP or approval matrix row into the v1
   policy fields.
5. The carrier or responsible operations user provides current confirmation,
   including source, timestamp, and verifier.
6. EKOS updates the packet state from `missing`, `stale`, or `unknown` only
   when the fallback evidence is source-backed.
7. If any required fallback step remains incomplete, EKOS limits the packet to
   Level 1 recommendation and blocks Level 2 preparation.

Manual fallback must be auditable. A note such as "confirmed by team" is not
enough unless it identifies who confirmed, when, based on what source, and which
object it applies to.

## 13. What Can Be Automated In v1

The following can be automated in v1 when the required source data is available:

- importing delivery, CBO tracking, event-history, policy, and role-matrix
  extracts
- normalizing source rows into canonical object and evidence fields
- resolving exact delivery IDs across uploaded sources
- linking delivery to FO/FU or shipment records when deterministic keys exist
- selecting the latest external carrier update for a delivery or transport
  object
- computing `current`, `stale`, `missing`, `conflicting`, or `unknown`
  freshness state
- checking whether current external confirmation exists
- checking whether delay owner verification exists
- applying the normalized `delay_confirmation_policy`
- creating blocking risks such as `stale_external_event` and
  `insufficient_confirmation_for_execution_plan`
- assembling the decision packet
- producing a reviewable explanation skeleton that ties evidence, policy,
  preconditions, and authority boundary together

Automation should stop at decision support. In v1, automation may prepare the
packet and recommended safe boundary, but it should not execute business action.

## 14. What Must Remain Human-Governed In v1

The following must remain human-governed in v1:

- normalizing or approving policy constraints from SOPs and approval matrices
- resolving ambiguous DO-FO/FU relationships
- confirming the true delay owner when the source data is unclear
- accepting manual carrier confirmation as a substitute for system data
- overriding a freshness or policy block
- deciding customer-facing communication or commitment changes
- preparing, staging, submitting, or executing a correction workflow
- approving any production action in SAP, TM, workflow, or customer systems
- resolving conflicting evidence between SAP, carrier systems, and manual notes
- determining whether reversibility is acceptable for a real correction action

This boundary is central to EKOS v1. The product may make the safe answer
reviewable, but it does not replace business authority.

## 15. Connection To The Existing Enterprise Workflow Demo

The existing enterprise workflow demo starts after the decision packet already
exists.

Current demo chain:

```text
Scenario
-> Decision
-> Enterprise Explanation
-> Explanation Quality Check
-> Enterprise Answer
-> Human Review
-> Final Approved Decision
```

This onboarding plan defines the missing upstream chain:

```text
Messy SAP/logistics data
-> source references
-> object resolution
-> evidence normalization
-> policy constraint mapping
-> freshness evaluation
-> authority boundary
-> decision packet
```

Together, the product story becomes:

```text
CSV/OData/RFC/manual upload
-> Delivery / carrier / event / policy onboarding
-> governed decision packet
-> existing enterprise workflow demo
```

For the current Scenario 03 demo, the onboarding plan explains where each
artifact should come from:

| Demo artifact | Onboarding source |
| --- | --- |
| `Delivery 80001241` | SAP delivery or DO extract. |
| `delivery_status_event` | SAP delivery status or CBO event history. |
| `carrier_update_stale` | Latest carrier update compared with policy confirmation window. |
| `delay_confirmation_policy` | Normalized SOP or approval matrix rule. |
| `obtain current carrier confirmation` | Missing or stale external confirmation fallback task. |
| `verify delay owner` | Role matrix, workflow owner, or manual owner verification. |
| `maximum_safe_delegation_level = 1` | Authority rule derived from stale confirmation and unmet preconditions. |
| `Level 2 blocked` | Policy plus freshness plus owner-verification constraint. |

This closes the product gap identified in Issue #4 at the planning level. It
does not claim that the EKOS implementation already ingests these sources. It
defines the narrow, realistic onboarding path that would make the existing
decision-packet demo credible as a v1 product workflow.
