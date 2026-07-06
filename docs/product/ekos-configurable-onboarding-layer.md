# EKOS Configurable Onboarding Layer

Status: Product architecture document
Repository role: North Star product strategy and platform architecture
Implementation repository: `2000silpeed/ekos-sap-knowledge-os`
Reference issue: North Star Issue #4, "Product Gap: real enterprise data
onboarding for EKOS decision packets"

This document reframes EKOS from a custom-coded SAP logistics demo into a
configurable enterprise decision-packet onboarding platform.

It does not implement code, add SAP logistics fields, create sample data, add
RAG, create a benchmark, or claim production readiness.

## 1. Executive Summary

The current SAP-like demo is valuable as a reference implementation. It proves
that EKOS can turn structured logistics extracts into evidence objects, a
decision packet, a human-readable report, and a governance status.

But EKOS must not become a custom-coded rule engine for one SAP logistics
scenario. If every new workflow requires developers to hand-code fields,
evidence rules, policy mappings, blocking risks, authority labels, and report
phrasing, EKOS will not scale inside a real enterprise.

The product direction should be:

```text
Configurable enterprise decision-packet builder
```

That means EKOS should let an implementation team configure how a workflow maps
from enterprise source data to:

- business objects
- evidence objects
- policy constraints
- blocking risks
- authority boundaries
- review and governance states
- human-readable reports

The SAP logistics demo remains useful. It should now be treated as a reference
workflow that teaches what must become configurable.

## 2. Why Bespoke Implementation Does Not Scale

Bespoke implementation is acceptable for a first proof of value. It is not a
product architecture.

If each workflow requires custom engineering, the cost grows with every new
process:

- every workflow needs new fields
- every process needs new evidence rules
- every policy needs mapping
- every exception needs blocking logic
- every report needs business labels
- every source requires custom transformation decisions
- every authority boundary requires developer interpretation
- developers become the bottleneck
- enterprise adoption becomes consulting-heavy

The result would be a high-quality demo that is hard to deploy repeatedly.

The failure mode is not that EKOS cannot reason about one delivery-delay case.
The failure mode is that EKOS becomes too expensive to onboard across multiple
enterprise workflows.

## 3. What The Current Demo Actually Proves

The current demo proves the target artifact and user experience:

```text
source data
-> evidence
-> policy
-> allowed action
-> blocked action
-> governance status
-> human-readable report
```

It proves that a manager-facing report can answer:

- what object is being reviewed
- what action is allowed now
- what action is blocked
- why the action is blocked
- what evidence was used
- what policy boundary applies
- what human action is required next
- why governance remains `review_required`

It does not prove scalable onboarding.

The demo does not yet prove that a team can onboard the next workflow without
bespoke Python changes, new hard-coded report text, and custom rule logic. That
is the next product risk.

## 4. Product Reframing

Bad direction:

```text
Custom-coded SAP logistics rule engine
```

Why this is the wrong product:

- it overfits one scenario
- it makes developers the permanent onboarding path
- it hides business semantics inside code
- it makes policy changes slow to review
- it makes audit and governance depend on implementation details

Correct direction:

```text
Configurable enterprise decision-packet builder
```

Why this is the right product:

- source mappings become explicit
- evidence rules become reviewable
- policy constraints become versioned product artifacts
- blocking risks become governed configuration
- business labels can be maintained by the process owner
- engineering focuses on the platform, validators, connectors, and governance
  controls

The core product is not a SAP-specific rule set. The core product is the
governed transformation from enterprise data and policy into decision packets.

## 5. Configurable Onboarding Model

The onboarding layer should make the following layers configurable.

### A. Source Schema Mapping

Configuration should define how raw fields are interpreted.

Configurable elements:

- CSV, table, API, or extract fields
- source aliases
- required fields
- optional fields
- data freshness fields
- timestamp fields
- source record references
- data-quality expectations

Example questions the configuration should answer:

- Which source field is the delivery identity?
- Which timestamp is the carrier event time?
- Which timestamp is the enterprise receipt time?
- Which fields are required before a packet is reviewable?
- Which fields are optional context?

### B. Object Mapping

Configuration should define the enterprise objects produced from source rows.

Configurable elements:

- primary business object
- related objects
- object identity fields
- relationship fields
- source-to-object cardinality
- missing relationship behavior

Example objects:

- Delivery
- CarrierUpdate
- PolicyConstraint
- BlockingRisk
- ReviewRecord
- DecisionPacket

### C. Evidence Rules

Configuration should define how evidence objects are created.

Configurable elements:

- `evidence_id`
- condition
- business meaning
- source field dependencies
- freshness logic
- confidence or trust level
- whether the evidence supports a claim, weakens a claim, or blocks an action
- source record reference requirements

Evidence rules should be visible to operations, audit, and governance reviewers.
If an evidence rule cannot be explained to a reviewer, it should not silently
control an enterprise action boundary.

### D. Policy Constraints

Configuration should define normalized policy constraints.

Configurable elements:

- `policy_id`
- policy version
- SOP reference
- required preconditions
- blocked actions
- authority condition
- effective date range
- policy owner
- human review requirement

Policy configuration does not mean the system understands every policy document
automatically. It means the relevant policy boundary is represented explicitly
enough for EKOS to build a governed decision packet.

### E. Blocking Risks

Configuration should define risks derived from evidence and policy.

Configurable elements:

- `risk_id`
- trigger condition
- affected action
- severity
- source evidence dependencies
- whether the risk blocks preparation, approval, or execution
- wording for human-readable explanation

Blocking risks should be action-specific. A risk that blocks execution may not
always block recommendation. A risk that blocks approval-preparation may still
allow information requests.

### F. Authority Boundary

Configuration should define the business action boundary.

Configurable elements:

- action levels
- business labels
- allowed action
- blocked action
- required approver role
- escalation rule
- maximum safe action when preconditions are unmet
- prohibited action categories

The SAP logistics workflow uses labels such as:

- Recommendation only
- Prepare for approval
- Execute or commit

Other enterprise workflows may need different labels, but the product pattern is
the same: EKOS should explain what the enterprise is currently allowed to do.

### G. Review And Governance

Configuration should define review requirements and governance transitions.

Configurable elements:

- review status
- reviewer fields
- review artifact requirements
- governance status transitions
- approval artifact requirements
- conditions for `review_required`
- conditions for `approved`
- conditions that prevent approval claims

The configuration must prevent synthetic, missing, or incompatible review data
from being treated as approval.

### H. Human-Readable Report Labels

Configuration should define the wording used in reports.

Configurable elements:

- manager-facing labels
- allowed action wording
- blocked action wording
- requested action wording
- evidence explanation wording
- policy explanation wording
- next-action wording
- claim boundary wording

This matters because enterprise users should not need to understand internal
delegation numbers before understanding the business decision.

## 6. Minimum Configuration Schema

The schema below is conceptual. It is not implementation code and does not
define a committed file format. It shows the minimum shape a workflow
configuration would need so the delivery-delay scenario could be onboarded
without bespoke rule code.

```yaml
workflow_id: delivery_delay_carrier_confirmation_v1
workflow_name: Delivery delay / carrier confirmation readiness
status: draft_configuration

source_files:
  deliveries:
    path: data/demo/sap_like/deliveries.csv
    required: true
    source_family: delivery_header
    required_fields:
      - delivery_id
      - current_status
      - planned_handoff_at
      - planned_delivery_date
  carrier_updates:
    path: data/demo/sap_like/carrier_updates.csv
    required: true
    source_family: carrier_3pl_update
    required_fields:
      - delivery_id
      - carrier_id
      - tracking_number
      - latest_confirmed_eta
      - carrier_confirmation_timestamp
      - confirmation_status
  policies:
    path: data/demo/sap_like/policies.csv
    required: true
    source_family: normalized_policy
    required_fields:
      - policy_id
      - policy_version
      - sop_reference
      - confirmation_window_hours
      - requires_current_confirmation

primary_object:
  object_type: Delivery
  identity_field: deliveries.delivery_id
  display_label_template: "Delivery {delivery_id}"
  status_field: deliveries.current_status

object_mappings:
  CarrierUpdate:
    identity_field: carrier_updates.carrier_update_id
    object_refs:
      delivery_id: carrier_updates.delivery_id
    fields:
      carrier_id: carrier_updates.carrier_id
      tracking_number: carrier_updates.tracking_number
      latest_confirmed_eta: carrier_updates.latest_confirmed_eta
      confirmation_timestamp: carrier_updates.carrier_confirmation_timestamp
      confirmation_status: carrier_updates.confirmation_status

evidence_rules:
  - evidence_id: delivery_status_event
    condition: "deliveries.current_status == 'DELAYED'"
    business_meaning: "The delivery is delayed and needs operations attention."
    source_dependencies:
      - deliveries.delivery_id
      - deliveries.current_status
    effect: supports_review

  - evidence_id: carrier_update_stale
    condition: "age(carrier_updates.carrier_confirmation_timestamp) > policies.confirmation_window_hours"
    business_meaning: "The carrier confirmation is too old to support approval preparation."
    source_dependencies:
      - carrier_updates.carrier_confirmation_timestamp
      - policies.confirmation_window_hours
    freshness_logic: confirmation_window_hours
    effect: blocks_action

  - evidence_id: policy_confirmation_required
    condition: "policies.requires_current_confirmation == true"
    business_meaning: "Policy requires current external confirmation before correction preparation."
    source_dependencies:
      - policies.policy_id
      - policies.policy_version
      - policies.sop_reference
    effect: sets_authority_boundary

policy_constraints:
  - policy_id: delay_confirmation_policy
    version_field: policies.policy_version
    sop_reference_field: policies.sop_reference
    required_preconditions:
      - obtain_current_carrier_confirmation
      - verify_delay_owner
    blocked_actions_when_unmet:
      - prepare_approval_ready_correction_workflow
      - execute_or_commit
    allowed_actions_when_unmet:
      - recommend
      - request_missing_information

blocking_risks:
  - risk_id: stale_external_event
    trigger_condition: "carrier_update_stale == true"
    affected_action: prepare_approval_ready_correction_workflow
    severity: high
    blocks: preparation

  - risk_id: insufficient_confirmation_for_execution_plan
    trigger_condition: "carrier_update_stale == true and policy_confirmation_required == true"
    affected_action: prepare_approval_ready_correction_workflow
    severity: high
    blocks: preparation

authority_boundary:
  action_levels:
    recommendation_only:
      internal_level: 1
      business_label: Recommendation only
      allowed_action: "Recommend likely carrier-delay classification and request current carrier confirmation."
    prepare_for_approval:
      internal_level: 2
      business_label: Prepare for approval
      blocked_action: "Prepare approval-ready correction workflow."
    execute_or_commit:
      internal_level: 3
      business_label: Execute or commit
      allowed_in_v1: false
  requested_action: prepare_for_approval
  fallback_when_blocked: recommendation_only
  required_approver_role_field: approval_matrix.approval_role

decision_packet_shape:
  required_sections:
    - primary_object
    - evidence_ids
    - policy_ids
    - execution_preconditions
    - blocking_risks
    - allowed_action
    - blocked_action
    - governance_status
    - review_trace

report_template_labels:
  manager_view_title: "Manager View"
  current_allowed_action_label: "Current allowed action"
  blocked_action_label: "Blocked action"
  why_blocked_label: "Why blocked"
  governance_result_label: "Governance result"
  claim_boundary:
    - "Synthetic SAP-like data"
    - "Not live SAP integration"
    - "Not production approval"
    - "No provider calls"
    - "No human validation claim"

governance_rules:
  default_status: review_required
  approval_requires:
    - compatible_human_review_artifact
    - required_approver_role
    - no_active_preparation_blockers
  missing_review_artifact_status: review_required
  synthetic_demo_status: review_required
```

This schema should remain understandable to business, SAP, governance, and
engineering stakeholders. If the configuration becomes understandable only to
developers, it has failed the product objective.

## 7. Roles And Responsibilities

Configurable onboarding does not mean every user configures everything. It means
each role owns the parts it is qualified to define.

| Role | Owns | Should not own |
| --- | --- | --- |
| Business user | Business labels, allowed and blocked action wording, operational next actions, report clarity | Source access, security controls, complex joins |
| SAP consultant / process expert | Source field mapping, object relationships, process status meaning, SAP object semantics | Final governance policy or security approval |
| Governance / audit | Policy constraints, approval matrix, review requirements, claim boundaries, audit trace requirements | Raw connector implementation |
| IT / security | Source access, credentials, IAM, audit logging, production deployment controls, data-retention boundaries | Business meaning of the workflow |
| EKOS developer | Platform engine, configuration interpreter, validators, report renderer, connector framework | One-off business labels or policy decisions hidden in code |

The separation matters because EKOS should not turn business policy into hidden
application logic. The platform should provide guardrails and validation. The
enterprise should own the meaning, policy, and authority boundaries.

## 8. Onboarding Workflow For A New Scenario

A repeatable onboarding workflow should look like this:

1. Select workflow slice.
2. Upload or connect sample extracts.
3. Map source fields.
4. Define primary object and relationships.
5. Define evidence rules.
6. Define policy constraints.
7. Define blocking risks.
8. Define authority boundaries.
9. Generate decision packet.
10. Generate human-readable report.
11. Review with business user.
12. Promote to governed configuration only after validation.

The promotion step is important. A draft configuration should not become a
governed workflow merely because it produces a plausible report. It should pass
field realism review, governance review, and technical validation.

## 9. What Still Requires Engineering

Configuration does not remove engineering work. It changes where engineering
effort should go.

Engineering is still required for:

- new source connectors
- complex joins
- advanced temporal logic
- security integration
- production audit logging
- UI and workflow system integration
- permission model
- validation framework
- configuration versioning
- data lineage
- error handling and observability
- deployment controls

The platform must also prevent unsafe configurations. A no-code interface that
lets users accidentally grant execution authority would be worse than bespoke
code.

## 10. What Can Become No-Code / Low-Code

The following can plausibly become no-code or low-code once the platform engine
exists:

- field mapping
- source aliases
- required and optional field declarations
- evidence rule definition for simple conditions
- policy constraint registration
- business labels
- report wording
- basic blocking rules
- workflow-specific glossary
- authority boundary labels
- next-action wording
- claim boundary wording

These should still be governed. Low-code does not mean uncontrolled. Policy and
authority changes should remain reviewable, versioned, and auditable.

## 11. Relationship To RAG

RAG remains an optional support layer for unstructured policy, SOP, historical
case, or audit-comment lookup.

RAG should feed candidate policy and evidence references into the configuration
model. It should not become the decision authority.

Correct relationship:

```text
RAG retrieves candidate references
-> EKOS grounds them into configured evidence or policy objects
-> EKOS applies configured authority boundaries
-> EKOS emits a governed decision packet
```

Incorrect relationship:

```text
RAG answer
-> enterprise decision
```

The configurable onboarding layer strengthens the RAG boundary. Retrieved text
may help fill or suggest policy references, but it must still pass through
explicit configuration, grounding, review, and governance.

## 12. Product Roadmap Implication

The roadmap should shift from adding more bespoke logistics rules to building
onboarding leverage.

### v1 Current

Reference implementation and deterministic demo.

Purpose:

- prove the target artifact
- show the manager-facing UX
- demonstrate source data to evidence to decision packet to report
- maintain strict claim boundaries

### v1.1

Configurable schema design and static YAML-driven workflow.

Purpose:

- make the delivery-delay workflow declarative
- move source mappings and report labels out of code
- validate whether configuration can express the current demo without losing
  reviewability

### v1.2

Config-driven SAP-like onboarding engine.

Purpose:

- interpret workflow configuration
- produce evidence objects and decision packets from configured mappings
- validate required fields and blocking risks
- preserve the current no-provider, no-RAG structured-data boundary

### v1.5

UI-assisted configuration and validation worksheet.

Purpose:

- let business, SAP, governance, and IT reviewers collaborate on workflow
  configuration
- connect configuration review to the data-model validation worksheet
- make field priority and blocking behavior reviewable before promotion

### v2

Governed enterprise onboarding platform with connectors, approvals, audit trail,
and optional RAG support.

Purpose:

- support multiple workflow slices
- version configurations
- integrate connectors and IAM
- preserve audit trail
- add optional RAG support for SOP and historical-case discovery without making
  RAG the decision authority

## 13. Key Product Risk

The biggest risk is not whether EKOS can generate a good report.

The biggest risk is whether EKOS can make new workflow onboarding repeatable
without bespoke engineering.

If EKOS cannot make workflow onboarding repeatable, it becomes a consulting
project. If it can, it becomes enterprise decision infrastructure.

The next product question is therefore:

```text
Can a new governed decision-packet workflow be configured, reviewed, validated,
and promoted without writing workflow-specific code?
```

That question is not answered yet.

## 14. Claim Boundary

This is a product architecture document.

It is not an implementation.

It does not prove scalability.

It does not replace actual SAP integration.

It does not claim no-code is fully achievable.

It does not add RAG.

It does not create sample data.

It does not create a benchmark.

It does not claim production readiness.

It does not modify EKOS.

It defines the product direction that should guide future EKOS implementation
work in `2000silpeed/ekos-sap-knowledge-os`.
