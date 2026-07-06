# EKOS SAP Logistics Data Model v2 Validation Worksheet

Status: Product discovery validation worksheet
Repository role: North Star product validation and SAP operations review
Reference issue: North Star Issue #4, "Product Gap: real enterprise data onboarding for EKOS decision packets"

This worksheet is for reviewing the EKOS SAP Logistics Data Model v2 with real
SAP logistics and control users. It is intended to test whether the proposed
field set is operationally realistic enough for review, not whether EKOS is
correct or production-ready.

## 1. Validation Goal

The current EKOS SAP-like demo is understandable: a reviewer can see what EKOS
allows, what it blocks, and why.

The next product risk is operational realism:

```text
Does the v2 data model contain the fields real SAP logistics users need to
review a delivery-delay, carrier-confirmation, and correction-workflow decision?
```

This is not a correctness benchmark, production validation, human validation,
or approval test. It is a field-realism and operational-reviewability check.

Use the worksheet to identify:

- fields that are required before approval-preparation should be allowed
- fields that improve reviewability but do not change the allowed action
- fields that are useful context but not necessary for v2
- fields that are hard to extract from SAP, carrier systems, custom CBO tables,
  Excel, email, or manual workflow records

## 2. Reviewer Profile

Target reviewers:

- SAP logistics operations user
- SAP LE/TM/EWM support user
- logistics process owner
- audit/control reviewer
- approval workflow owner

Reviewer metadata:

| Item | Response |
| --- | --- |
| Reviewer role | |
| Team or function | |
| SAP domain familiarity | |
| Date | |
| Scenario reviewed | |
| Notes on review context | |

## 3. Demo Materials To Show

Show the reviewer the smallest set of materials needed to understand the
current demo and the proposed v2 data model.

Primary materials:

- `out/ekos-enterprise-workflow-from-sap-like-r1/human_readable_report.md`
- `out/ekos-data-onboarding-sap-logistics-r1/onboarding_report.md`
- `docs/product/ekos-sap-logistics-data-model-v2.md`

Optional source extracts:

- `data/demo/sap_like/deliveries.csv`
- `data/demo/sap_like/carrier_updates.csv`
- `data/demo/sap_like/policies.csv`

Do not show this as a production SAP integration. These are synthetic SAP-like
artifacts used to test whether the workflow and field model are understandable.

## 4. Core Validation Questions

Ask these questions before walking through the field table:

1. Can you understand what EKOS allows and blocks?
2. What field is missing before you would trust this in your workflow?
3. Which missing field would change the allowed action?
4. Which missing field only improves context?
5. Which missing field is hard to extract from SAP?
6. Which field should block approval-preparation if missing?
7. Which field is nice-to-have but not required?

Record free-form notes:

| Question | Reviewer answer |
| --- | --- |
| What EKOS allows and blocks | |
| Missing field before workflow trust | |
| Missing field that changes allowed action | |
| Missing field that only improves context | |
| Field hard to extract from SAP | |
| Field that should block approval-preparation if missing | |
| Nice-to-have field | |

## 5. Field Review Table

Use this table to review each v2 field. Score the field using the decision
impact scale in Section 6, then capture whether the reviewer believes the field
is needed, blocking, extractable, or removable.

| Field group | Field | Needed for real review? | Blocks approval-preparation if missing? | Easy to extract? | Reviewer notes |
| --- | --- | --- | --- | --- | --- |
| Core delivery identity | `delivery_id` | | | | |
| Core delivery status | `current_status` | | | | |
| Logistics timing | `planned_handoff_at` | | | | |
| Logistics timing | `planned_delivery_date` | | | | |
| Carrier and tracking | `carrier_id` | | | | |
| Carrier and tracking | `tracking_number` | | | | |
| Carrier and tracking | `latest_confirmed_eta` | | | | |
| Carrier confirmation | `carrier_confirmation_timestamp` | | | | |
| Carrier confirmation | `confirmed_by` | | | | |
| Carrier confirmation | `confirmation_status` | | | | |
| Ownership and responsibility | `delay_owner` | | | | |
| Ownership and responsibility | `responsible_team` | | | | |
| Policy and approval | `policy_id` | | | | |
| Policy and approval | `policy_version` | | | | |
| Policy and approval | `sop_reference` | | | | |
| Policy and approval | `approval_role` | | | | |
| Exception and review | `review_status` | | | | |
| Exception and review | `reviewer_id` if available | | | | |
| Exception and review | `exception_reason_code` | | | | |
| Customer and business impact | `ship_to` | | | | |
| Customer and business impact | `customer_commitment` | | | | |
| Customer and business impact | `SLA` | | | | |
| SAP logistics relationships | `DO-FO/FU relationship` | | | | |
| SAP logistics relationships | `route` | | | | |
| SAP logistics relationships | `shipping_point` | | | | |
| SAP logistics relationships | `plant` | | | | |
| SAP logistics relationships | `location_code` | | | | |
| SAP logistics relationships | `HBL/AWB` | | | | |
| SAP logistics relationships | `container_number` | | | | |

## 6. Decision Impact Scoring

Use the same score for all reviewers so field feedback can be compared across
roles.

| Score | Meaning | Interpretation |
| --- | --- | --- |
| 0 | Not needed | The field should not be part of the v2 minimum model. |
| 1 | Context only | The field is helpful background but does not affect review. |
| 2 | Affects urgency/escalation | The field changes priority, escalation, or customer-impact handling. |
| 3 | Affects reviewability | The field is needed to understand, audit, or reproduce the decision. |
| 4 | Affects allowed/blocked action | The field can change whether EKOS may prepare an approval-ready workflow. |
| 5 | Must-have blocker | If missing, approval-preparation should be blocked. |

Reviewer scoring summary:

| Field | Decision impact score | Reason |
| --- | --- | --- |
| | | |
| | | |
| | | |

## 7. P0/P1 Confirmation

Ask the reviewer to confirm, downgrade, upgrade, or remove each proposed
priority. The current priorities are hypotheses until reviewed by real SAP
operations users.

### Proposed P0 Fields

These fields are expected to affect allowed action, blocked action,
reviewability, or governance trace.

| Field | Keep P0? | Change to P1? | Remove? | Reviewer reason |
| --- | --- | --- | --- | --- |
| `carrier_confirmation_timestamp` | | | | |
| `confirmed_by` | | | | |
| `confirmation_status` | | | | |
| `delay_owner` | | | | |
| `responsible_team` | | | | |
| `policy_version` | | | | |
| `sop_reference` | | | | |
| `approval_role` | | | | |
| `review_status` | | | | |
| `exception_reason_code` | | | | |

### Proposed P1 Fields

These fields are expected to improve operational realism, urgency assessment,
transport traceability, or review context.

| Field | Keep P1? | Upgrade to P0? | Remove? | Reviewer reason |
| --- | --- | --- | --- | --- |
| `ship_to` | | | | |
| `customer_commitment` | | | | |
| `SLA` | | | | |
| `DO-FO/FU relationship` | | | | |
| `route` | | | | |
| `shipping_point` | | | | |
| `plant` | | | | |
| `location_code` | | | | |
| `HBL/AWB` | | | | |
| `container_number` | | | | |

## 8. Operational Trust Questions

Use these questions after the reviewer has seen the current manager report and
the proposed field list.

| Question | Reviewer answer |
| --- | --- |
| Could this report be used in a real daily operations meeting? | |
| Could this report be attached to an approval request? | |
| Could an auditor reconstruct why EKOS blocked the action? | |
| What would make the report unacceptable? | |
| What wording is still confusing? | |
| What field would most improve trust? | |
| What field would most improve auditability? | |
| What field would most improve operational actionability? | |

## 9. Extraction Feasibility Questions

Use these questions to separate decision importance from extraction difficulty.
A field may be important even if it is hard to extract, and an easy field should
not be added unless it improves action boundaries, reviewability, urgency, or
governance trace.

| Question | Reviewer answer |
| --- | --- |
| Which fields are available in SAP standard tables? | |
| Which fields are in custom CBO tables? | |
| Which fields are in external carrier or 3PL feeds? | |
| Which fields are in manual Excel or email today? | |
| Which fields have poor data quality? | |
| Which fields require business approval to access? | |
| Which fields are usually known but not captured in SAP? | |
| Which fields are captured but not trusted by operators? | |

Extraction feasibility summary:

| Field | Likely source | Feasibility | Data quality risk | Access risk |
| --- | --- | --- | --- | --- |
| | | | | |
| | | | | |
| | | | | |

## 10. Output Format

Record each completed review with this summary format.

| Output item | Value |
| --- | --- |
| Reviewer role | |
| Date | |
| Scenario reviewed | |
| Field priority changes | |
| Must-have missing fields | |
| Fields to remove | |
| Fields to defer | |
| Implementation recommendation | |
| Confidence level | Low / Medium / High |
| Reason for confidence level | |

Implementation recommendation options:

- no v2 implementation yet; more reviewer input needed
- implement only fields scored 5
- implement fields scored 4 or higher
- implement fields scored 3 or higher
- remove or defer fields scored 0 or 1
- revisit field definitions before implementation

## 11. Decision Rule For Next Implementation

Only implement v2 fields that meet at least one of these conditions:

- score 3 or higher from at least one real SAP operations reviewer
- directly affect the allowed or blocked action
- materially improve reviewability
- materially improve governance trace

Do not implement a field only because it is easy to extract. Do not defer a
field only because it is difficult to extract if reviewers say it is required to
avoid unsafe approval-preparation.

Before implementation, classify each accepted field as one of:

- approval-preparation blocker
- reviewability requirement
- governance trace requirement
- urgency or escalation context
- optional operational context

## 12. Claim Boundary

This worksheet does not prove EKOS correctness.

It does not prove production readiness.

It is not human validation of EKOS decisions.

It is not a benchmark.

It does not add RAG.

It does not modify EKOS implementation.

It is a product-discovery validation tool for deciding whether the proposed SAP
Logistics Data Model v2 field set is realistic enough for real operations
review.
