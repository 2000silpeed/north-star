# EKOS SAP Logistics Data Model v2

Status: Product data model iteration plan
Repository role: North Star product strategy and enterprise data architecture
Implementation repository: `2000silpeed/ekos-sap-knowledge-os`
Reference issue: North Star Issue #4, "Product Gap: real enterprise data onboarding for EKOS decision packets"

This document defines the next realistic SAP logistics data model iteration for
EKOS after the first SAP-like E2E manager-report comprehension check.

It is not an implementation plan, benchmark, sample-data design, RAG addition,
or production-readiness claim.

## 1. Executive Summary

The current EKOS SAP-like demo passed a basic comprehension check. A viewer can
understand what EKOS allows, what it blocks, and why:

- allowed action: recommendation only
- blocked action: prepare an approval-ready correction workflow
- reason: stale carrier confirmation, policy requirement for current
  confirmation, and missing delay owner verification
- governance result: `review_required`

The next gap is not basic UX. The next gap is operational realism:

```text
Does the data model contain enough SAP/logistics context to support real-world
review?
```

The current demo is good enough to explain EKOS's value chain. It is not yet
rich enough to survive operational review by SAP logistics users without adding
ownership, customer impact, timing, carrier identity, SAP logistics
relationships, policy versioning, approval context, and review trace fields.

## 2. Current Demo Data Model

The current minimal model supports the first deterministic demo path:

```text
SAP-like structured extracts
-> evidence objects
-> decision packet
-> manager report
-> review_required governance state
```

Current model elements:

- delivery identity
- delivery status
- carrier update timestamp
- confirmation window
- policy requirement
- approval boundary
- evidence objects
- decision packet
- `review_required` governance state

This is sufficient to explain why a stale carrier update blocks
approval-preparation. It is not sufficient to answer all questions an SAP
operations manager would ask before trusting the packet in a real workflow.

## 3. Feedback-Derived Missing Fields

The first manager-report comprehension check showed that the report is readable,
but it also surfaced the fields needed for operational realism.

### A. Ownership And Responsibility

Fields:

- delivery owner
- delay owner
- responsible team
- reviewer ID

Why this matters:

Approval-ready workflow preparation should not proceed unless the responsible
human or team is known. Missing ownership should remain an execution
precondition or governance blocker.

### B. Customer And Business Impact

Fields:

- ship-to
- customer impact
- customer commitment
- SLA

Why this matters:

Customer impact changes urgency, escalation path, and review priority. A delay
with no customer commitment at risk should not be treated the same as a delay
that threatens an SLA or customer-facing promise.

### C. Logistics Timing

Fields:

- planned GI
- actual GI
- planned delivery date
- planned handoff
- latest confirmed ETA
- last SAP status change timestamp

Why this matters:

Timing determines whether a delay is active, stale, recently changed, or already
superseded. EKOS needs both planned and actual timing to avoid drawing
conclusions from a stale snapshot.

### D. Carrier And Tracking

Fields:

- carrier ID
- tracking number
- carrier confirmation timestamp
- confirmed_by
- update source

Why this matters:

Carrier evidence should be attributable. "Carrier update exists" is weaker than
"Carrier X confirmed ETA Y at time Z through source S by actor A." Trust,
freshness, and reviewability depend on these fields.

### E. SAP Logistics Relationships

Fields:

- DO-FO/FU relationship
- route
- shipping point
- plant
- location code
- container number if applicable
- HBL/AWB if applicable

Why this matters:

Real logistics review often depends on transport relationships and physical
movement context. A delivery delay may need to be interpreted through freight
order, freight unit, route, location, container, or air/sea waybill context.

### F. Policy And Approval

Fields:

- approval role
- approval matrix version
- policy version
- SOP reference
- confirmation requirement
- exception handling rule

Why this matters:

Auditability depends on policy version and approval matrix traceability. Without
versioned policy and approval context, EKOS can explain a boundary but cannot
prove which rule or authority matrix produced it.

### G. Exception And Review

Fields:

- exception reason code
- manual review note
- reviewer ID
- review timestamp
- previous decision packet ID

Why this matters:

Review state is not only a final status. Real operations need exception reason,
reviewer identity, review timing, and continuity from prior packets or manual
override attempts.

## 4. Field-to-EKOS Impact Matrix

| Field group | Example fields | EKOS object/evidence affected | Why it matters | v2 priority |
| --- | --- | --- | --- | --- |
| Carrier confirmation | carrier confirmation timestamp, confirmed_by, confirmation_status | `CarrierUpdate`, `EvidenceObject` | Determines whether carrier confirmation is current and trusted. | P0 |
| Ownership and responsibility | delay owner, responsible team | `ExecutionPrecondition`, `GovernanceReview` | Needed before approval-ready workflow preparation. | P0 |
| Policy traceability | policy version, SOP reference | `PolicyConstraint` | Needed for auditability and review traceability. | P0 |
| Approval authority | approval role, approval matrix version | `ApprovalBoundary`, `GovernanceReview` | Establishes who can review or approve higher-risk preparation. | P0 |
| Exception context | exception reason code, review status, reviewer ID | `ReviewTrace`, `BlockingRisk` | Supports review continuity and prevents unexplained packet states. | P0 |
| Customer impact | customer commitment, SLA, ship-to | `BusinessImpact`, `BlockingRisk` | Determines urgency and escalation path. | P1 |
| SAP logistics relationship | DO-FO/FU relationship, route, shipping point, plant | `LogisticsRelationship` | Needed to understand downstream transport impact. | P1 |
| Physical tracking context | tracking number, location code, HBL/AWB, container number | `CarrierUpdate`, `LogisticsRelationship` | Improves traceability across carrier and transport records. | P1 |
| SAP timing | planned GI, actual GI, planned delivery date, last SAP status change timestamp | `Delivery`, `DeliveryEvent`, `EvidenceObject` | Distinguishes active delay from outdated or superseded status. | P1 |
| Manual review continuity | manual review note, review timestamp, previous decision packet ID | `ReviewTrace`, `GovernanceReview` | Allows future reviewers to reconstruct prior reasoning. | P1 |

Priority rule:

```text
P0 means the field can change whether approval-preparation is allowed or
reviewable.
P1 means the field improves operational realism, urgency, traceability, or
escalation, but may not always change the allowed/blocked action.
```

## 5. Recommended v2 Minimal Field Set

The v2 field set should remain narrow. Add fields because they affect the
allowed action, blocked action, reviewability, or governance trace, not because
they are easy to extract.

### P0 Fields

- `delivery_id`
- `current_status`
- `planned_handoff_at`
- `planned_delivery_date`
- `carrier_id`
- `tracking_number`
- `latest_confirmed_eta`
- `carrier_confirmation_timestamp`
- `confirmed_by`
- `confirmation_status`
- `delay_owner`
- `responsible_team`
- `policy_id`
- `policy_version`
- `sop_reference`
- `approval_role`
- `review_status`
- `reviewer_id` if available
- `exception_reason_code`

### P1 Fields

- `ship_to`
- `customer_commitment`
- `SLA`
- `DO-FO/FU relationship`
- `route`
- `shipping_point`
- `plant`
- `location_code`
- `HBL/AWB`
- `container_number`

## 6. Updated Evidence Objects

The v2 evidence model should retain the current evidence objects and add
operationally specific evidence where needed.

Evidence objects:

- `delivery_status_event`
- `carrier_update_stale`
- `carrier_confirmation_current`
- `carrier_confirmation_missing`
- `delay_owner_unverified`
- `policy_confirmation_required`
- `approval_role_required`
- `customer_commitment_at_risk`
- `sap_status_recently_changed`
- `transport_relationship_available`
- `exception_reason_recorded`

Interpretation notes:

- `carrier_update_stale` should remain distinct from
  `carrier_confirmation_missing`. A carrier may have sent an old update, and
  there may also be no current confirmed ETA.
- `delay_owner_unverified` should become explicit evidence, not only an implied
  missing precondition.
- `approval_role_required` should exist when policy or approval matrix requires
  a human role before preparation can proceed.
- `customer_commitment_at_risk` should affect urgency and escalation, not
  automatically grant higher authority.

## 7. Updated Blocking Risks

Updated blocking risks:

- `stale_external_event`
- `missing_confirmed_eta`
- `unverified_delay_owner`
- `missing_policy_version`
- `missing_approval_role`
- `customer_commitment_at_risk`
- `incomplete_transport_relationship`
- `missing_review_note`

Risk interpretation:

- `stale_external_event` and `missing_confirmed_eta` block
  approval-preparation when policy requires current external confirmation.
- `unverified_delay_owner` blocks preparation when owner verification is
  required by policy or approval matrix.
- `missing_policy_version` blocks audit-grade approval-preparation because the
  governing rule cannot be reconstructed.
- `missing_approval_role` blocks governance workflow routing.
- `customer_commitment_at_risk` may increase urgency or escalation; it should
  not bypass confirmation or approval requirements.
- `incomplete_transport_relationship` limits confidence in downstream
  logistics impact.
- `missing_review_note` blocks review continuity when a manual override or prior
  reviewer action is referenced but not explained.

## 8. Updated Decision Packet v2 Shape

The v2 decision packet shape should remain conceptual until implemented in the
EKOS repository.

```json
{
  "primary_object": {
    "type": "Delivery",
    "id": "..."
  },
  "business_impact": {
    "ship_to": "...",
    "customer_commitment": "...",
    "sla": "...",
    "customer_commitment_at_risk": true
  },
  "logistics_relationships": {
    "do_fo_fu_relationship": "...",
    "route": "...",
    "shipping_point": "...",
    "plant": "...",
    "location_code": "...",
    "container_number": "...",
    "hbl_awb": "..."
  },
  "evidence_ids": [
    "delivery_status_event",
    "carrier_update_stale",
    "policy_confirmation_required"
  ],
  "policy_ids": [
    "delay_confirmation_policy"
  ],
  "ownership": {
    "delivery_owner": "...",
    "delay_owner": "...",
    "responsible_team": "..."
  },
  "approval_boundary": {
    "approval_role": "...",
    "approval_matrix_version": "...",
    "allowed_action": "Recommendation only",
    "blocked_action": "Prepare approval-ready correction workflow"
  },
  "execution_preconditions": [
    "obtain current carrier confirmation",
    "verify delay owner before preparing workflow"
  ],
  "blocking_risks": [
    "stale_external_event",
    "unverified_delay_owner"
  ],
  "allowed_action": "Recommendation only",
  "blocked_action": "Prepare approval-ready correction workflow",
  "governance_status": "review_required",
  "review_trace": {
    "review_status": "...",
    "reviewer_id": "...",
    "review_timestamp": "...",
    "manual_review_note": "...",
    "previous_decision_packet_id": "..."
  }
}
```

This shape is not implementation code. It is the product contract for what a
more realistic packet must be able to express.

## 9. Validation Plan

Validate this v2 model with real SAP operations users before implementation.

Recommended validation sequence:

1. Show the current `human_readable_report.md`.
2. Ask what field is missing for operational trust.
3. Compare answers against the v2 field set.
4. Prioritize fields by decision impact, not data availability.
5. Avoid adding fields that do not change the allowed action, blocked action,
   reviewability, escalation path, or governance trace.

Validation questions:

- Can the reviewer identify the responsible owner or team?
- Can the reviewer tell whether the customer commitment is at risk?
- Can the reviewer reconstruct carrier confirmation freshness?
- Can the reviewer trace the policy version and SOP source?
- Can the reviewer route the packet to the correct approval role?
- Can the reviewer understand whether transport relationships are complete?
- Can the reviewer tell whether a manual review already happened?

If a requested field does not affect any of these questions, it should remain
out of the v2 minimum set.

## 10. Claim Boundary

- This is a data model iteration plan, not an implementation.
- It does not prove production readiness.
- It does not require live SAP access.
- It does not add RAG.
- It does not create sample data.
- It does not modify EKOS.
- It is based on first comprehension feedback and should be validated with real
  SAP operations users.
- It preserves the current EKOS boundary: deterministic structured data can
  produce governed decision packets, while RAG remains outside the core decision
  authority.
