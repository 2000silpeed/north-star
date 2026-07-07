# EKOS Projection Gap Closure Plan

Status: Product/data architecture plan
Repository role: North Star product strategy and SAP operations data architecture
Implementation repository: `2000silpeed/ekos-sap-knowledge-os`
Reference issue: North Star Issue #4, "Product Gap: real enterprise data onboarding for EKOS decision packets"
Snapshot date: 2026-07-07

This document analyzes the projection gap discovered by the EKOS SAP asset
projection prototype:

```bash
python -m ekos sap-assets project --workflow delivery_delay_confirmation
```

It does not modify EKOS implementation, create code, create sample data, add
RAG, connect to live SAP, or claim production readiness.

## 1. Executive Summary

The SAP asset projection prototype is valuable because it shows what EKOS can
and cannot currently derive from existing fixtures and adapters.

Current result:

| Alias | Projection status |
| --- | --- |
| `deliveries` | partial |
| `tracking_events` | partial |
| `policies` | needs manual mapping |
| `carrier_updates` | missing |
| `approval_matrix` | missing |

This is a useful failure. It prevents EKOS from pretending that all
decision-critical workflow inputs can be inferred from generic SAP asset
fixtures.

The result means EKOS should not proceed directly to
`FragmentStoreSourceProvider` yet. A FragmentStore provider would move the
source boundary, but it would not solve the projection gap by itself. The
missing aliases are not merely storage problems. They are workflow-contract
problems:

- Which source supplies current carrier confirmation?
- Which governed source supplies approval authority?
- Which policy row actually represents the delay confirmation SOP?
- Which missing aliases should block configured onboarding before a packet is
  constructed?

The next product step should be a minimum source package validator that checks
whether all workflow-critical aliases exist before configured onboarding runs.

## 2. Why This Matters

Configured onboarding requires normalized workflow aliases.

For the delivery-delay / carrier-confirmation workflow, the configured layer
expects aliases such as:

- `deliveries`
- `tracking_events`
- `carrier_updates`
- `policies`
- `approval_matrix`

If critical aliases are missing, EKOS cannot safely construct decision packets.

For this workflow:

- `carrier_updates` determines freshness and current external confirmation.
- `approval_matrix` determines authority boundary and routing.
- `policies` determines what preconditions are required and which actions are
  blocked when they are unmet.

The partial projection result should therefore be treated as a product signal,
not an implementation inconvenience.

Without `carrier_updates`, EKOS may know that a delivery is delayed, but it
cannot know whether carrier confirmation is current.

Without `approval_matrix`, EKOS may describe the evidence, but it cannot route
or define the approval-preparation boundary reliably.

Without a workflow-specific policy mapping, EKOS may have policy-shaped data,
but it cannot claim that the policy constrains delivery-delay correction
workflow preparation.

## 3. Alias Readiness Table

| Alias | Current projection status | Why it matters | Possible source | Product decision |
| --- | --- | --- | --- | --- |
| `deliveries` | partial | Establishes the primary delivery/DO object, status, timing, shipping point, and plant. | SAP delivery extract, `LIKP`-like row, CDS/OData view, RFC row, delivery report export, manual CSV. | Use projection when identity and status are present, but require missing context to be explicit. |
| `tracking_events` | partial | Provides delay event chronology and supports the delivery-status evidence. | Custom LE tracking table, event history table, LE timeline adapter, tracking extract, manual event CSV. | Accept partial projection as supporting context; do not treat it as carrier confirmation. |
| `carrier_updates` | missing | Determines whether external carrier confirmation is current, stale, missing, or conflicting. | Carrier/3PL feed, carrier portal export, custom tracking update table, event history, manual confirmation CSV. | Workflow-critical. Must be sourced or explicitly marked missing before packet construction. |
| `policies` | needs manual mapping | Defines confirmation window, required preconditions, and blocked actions. | Curated policy mapping CSV, SOP normalization, policy table, governance-maintained rule file. | Do not infer from unrelated policy fixture. Require curated mapping for v1. |
| `approval_matrix` | missing | Defines approver role, reviewer role, owner verification requirement, and authority boundary routing. | Workflow customizing, approval matrix spreadsheet, GRC/role mapping, SOP table, governance CSV. | Workflow-critical. Require manual/governed source for v1. |

## 4. Carrier Updates Gap

### Possible Sources

The `carrier_updates` alias should come from a source that represents the latest
external carrier signal or a governed manual substitute.

Possible sources:

- LE Tracking custom event main table
- LE Tracking event history table
- 3PL or carrier event feed
- carrier portal extract
- carrier API or EDI payload export
- manual CSV or Excel upload
- email-derived confirmation later, only if governed and source-backed

The projection prototype found delivery and logistics event context, but it did
not find the external ETA update timestamp and confirmation state needed for
the current workflow.

### Required Fields

Minimum fields:

- `delivery_id`
- `carrier_id`
- `eta_at` or `latest_confirmed_eta`
- `updated_at`
- `confirmation_status`
- `confirmed_by` or `update_source`
- `source_system`

Recommended reviewability fields:

- `source_record_ref`
- `received_at`
- `source_channel`
- `raw_payload_ref`
- `tracking_number`
- `transport_object_id`

### Product Interpretation

Without `carrier_updates`, EKOS can detect some delivery-delay context, but it
cannot determine whether carrier confirmation is current.

That means EKOS should not construct a packet that implies Level 2
approval-preparation readiness. The correct behavior is to identify the missing
alias, explain why it matters, and require a source package correction or
manual fallback.

## 5. Approval Matrix Gap

### Possible Sources

The `approval_matrix` alias should come from a governed source of authority,
not from inferred text or generic role names.

Possible sources:

- SAP workflow customizing
- team-level approval matrix
- SOP or policy table
- GRC/role mapping
- manual approval matrix CSV for v1
- organization rule maintained by the governance team

### Required Fields

Minimum fields:

- `workflow_id`
- `action_category`
- `requested_action`
- `required_approver_role`
- `approval_matrix_version`
- `can_prepare`
- `can_execute`
- `effective_from`
- `effective_to`

Recommended reviewability fields:

- `owner_role`
- `reviewer_role`
- `approver_role`
- `owner_verification_required`
- `matrix_owner`
- `source_record_ref`

### Product Interpretation

Without `approval_matrix`, EKOS can explain evidence but cannot route or define
the authority boundary reliably.

This matters because the central EKOS product promise is not just explanation.
It is governed decision-packet construction. A packet that cannot identify the
authority route should remain incomplete or `review_required`; it should not
claim preparation readiness.

## 6. Policy Manual Mapping Gap

The existing `sap_policy` fixture is useful because it shows policy-shaped
assets can be represented. But it is not necessarily the specific delay
confirmation SOP.

For the delivery-delay workflow, policy mapping must answer:

- Does policy require current external carrier confirmation?
- What is the confirmation window?
- Does policy require delay owner verification?
- Which actions are allowed when confirmation is missing?
- Which actions are blocked when confirmation is missing?
- Which policy version and SOP reference should a reviewer inspect?

Manual mapping means:

1. Identify the relevant policy paragraph, SOP row, or governance rule.
2. Assign a stable `policy_id`.
3. Assign `policy_version`.
4. Assign `sop_reference`.
5. Define required preconditions.
6. Define blocked actions if preconditions are missing.
7. Record policy owner and effective dates.

This does not mean policy stays manual forever. It means v1 should not pretend
that unrelated policy fixtures or retrieved text can automatically become a
workflow authority rule.

## 7. Recommended v1 Practical Approach

For v1, do not wait for perfect SAP sources.

Use a hybrid source approach:

| Alias | Recommended v1 source |
| --- | --- |
| `deliveries` | Existing SAP/LE tracking projection or delivery extract. |
| `tracking_events` | Existing SAP/LE tracking projection, custom event table, or event history extract. |
| `carrier_updates` | Custom tracking table, carrier/3PL extract, carrier portal export, or controlled manual CSV. |
| `policies` | Curated policy mapping CSV maintained by policy/governance owner. |
| `approval_matrix` | Curated governance CSV or approval matrix maintained by operations/governance. |

This is not production automation.

It is controlled onboarding.

The product value is that EKOS can make the source package explicit and
reviewable before it creates a governed decision packet.

## 8. Minimum Source Package For Realistic Demo

The smallest data package needed to make projection complete for the
delivery-delay / carrier-confirmation workflow is:

1. Delivery extract
2. Tracking event extract
3. Carrier update extract
4. Policy mapping file
5. Approval matrix file

Minimum package:

| File or source | Required alias | Purpose |
| --- | --- | --- |
| Delivery extract | `deliveries` | Primary object identity, status, planned/actual timing. |
| Tracking event extract | `tracking_events` | Delay evidence and event chronology. |
| Carrier update extract | `carrier_updates` | External ETA, confirmation timestamp, freshness classification. |
| Policy mapping file | `policies` | Confirmation window, preconditions, blocked actions. |
| Approval matrix file | `approval_matrix` | Authority route, approver role, owner verification requirement. |

If any of the last three are missing, configured onboarding should not silently
construct a complete decision packet. It should fail clearly, enter a manual
fallback path, or produce an explicit incomplete-readiness artifact.

## 9. How This Connects To Configured Onboarding

Projection should produce the aliases expected by YAML configuration.

Configured onboarding should not care whether aliases came from:

- SAP fixture
- RFC row
- fragment
- manual CSV
- curated governance file
- controlled upload

The interface should be source-neutral:

```text
source asset
-> projected alias records
-> minimum source package validation
-> configured onboarding
-> evidence objects
-> blocking risks
-> authority boundary
-> decision packet
```

This keeps the responsibility split clean:

- Projection prepares normalized records.
- Minimum source package validation checks completeness.
- Configured onboarding applies evidence, policy, blocking risk, and authority
  rules.

Configured onboarding should not be responsible for guessing where missing
workflow-critical aliases should have come from.

## 10. Next Implementation Recommendation

Do not implement `FragmentStoreSourceProvider` yet.

Two plausible next EKOS implementation options exist.

### Option A: Add Projection Support For Missing Aliases Using Local Synthetic Sources

This option would add local synthetic source files or fixture projections for:

- `carrier_updates`
- `approval_matrix`

Benefits:

- makes the projection output look complete;
- provides more end-to-end demo continuity;
- can exercise the projection code path with all configured aliases present.

Risks:

- may create false confidence if the source package is still hand-shaped;
- may hide the central product problem: missing critical aliases should block
  onboarding;
- may encourage adding more bespoke synthetic source handling before the source
  contract is stable.

### Option B: Create A Minimum Source Package Validator

This option would validate that the workflow has all required aliases and
required fields before configured onboarding runs.

Benefits:

- prevents false confidence when `carrier_updates`, `policies`, or
  `approval_matrix` are missing;
- makes source completeness a product artifact, not an implicit runtime error;
- helps business, SAP, governance, and IT reviewers see exactly what source is
  missing;
- preserves the boundary between projection and decision logic;
- prepares the contract that future `FragmentStoreSourceProvider` or
  `RfcRowSourceProvider` must satisfy.

Risks:

- does not immediately create a prettier demo;
- may stop configured onboarding earlier when source data is incomplete;
- requires careful messaging so a validation failure is treated as useful
  governance, not product failure.

### Recommendation

Start with Option B: minimum source package validator.

Reason:

The projection prototype already showed the most important product risk:
workflow-critical aliases can be missing even when EKOS can represent useful
SAP-shaped assets.

The next implementation should therefore make incompleteness explicit before
building more projection convenience. A validator should answer:

- Which aliases are required for this workflow?
- Which aliases are present?
- Which aliases are partial?
- Which fields are missing?
- Which missing fields block configured onboarding?
- Which missing fields only reduce context?
- Which manual fallback source can satisfy the gap?

Only after the validator exists should EKOS add richer projection support for
`carrier_updates` and `approval_matrix`.

## 11. Claim Boundary

This is a product/data architecture plan.

It does not prove production readiness.

It does not connect to live SAP.

It does not modify EKOS implementation.

It does not create source files or sample data.

It does not automate policy governance.

It does not solve approval authority management.

It does not add RAG as a core decision layer.

It does not assume the missing aliases can be automatically inferred.

It only defines how to close the projection gap discovered by the dry-run and
projection prototype.
