# SAP Logistics Table-To-Evidence Rules

Status: Product data transformation design
Repository role: North Star product strategy and enterprise data modeling
Implementation repository: `2000silpeed/ekos-sap-knowledge-os`
Reference issue: North Star Issue #4, "Product Gap: real enterprise data onboarding for EKOS decision packets"

This document defines deterministic transformation rules from realistic
SAP-like logistics extracts into EKOS evidence objects and decision packets for
one narrow v1 slice:

```text
Delivery delay / carrier confirmation / correction workflow readiness
```

It does not create CSV files, ingestion code, benchmarks, or EKOS
implementation changes. Synthetic CSV paths named in this document are design
targets for future synthetic experiment data, not files created in North Star.

## 1. Purpose

The purpose of this document is to bridge two product layers:

```text
realistic SAP-like table extracts
-> EKOS decision packet artifacts
```

The existing EKOS enterprise workflow demo already shows how a decision packet
can move through decision, explanation, explanation quality check, human review,
and governance approval. The missing upstream step is how realistic SAP-like
tables and custom logistics extracts become the structured objects assumed by
that packet.

This document therefore defines:

- source table families for the logistics slice
- canonical EKOS objects produced from those sources
- field-to-object mappings
- deterministic transformation rules
- synthetic CSV data designs
- one end-to-end example using Delivery `80001241`
- data quality checks
- v1 automation and human-governance boundaries

The goal is not live SAP integration. The goal is a precise transformation
design that can make synthetic SAP-like onboarding credible without using
confidential company data.

## 2. Source Table Families

The table names below are SAP-like examples. Customer landscapes vary, and the
same meaning may come from SAP standard tables, CDS/OData views, RFC/BAPI
extracts, custom CBO tables, SAP TM extracts, manual CSV exports, or controlled
spreadsheets.

| Source family | SAP-like or extract example | Minimum purpose |
| --- | --- | --- |
| Delivery header | `LIKP`-like extract or `deliveries.csv` | Delivery identity, status, shipping context, planned handoff, actual handoff, source reference. |
| Delivery item | `LIPS`-like extract, optional | Item-level material, quantity, plant, shipping point, or partial-status details if header alone is not enough. |
| Custom LE tracking event main table | `ZLE_TRACK_EVT`-like extract or `tracking_events.csv` | Current or summarized logistics tracking event for the delivery or transport object. |
| Custom LE tracking history table | `ZLE_TRACK_HIST`-like extract | Event chronology, previous values, late arrivals, retries, status corrections. |
| Carrier / 3PL event update extract | Carrier portal/API/EDI export or `carrier_updates.csv` | External carrier ETA, pickup, delay, and confirmation timestamp. |
| DO-FO/FU relationship extract | SAP TM/CDS/custom mapping extract, optional | Delivery-to-freight-order or delivery-to-freight-unit relationship when transportation planning context is needed. |
| Policy/SOP extract | Normalized SOP row or `policies.csv` | Confirmation window, required current external confirmation, blocked and allowed actions. |
| Approval matrix extract | Role/workflow matrix or `approval_matrix.csv` | Delay owner, reviewer, approver role, and human-governed escalation boundaries. |

Minimum source assumption:

```text
EKOS may receive extracts, not live system access.
```

Therefore every extract row must preserve source identity, record reference,
timestamps, and load time. When source identity is missing, the transformed
evidence should be marked as non-reviewable rather than silently accepted.

## 3. Canonical EKOS Objects

### Delivery

The primary business object being evaluated.

Minimum fields:

- `id`
- `source_system`
- `source_record_ref`
- `status`
- `planned_handoff_time`
- `actual_handoff_time`
- `ship_to_party`
- `plant`
- `shipping_point`

### DeliveryEvent

A time-bound internal or logistics event connected to the delivery.

Minimum fields:

- `id`
- `delivery_id`
- `event_type`
- `event_time`
- `recorded_at`
- `source_system`
- `source_record_ref`

### CarrierUpdate

An external or manually confirmed carrier signal.

Minimum fields:

- `id`
- `delivery_id`
- `carrier_id`
- `update_type`
- `updated_at`
- `received_at`
- `eta_time`
- `confirmation_state`
- `source_channel`
- `source_record_ref`

### EvidenceObject

A source-backed record that supports, weakens, or blocks a decision.

Minimum fields:

- `id`
- `type`
- `source_record_ref`
- `object_refs`
- `event_time`
- `recorded_at`
- `freshness_state`
- `supports_or_blocks`
- `reviewability_state`

### PolicyConstraint

A normalized policy rule that constrains preparation, approval, confirmation,
or execution.

Minimum fields:

- `id`
- `version`
- `requires_current_confirmation`
- `confirmation_window`
- `required_owner_verification`
- `allowed_when_unmet`
- `blocked_when_unmet`
- `source_ref`

### BlockingRisk

A derived risk that prevents the requested delegation level.

Minimum fields:

- `id`
- `reason`
- `source_evidence_ids`
- `blocked_action`
- `blocks_level`

### ExecutionPrecondition

A required condition before a higher-risk action can be prepared or executed.

Minimum fields:

- `id`
- `status`
- `required_for_level`
- `source_policy_id`
- `source_evidence_ids`

### DelegationDecision

The safe action boundary for the current packet.

Minimum fields:

- `requested_level`
- `maximum_safe_level`
- `action_category`
- `allowed_now`
- `blocked_now`
- `required_approver_role`

### DecisionPacket

The EKOS handoff artifact consumed by the enterprise workflow demo.

Minimum fields:

- `packet_id`
- `primary_object`
- `related_objects`
- `evidence`
- `policies`
- `execution_preconditions`
- `blocking_risks`
- `delegation_decision`
- `review_state`
- `claim_boundary`

## 4. Field-To-Object Mapping

The mappings below define how SAP-like fields become EKOS concepts. They are
source-neutral: a field may come from SAP table export, CDS view, OData payload,
RFC extract, CBO table, or manual CSV.

| SAP-like field | Meaning | EKOS concept | Example transformation |
| --- | --- | --- | --- |
| `LIKP-VBELN` or `delivery_id` | Delivery number / DO identity | `Delivery.id` | `80001241` -> `Delivery(id="80001241")` |
| `LIKP-WADAT` or `planned_handoff_time` | Planned goods issue / planned handoff date | `Delivery.planned_handoff_time` | `2026-05-30T08:00:00Z` -> planned handoff timestamp |
| `LIKP-WADAT_IST` or `actual_handoff_time` | Actual goods issue / actual handoff | `Delivery.actual_handoff_time` | Empty value -> no actual handoff evidence |
| `LIKP-LFSTK` or `delivery_status` | Delivery status | `Delivery.status` and possible `DeliveryEvent` | `DELAYED` -> candidate `delivery_status_event` |
| `LIPS-VGPOS` or `delivery_item_id` | Delivery item identity | `Delivery.related_item_ids` | Item rows linked to delivery when item-level status matters |
| `tracking_event_id` | Tracking event identity | `DeliveryEvent.id` | `TE-80001241-01` -> delivery event object |
| `tracking_event_type` | Event kind | `DeliveryEvent.event_type` | `HANDOFF_MISSED` -> delay-supporting event |
| `tracking_event_time` | Latest event time | `DeliveryEvent.event_time` | `2026-05-30T10:00:00Z` -> event timestamp |
| `tracking_recorded_at` | When event entered enterprise system | `DeliveryEvent.recorded_at` | Used to detect delayed ingestion or impossible ordering |
| `carrier_update_id` | Carrier update identity | `CarrierUpdate.id` | `CU-80001241-01` -> carrier update object |
| `carrier_id` | Carrier or 3PL identity | `CarrierUpdate.carrier_id` | `CARRIER-17` -> external source owner |
| `carrier_eta_time` | ETA supplied by carrier | `CarrierUpdate.eta_time` | `2026-05-30T18:00:00Z` -> ETA evidence |
| `carrier_eta_update_time` | Carrier update timestamp | `CarrierUpdate.updated_at` | `2026-05-30T12:00:00Z` -> carrier update timestamp |
| `carrier_update_received_at` | Enterprise receipt timestamp | `CarrierUpdate.received_at` | Used to distinguish old carrier event from late enterprise ingestion |
| `current_time - carrier_eta_update_time > confirmation_window` | Freshness rule result | `EvidenceObject.carrier_update_stale` | `12h > 6h` -> stale carrier update evidence |
| `policy.requires_current_confirmation = true` | Policy requires external confirmation | `PolicyConstraint.delay_confirmation_policy` | Row becomes `delay_confirmation_policy` |
| `policy.confirmation_window_hours` | Validity window for carrier confirmation | `PolicyConstraint.confirmation_window` | `6` -> confirmation window of six hours |
| Missing current confirmation | Required evidence absent | `ExecutionPrecondition.obtain_current_carrier_confirmation` | No current carrier confirmation -> precondition `unmet` |
| `approval_matrix.delay_owner_required = true` | Owner verification is required | `ExecutionPrecondition.verify_delay_owner` | Missing owner verification -> precondition `unmet` |
| Stale update plus policy requirement | Stale external signal constrains action | `BlockingRisk.stale_external_event` | `carrier_update_stale + delay_confirmation_policy` -> risk active |
| Stale update plus missing confirmation | Correction plan lacks sufficient confirmation | `BlockingRisk.insufficient_confirmation_for_execution_plan` | Missing current confirmation -> Level 2 blocked |
| Active blocking risk | Delegation cap | `DelegationDecision.maximum_safe_level` | Any Level 2 blocker active -> maximum safe level `1` |

## 5. Transformation Rules

The rules are deterministic where the source data is deterministic. Ambiguous
policy meaning, ambiguous ownership, and production execution remain
human-governed in v1.

### Rule 1: Delivery Identity Resolution

Input:

- `LIKP-VBELN` or `delivery_id`
- source system and source record reference

Transformation:

1. Normalize delivery ID by trimming whitespace and preserving leading zeros
   when the source treats them as part of identity.
2. Require exactly one primary delivery header row for the packet.
3. Attach `source_system` and `source_record_ref`.
4. If duplicates exist with conflicting status or handoff timestamps, do not
   merge silently; create a data quality failure.

Output:

```text
Delivery(id="80001241", source_record_ref="LIKP:80001241")
```

### Rule 2: Delay Status Detection

Input:

- delivery status
- planned handoff time
- actual handoff time
- tracking events
- decision time

Transformation:

Create `EvidenceObject.delivery_status_event` when at least one of the following
is true:

- delivery status is explicitly delayed
- planned handoff time is earlier than decision time and actual handoff is
  missing
- latest tracking event indicates missed handoff, pickup missed, carrier delay,
  or equivalent delay status

If status and event history conflict, retain both and mark freshness or
relationship state as `conflicting`.

Output:

```text
EvidenceObject(type="delivery_status_event", supports_or_blocks="supports_delay")
```

### Rule 3: Carrier Update Freshness

Input:

- carrier update timestamp
- carrier update receipt timestamp
- policy confirmation window
- decision time

Transformation:

1. Select the latest relevant external carrier update for the delivery or its
   related transport object.
2. Compute age from `carrier_eta_update_time` to `decision_time`.
3. If no carrier update exists, freshness is `missing`.
4. If `age > confirmation_window`, freshness is `stale`.
5. If `age <= confirmation_window`, freshness is `current`.
6. If timestamps are impossible or conflicting, freshness is `conflicting`.

Important:

```text
loaded_at does not make an old carrier update current.
```

Output:

```text
EvidenceObject(type="carrier_update_stale", freshness_state="stale")
```

### Rule 4: Current Confirmation Requirement

Input:

- normalized policy row
- requested action
- requested delegation level
- carrier update freshness

Transformation:

If policy requires current confirmation and the requested action includes
preparing, staging, approving, or executing a correction workflow, then current
external confirmation is mandatory for Level 2 or higher.

If current confirmation is stale, missing, unknown, or conflicting, create:

```text
ExecutionPrecondition(id="obtain_current_carrier_confirmation", status="unmet")
```

### Rule 5: Delay Owner Verification Requirement

Input:

- approval matrix row
- policy row
- delivery owner or responsible logistics role
- manual verification record, if any

Transformation:

If the policy or approval matrix requires delay owner verification, check for a
verified owner record. If none exists, create:

```text
ExecutionPrecondition(id="verify_delay_owner", status="unmet")
```

If multiple owner candidates exist and no authoritative source resolves the
owner, mark owner verification as `unknown` and require human review.

### Rule 6: Blocking Risk Derivation

Input:

- evidence objects
- policy constraints
- execution preconditions

Transformation:

Create `BlockingRisk.stale_external_event` when:

```text
carrier_update_stale is active
and delay_confirmation_policy.requires_current_confirmation = true
```

Create `BlockingRisk.insufficient_confirmation_for_execution_plan` when:

```text
obtain_current_carrier_confirmation is unmet
and requested_delegation_level >= 2
```

Create `BlockingRisk.delay_owner_unverified` when:

```text
verify_delay_owner is unmet or unknown
and requested_delegation_level >= 2
```

All blocking risks must retain source evidence IDs and policy IDs.

### Rule 7: Maximum Safe Delegation Level Derivation

Input:

- requested delegation level
- blocking risks
- policy constraints
- data quality state

Transformation:

Default v1 levels:

| Condition | Maximum safe delegation level |
| --- | ---: |
| Primary delivery identity is missing or non-unique | 0 |
| Required policy is missing or unversioned | 1 |
| Carrier confirmation is stale, missing, unknown, or conflicting | 1 |
| Delay owner verification is missing or unknown | 1 |
| Any Level 2 blocking risk is active | 1 |
| Current confirmation, owner verification, policy, and required relationships are all valid | 2 |
| Production execution requested | 1 in v1; Level 3 is not automated |

Scenario 03-style result:

```text
requested_delegation_level = 2
maximum_safe_delegation_level = 1
action_category = recommend
```

### Rule 8: Decision Packet Construction

Input:

- `Delivery`
- related objects
- evidence objects
- policy constraints
- execution preconditions
- blocking risks
- delegation decision

Transformation:

Create a `DecisionPacket` that contains:

- primary object
- source snapshot reference
- related delivery, carrier, and transport objects
- evidence IDs and freshness states
- policy IDs and source references
- unmet execution preconditions
- active blocking risks
- maximum safe delegation level
- allowed and blocked actions
- human review requirement
- claim boundary

The packet may be consumed by the existing enterprise workflow demo only after
source references and blocking risks are explicit enough for review.

## 6. Synthetic Experiment Data

The following synthetic CSV designs are proposed for a future SAP-like
experiment package. They should belong in the EKOS implementation repository if
materialized. North Star defines the design only.

```text
data/demo/sap_like/deliveries.csv
data/demo/sap_like/tracking_events.csv
data/demo/sap_like/carrier_updates.csv
data/demo/sap_like/policies.csv
data/demo/sap_like/approval_matrix.csv
```

Optional relationship file when DO-FO/FU mapping is needed:

```text
data/demo/sap_like/delivery_relationships.csv
```

### `data/demo/sap_like/deliveries.csv`

Columns:

```text
delivery_id, source_system, source_record_ref, delivery_status, shipping_point,
plant, ship_to_party, planned_handoff_time, actual_handoff_time,
planned_delivery_time, last_internal_status_at
```

Example rows:

| delivery_id | source_system | source_record_ref | delivery_status | planned_handoff_time | actual_handoff_time | last_internal_status_at |
| --- | --- | --- | --- | --- | --- | --- |
| 80001241 | SAP-S4-LE | LIKP:80001241 | DELAYED | 2026-05-30T08:00:00Z |  | 2026-05-30T10:30:00Z |
| 80001242 | SAP-S4-LE | LIKP:80001242 | IN_TRANSIT | 2026-05-30T09:00:00Z | 2026-05-30T09:15:00Z | 2026-05-30T09:20:00Z |

Produces:

- `Delivery`
- candidate `EvidenceObject.delivery_status_event`

### `data/demo/sap_like/tracking_events.csv`

Columns:

```text
tracking_event_id, delivery_id, transport_object_id, event_type, event_time,
recorded_at, source_system, source_record_ref, event_description
```

Example rows:

| tracking_event_id | delivery_id | event_type | event_time | recorded_at | source_record_ref |
| --- | --- | --- | --- | --- | --- |
| TE-80001241-01 | 80001241 | HANDOFF_MISSED | 2026-05-30T10:00:00Z | 2026-05-30T10:05:00Z | ZLE_TRACK_EVT:TE-80001241-01 |
| TE-80001241-02 | 80001241 | DELAY_CLASSIFICATION_REVIEW | 2026-05-30T10:15:00Z | 2026-05-30T10:16:00Z | ZLE_TRACK_HIST:TE-80001241-02 |

Produces:

- `DeliveryEvent`
- `EvidenceObject.delivery_status_event`
- event history for data quality and timeline checks

### `data/demo/sap_like/carrier_updates.csv`

Columns:

```text
carrier_update_id, delivery_id, transport_object_id, carrier_id, update_type,
carrier_eta_time, carrier_eta_update_time, carrier_update_received_at,
confirmation_state, source_channel, source_record_ref
```

Example rows:

| carrier_update_id | delivery_id | carrier_id | update_type | carrier_eta_update_time | carrier_update_received_at | confirmation_state |
| --- | --- | --- | --- | --- | --- | --- |
| CU-80001241-01 | 80001241 | CARRIER-17 | ETA_DELAY | 2026-05-30T12:00:00Z | 2026-05-30T12:02:00Z | UNCONFIRMED |
| CU-80001242-01 | 80001242 | CARRIER-04 | ETA_CONFIRMED | 2026-05-30T23:00:00Z | 2026-05-30T23:01:00Z | CONFIRMED |

Produces:

- `CarrierUpdate`
- `EvidenceObject.carrier_update_stale` or
  `EvidenceObject.carrier_update_current`
- `ExecutionPrecondition.obtain_current_carrier_confirmation`

### `data/demo/sap_like/policies.csv`

Columns:

```text
policy_id, policy_version, policy_source_ref, applies_to_object_type,
requires_current_confirmation, confirmation_window_hours,
required_owner_verification, allowed_when_unmet, blocked_when_unmet,
effective_from, effective_to, policy_owner
```

Example rows:

| policy_id | policy_version | requires_current_confirmation | confirmation_window_hours | required_owner_verification | blocked_when_unmet |
| --- | --- | --- | ---: | --- | --- |
| delay_confirmation_policy | 2026.05 | true | 6 | true | prepare_for_approval;stage_workflow;execute |

Produces:

- `PolicyConstraint.delay_confirmation_policy`
- policy source reference for review

### `data/demo/sap_like/approval_matrix.csv`

Columns:

```text
object_type, action_category, requested_delegation_level, owner_role,
reviewer_role, approver_role, owner_verification_required,
production_execution_allowed_in_v1
```

Example rows:

| object_type | action_category | requested_delegation_level | owner_role | reviewer_role | approver_role | owner_verification_required | production_execution_allowed_in_v1 |
| --- | --- | ---: | --- | --- | --- | --- | --- |
| Delivery | recommend | 1 | logistics_operations | internal_audit_reviewer |  | true | false |
| Delivery | prepare_for_approval | 2 | logistics_operations | internal_audit_reviewer | logistics_manager | true | false |

Produces:

- `ExecutionPrecondition.verify_delay_owner`
- `DelegationDecision.required_approver_role`
- human review and approval boundary metadata

### Optional `data/demo/sap_like/delivery_relationships.csv`

Columns:

```text
delivery_id, relationship_type, related_object_type, related_object_id,
carrier_id, relationship_status, relationship_observed_at, source_record_ref
```

Example row:

| delivery_id | relationship_type | related_object_type | related_object_id | carrier_id | relationship_status |
| --- | --- | --- | --- | --- | --- |
| 80001241 | DO_TO_FU | FreightUnit | FU-5510-01 | CARRIER-17 | VERIFIED |

Produces:

- related `TransportObject`
- relationship evidence when transportation planning context is relevant

## 7. Example End-To-End Transformation

This example uses a synthetic decision time:

```text
decision_time = 2026-05-31T00:00:00Z
```

### Raw SAP-Like Rows

Delivery row:

| field | value |
| --- | --- |
| `delivery_id` | `80001241` |
| `source_record_ref` | `LIKP:80001241` |
| `delivery_status` | `DELAYED` |
| `planned_handoff_time` | `2026-05-30T08:00:00Z` |
| `actual_handoff_time` | empty |
| `last_internal_status_at` | `2026-05-30T10:30:00Z` |

Tracking event row:

| field | value |
| --- | --- |
| `tracking_event_id` | `TE-80001241-01` |
| `event_type` | `HANDOFF_MISSED` |
| `event_time` | `2026-05-30T10:00:00Z` |
| `recorded_at` | `2026-05-30T10:05:00Z` |

Carrier update row:

| field | value |
| --- | --- |
| `carrier_update_id` | `CU-80001241-01` |
| `carrier_id` | `CARRIER-17` |
| `update_type` | `ETA_DELAY` |
| `carrier_eta_update_time` | `2026-05-30T12:00:00Z` |
| `confirmation_state` | `UNCONFIRMED` |

Policy row:

| field | value |
| --- | --- |
| `policy_id` | `delay_confirmation_policy` |
| `requires_current_confirmation` | `true` |
| `confirmation_window_hours` | `6` |
| `required_owner_verification` | `true` |
| `blocked_when_unmet` | `prepare_for_approval;stage_workflow;execute` |

Approval matrix row:

| field | value |
| --- | --- |
| `object_type` | `Delivery` |
| `requested_delegation_level` | `2` |
| `owner_role` | `logistics_operations` |
| `reviewer_role` | `internal_audit_reviewer` |
| `approver_role` | `logistics_manager` |
| `owner_verification_required` | `true` |

### Normalized Objects

```text
Delivery:
  id = 80001241
  status = DELAYED
  planned_handoff_time = 2026-05-30T08:00:00Z
  actual_handoff_time = null

DeliveryEvent:
  id = TE-80001241-01
  event_type = HANDOFF_MISSED
  event_time = 2026-05-30T10:00:00Z

CarrierUpdate:
  id = CU-80001241-01
  carrier_id = CARRIER-17
  updated_at = 2026-05-30T12:00:00Z
  confirmation_state = UNCONFIRMED

PolicyConstraint:
  id = delay_confirmation_policy
  requires_current_confirmation = true
  confirmation_window = 6 hours
```

### Evidence Objects

At `2026-05-31T00:00:00Z`, the carrier update is 12 hours old.

```text
EvidenceObject.delivery_status_event:
  source = LIKP:80001241 + ZLE_TRACK_EVT:TE-80001241-01
  supports_or_blocks = supports_delay

EvidenceObject.carrier_update_stale:
  source = carrier update CU-80001241-01
  freshness_state = stale
  reason = 12 hours old > 6 hour confirmation window

EvidenceObject.policy_confirmation_required:
  source = delay_confirmation_policy version 2026.05
  supports_or_blocks = blocks_prepare_for_approval_without_current_confirmation
```

### Blocking Risks

```text
BlockingRisk.stale_external_event:
  reason = carrier update is stale while policy requires current confirmation
  blocks_level = 2

BlockingRisk.insufficient_confirmation_for_execution_plan:
  reason = current external confirmation is missing for correction workflow preparation
  blocks_level = 2

BlockingRisk.delay_owner_unverified:
  reason = delay owner verification is required but not yet recorded
  blocks_level = 2
```

### Decision Packet

```json
{
  "packet_id": "DP-80001241-S03",
  "primary_object": {
    "object_type": "Delivery",
    "object_id": "80001241",
    "source_record_ref": "LIKP:80001241"
  },
  "requested_action": "Recommend likely carrier-delay classification and request current carrier confirmation before drafting any correction workflow.",
  "requested_delegation_level": 2,
  "evidence_ids": [
    "delivery_status_event",
    "carrier_update_stale",
    "policy_confirmation_required"
  ],
  "policy_ids": [
    "delay_confirmation_policy"
  ],
  "execution_preconditions": [
    {
      "precondition_id": "obtain_current_carrier_confirmation",
      "status": "unmet",
      "required_for_level": 2
    },
    {
      "precondition_id": "verify_delay_owner",
      "status": "unmet",
      "required_for_level": 2
    }
  ],
  "blocking_risks": [
    "stale_external_event",
    "insufficient_confirmation_for_execution_plan",
    "delay_owner_unverified"
  ],
  "delegation_decision": {
    "requested_level": 2,
    "maximum_safe_level": 1,
    "action_category": "recommend",
    "allowed_now": [
      "recommend_likely_carrier_delay",
      "request_current_carrier_confirmation"
    ],
    "blocked_now": [
      "prepare_for_approval",
      "stage_workflow",
      "execute"
    ]
  },
  "claim_boundary": "Synthetic table-to-evidence transformation design; not production approval."
}
```

## 8. Data Quality Checks

The transformation layer should reject, limit, or flag packets when these checks
fail.

| Check | Detection | Required result |
| --- | --- | --- |
| Missing delivery ID | `delivery_id` or `LIKP-VBELN` is empty | Do not create a review-ready packet. |
| Duplicate delivery ID | Multiple primary rows conflict on status, source, or handoff time | Require manual reconciliation. |
| Missing latest event | No delivery status event or tracking event exists | Mark delay evidence as `unknown`; cap delegation at Level 1 or lower. |
| Stale timestamp | Latest carrier update exceeds confirmation window | Create `carrier_update_stale`; block Level 2. |
| Impossible timestamp | Event received before event happened, actual handoff before planned creation, or decision time before source event | Mark evidence as `conflicting`; require human review. |
| Missing policy | No applicable policy row for correction readiness | Do not grant Level 2. |
| Missing approval role | Approval matrix lacks required owner, reviewer, or approver | Mark authority boundary incomplete. |
| Unknown delay owner | Owner cannot be resolved from matrix, workflow, or manual verification | Create or retain `verify_delay_owner` precondition as unmet/unknown. |
| Inconsistent object relationship | Delivery points to multiple conflicting FO/FU objects or carrier IDs | Mark relationship unverified and require manual review. |

Data quality checks are not only validation errors. In EKOS, they may become
explicit evidence gaps, blocking risks, and explanation requirements.

## 9. v1 Automation Boundary

### What Can Be Automated

The following can be automated in v1 for synthetic or extract-based data:

- field mapping from SAP-like extracts into canonical objects
- exact delivery ID resolution
- deterministic DO-FO/FU linking when a reliable relationship extract exists
- delay status evidence creation from delivery and tracking rows
- carrier update freshness classification
- current confirmation precondition derivation
- policy constraint application from normalized policy rows
- blocking risk derivation
- maximum safe delegation level derivation
- decision packet creation

### What Remains Human-Governed

The following must remain human-governed in v1:

- policy authoring and policy normalization from SOP text
- approval matrix validation
- ambiguous delay ownership
- manual carrier confirmation acceptance
- resolution of conflicting SAP, carrier, and manual evidence
- final workflow execution
- production SAP writes
- customer-facing commitment changes
- override of any policy or freshness block

The automation boundary is intentional. EKOS v1 can prepare a decision-support
packet; it must not turn that packet into production authority.

## 10. Claim Boundary

This document is not live SAP integration.

It is not proof of production readiness.

It is not a benchmark, demo, or implementation change.

It does not assume access to confidential company data.

It defines transformation rules and synthetic SAP-like data designs for the
narrow v1 onboarding slice:

```text
Delivery delay / carrier confirmation / correction workflow readiness
```

The supported claim is limited:

```text
These rules describe how realistic SAP-like extracts could be transformed into
EKOS evidence objects and decision packets for a synthetic onboarding
experiment.
```

Any claim that EKOS has implemented this ingestion, validated it on live SAP
data, or is production-ready would require separate implementation and evidence
in `2000silpeed/ekos-sap-knowledge-os`.
