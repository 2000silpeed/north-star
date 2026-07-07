# EKOS Missing Source Acquisition Plan

Status: Product/data architecture plan
Repository role: North Star product strategy and SAP operations data architecture
Implementation repository: `2000silpeed/ekos-sap-knowledge-os`
Reference issue: North Star Issue #4, "Product Gap: real enterprise data onboarding for EKOS decision packets"
Snapshot date: 2026-07-07

This document defines how EKOS should acquire or curate the missing
workflow-critical sources for the delivery-delay / carrier-confirmation
configured onboarding workflow:

- `carrier_updates`
- `approval_matrix`

It does not modify EKOS implementation, create code, add RAG, connect to live
SAP, claim production readiness, or assume the missing sources can be inferred
automatically.

## 1. Executive Summary

The minimum source package validator correctly fails when `carrier_updates` and
`approval_matrix` are missing.

That failure is the right safe behavior. The delivery-delay workflow depends on
current external carrier confirmation and governed authority routing. If either
source is absent, EKOS should not generate a decision packet from projection
alone.

The next product task is not more decision logic. The next task is to identify
how the missing sources can be acquired, curated, reviewed, and governed:

```text
projected SAP/logistics assets
-> minimum source package validation
-> missing critical aliases identified
-> controlled source acquisition
-> validation rerun
-> configured onboarding only after package passes
```

For v1, the practical path is hybrid onboarding:

- use existing projection for `deliveries` and `tracking_events`;
- use curated or extracted `carrier_updates` from a custom LE tracking source,
  3PL feed, carrier portal export, or controlled manual file;
- use curated `approval_matrix` and policy mapping maintained by operations,
  audit, governance, or workflow owners.

This is controlled onboarding, not production automation.

## 2. Current Validation Result

Current minimum source package validation result:

| Alias | Result | Readiness policy | Interpretation |
| --- | --- | --- | --- |
| `deliveries` | pass | `partial_allowed` | Projection provides enough delivery identity/status context to continue source readiness review. |
| `tracking_events` | pass | `partial_allowed` | Projection provides partial logistics event context, but not carrier confirmation. |
| `policies` | pass | `manual_mapping_allowed` | Policy can proceed only as a curated/manual mapping source, not automatic inference. |
| `carrier_updates` | fail | `ready_required` | Missing workflow-critical external carrier confirmation source. |
| `approval_matrix` | fail | `ready_required` | Missing workflow-critical authority and routing source. |

Final result:

```text
fail
```

Decision packet generated:

```text
false
```

This is correct. EKOS should not construct a governed decision packet when the
workflow-critical source package is incomplete.

## 3. Why `carrier_updates` Is Workflow-Critical

Without `carrier_updates`, EKOS cannot determine whether external carrier
confirmation is:

- current;
- stale;
- missing;
- conflicting;
- trustworthy enough to support approval-preparation.

For the delivery-delay workflow, this alias is not decorative context. It is the
source that decides whether EKOS can move beyond "Recommendation only" toward
"Prepare approval-ready correction workflow."

### Required Fields

Minimum required fields:

| Field | Purpose |
| --- | --- |
| `delivery_id` | Links the carrier signal to the delivery being reviewed. |
| `carrier_id` | Identifies the external carrier or 3PL source. |
| `eta_at` or `latest_confirmed_eta` | Carries the latest externally confirmed ETA. |
| `updated_at` | Determines whether confirmation is current or stale. |
| `confirmation_status` | Distinguishes confirmed, pending, missing, rejected, or unknown states. |
| `confirmed_by` or `update_source` | Shows who or what produced the confirmation. |
| `source_system` | Preserves provenance for audit and review. |

Useful additional fields:

- `tracking_number`
- `received_at`
- `source_record_ref`
- `source_channel`
- `raw_payload_ref`
- `carrier_event_id`
- `transport_object_id`

### Possible Sources

Possible `carrier_updates` sources:

| Source | Product interpretation |
| --- | --- |
| LE Tracking custom event main table | Strong candidate if it stores latest carrier signal and confirmation timestamp. |
| LE Tracking event history table | Useful when latest state must be reconstructed from event chronology. |
| 3PL/carrier event feed | Strong candidate if feed identity, timestamp, and delivery linkage are reliable. |
| Carrier portal extract | Practical v1 source if downloaded/exported under controlled process. |
| Manually curated CSV/Excel for v1 demo | Acceptable controlled fallback if source owner and upload provenance are explicit. |
| Email-derived confirmation | Later support only, if governed; should not be treated as core v1 authority. |

### Product Rule

If `carrier_updates` is missing, EKOS may still report that delivery and
tracking context exist. It must not claim that current carrier confirmation is
available.

The safe product behavior is:

```text
carrier_updates missing
-> source package validation fails
-> no decision packet generated from projection alone
-> request carrier update source acquisition or governed manual fallback
```

## 4. Why `approval_matrix` Is Workflow-Critical

Without `approval_matrix`, EKOS cannot know who can review, prepare, approve,
or block the requested business action.

The approval matrix is not just a reporting label. It defines the authority
boundary for the workflow. EKOS can explain evidence without it, but it cannot
reliably route or authorize the requested action boundary.

### Required Fields

Minimum required fields:

| Field | Purpose |
| --- | --- |
| `workflow_id` | Identifies the workflow the matrix applies to. |
| `requested_action` | Describes the business action being governed. |
| `action_category` | Separates recommendation, preparation, approval, and execution boundaries. |
| `required_approver_role` | Identifies who must review or approve the action. |
| `approval_matrix_version` | Supports auditability and version traceability. |
| `can_prepare` | Indicates whether EKOS may prepare an approval-ready packet under current conditions. |
| `can_execute` | Indicates whether any execution or SAP-changing action is allowed. |
| `effective_from` | Defines when the authority rule starts. |
| `effective_to` | Defines when the authority rule expires, if applicable. |

Useful additional fields:

- `matrix_owner`
- `reviewer_role`
- `owner_role`
- `escalation_role`
- `owner_verification_required`
- `source_record_ref`
- `governance_review_cycle`

### Possible Sources

Possible `approval_matrix` sources:

| Source | Product interpretation |
| --- | --- |
| SAP workflow customizing | Useful where approval routes are already represented in SAP workflow configuration. |
| Team-level approval matrix | Practical if operations already maintains action/role routing outside SAP. |
| SOP or policy table | Useful if action boundaries and approver roles are explicitly maintained. |
| GRC/role mapping | Useful for authority and segregation-of-duties context, but may need normalization. |
| Manual governance CSV for v1 | Recommended controlled v1 fallback when no extractable system source exists. |
| Organization rule maintained by audit/governance team | Strong candidate if versioned and owned outside developer control. |

### Product Rule

If `approval_matrix` is missing, EKOS may still explain source evidence. It
must not claim that a requested approval-preparation action is properly routed
or governed.

The safe product behavior is:

```text
approval_matrix missing
-> source package validation fails
-> no decision packet generated from projection alone
-> acquire governed authority source before configured onboarding
```

## 5. Recommended v1 Acquisition Strategy

For v1, EKOS should not wait for perfect SAP integration or assume every source
will come from a single system.

Recommended hybrid source strategy:

| Alias | Recommended v1 acquisition path | Rationale |
| --- | --- | --- |
| `deliveries` | Existing projection, SAP delivery extract, report export, or manual CSV. | Already partially available; enough to identify the primary object and current status. |
| `tracking_events` | Existing projection, custom LE tracking event extract, or event history export. | Already partially available; useful for event context and delay chronology. |
| `carrier_updates` | Custom LE tracking table, 3PL/carrier extract, carrier portal export, or controlled manual CSV. | Must be acquired because freshness cannot be inferred from delivery status alone. |
| `policies` | Curated policy mapping CSV maintained by policy/governance owner. | Manual mapping is acceptable in v1 if source reference and version are explicit. |
| `approval_matrix` | Curated governance CSV maintained by workflow owner, audit, or operations control owner. | Must be acquired because authority routing cannot be inferred safely. |

This is controlled onboarding:

```text
not live production automation
not automatic policy governance
not automatic approval authority inference
```

The value of EKOS at this stage is that it makes missing source requirements
explicit before producing a decision packet.

## 6. Source Trust Levels

EKOS should treat sources differently based on provenance, governance, and
reviewability. The same field value may carry different authority depending on
where it came from.

| Trust level | Example source | Can support recommendation only? | Can support prepare-for-approval? | Notes |
| --- | --- | --- | --- | --- |
| System-recorded SAP source | SAP delivery, event history, workflow customizing, audited SAP extract | Yes | Yes, if fields are current, complete, and governed | Strongest structured source for enterprise review, but still needs source freshness and field quality checks. |
| External carrier/3PL source | Carrier API/export, 3PL EDI, portal extract | Yes | Yes, if timestamped, linked to object, and trusted by policy | Critical for carrier confirmation but may need receipt timestamp and source identity. |
| Governance-curated CSV | Approval matrix or policy mapping maintained by governance owner | Yes | Yes, if versioned and owned | Practical v1 path for authority and policy where system source is not ready. |
| Manually uploaded operator file | Controlled CSV/Excel upload by operations user | Yes | Maybe, only with reviewer trace and governance approval | Useful fallback, but should usually keep result at `review_required` unless explicitly reviewed. |
| Email-derived or unstructured source | Email confirmation, copied text, informal note | Limited | No by default | Later possible support only if governed; should not become silent authority. |

Recommended action boundary:

- Recommendation only can use lower-trust or partial sources when the report
  clearly states limitations.
- Prepare-for-approval requires the workflow-critical sources to pass minimum
  package validation and preserve provenance.
- Execution or SAP-changing action remains outside the v1 boundary unless
  explicitly governed and approved.
- `review_required` is the correct status when source trust or review artifact
  is insufficient.

## 7. Minimum Source Package v1

The minimum complete package for the delivery-delay / carrier-confirmation
workflow is:

| Source package item | Required alias | Minimum purpose |
| --- | --- | --- |
| Delivery extract | `deliveries` | Primary object identity, status, planned/actual timing, and source reference. |
| Tracking event extract | `tracking_events` | Delay chronology and operational context. |
| Carrier update extract | `carrier_updates` | External confirmation, ETA, timestamp, freshness, and source trust. |
| Policy mapping file | `policies` | Confirmation window, required preconditions, blocked actions, source SOP reference. |
| Approval matrix file | `approval_matrix` | Authority route, approver role, preparation boundary, execution boundary. |

This package can be assembled from mixed sources. The configured onboarding
layer should not care whether records came from SAP extract, projection,
fragment fixture, RFC row, curated governance file, or manual upload. It should
care whether the normalized aliases exist and pass readiness rules.

## 8. Governance Requirement

Approval matrix and policy mapping must not be maintained casually by
developers.

Developers can build validators, source providers, config interpreters, and
report renderers. They should not own the enterprise meaning of policy,
approval authority, or action boundaries.

Likely ownership model:

| Role | Responsibility |
| --- | --- |
| SAP operations/process owner | Validates operational source meaning, delay workflow readiness, and next-action language. |
| Audit/control reviewer | Validates reviewability, control evidence, policy traceability, and approval boundary. |
| Workflow owner | Owns requested action categories and approval-preparation process meaning. |
| IT/security | Controls source access, credentials, IAM, and deployment security. |
| Data steward | Owns source quality, field definitions, lineage, and refresh expectations. |
| EKOS platform team | Owns the platform engine, validators, source-provider boundary, and packet/report generation. |

Governed files such as policy mappings and approval matrices should include:

- owner;
- version;
- effective date range;
- source reference;
- last reviewed timestamp;
- review status;
- change history or approval note.

## 9. Next Implementation Recommendation

The next EKOS implementation should be a controlled supplemental source package
demo, not live SAP integration.

Recommended sequence:

1. Keep projected `deliveries` and `tracking_events` as partial source inputs.
2. Add curated synthetic `carrier_updates` as a controlled supplemental source.
3. Add curated synthetic `approval_matrix` as a controlled governance source.
4. Keep `policies` as curated policy mapping with explicit SOP/version fields.
5. Run minimum source package validation again.
6. Allow configured onboarding only if validation passes.
7. Preserve report language that distinguishes source acquisition from
   production approval.

This sequence tests the next product question:

```text
Can EKOS safely combine projected SAP/logistics assets with curated governance
sources to complete the minimum source package before decision-packet
construction?
```

It should not skip the validator. Passing the validator should become the gate
between source acquisition and configured onboarding.

Rejected direction:

```text
Add more decision logic to work around missing carrier_updates or approval_matrix.
```

Reason:

That would hide the product problem. Missing carrier confirmation and missing
authority routing are not reasoning gaps; they are source package gaps.

## 10. Claim Boundary

This is a source acquisition plan.

It does not connect to live SAP.

It does not prove production readiness.

It does not automate approval governance.

It does not solve carrier data quality.

It does not add RAG.

It does not assume missing sources can be inferred automatically.

It does not modify EKOS implementation.

It only defines how to close missing source gaps safely before configured
onboarding generates delivery-delay decision packets.
