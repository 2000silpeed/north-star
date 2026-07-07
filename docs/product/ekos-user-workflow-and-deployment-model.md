# EKOS User Workflow And Deployment Model

Status: Product/UX architecture document
Repository role: North Star product strategy and user workflow definition
Implementation repository: `2000silpeed/ekos-sap-knowledge-os`
Reference issue: North Star Issue #4, "Product Gap: real enterprise data onboarding for EKOS decision packets"
Snapshot date: 2026-07-08

This document defines how EKOS should be used by enterprise users after
deployment. It maps the current CLI/demo pipeline in the EKOS implementation
repository to a future product experience.

It does not implement a UI, modify EKOS code, create a benchmark, claim
production readiness, claim live SAP integration, claim autonomous approval, or
make RAG the core product.

## 1. Executive Summary

EKOS is not a chat-only assistant.

EKOS is a governed decision workflow system.

A user selects a business workflow, provides or connects the required source
package, and asks EKOS whether the case can be evaluated. EKOS first checks
whether the required inputs are sufficient. Only then does it generate a
decision packet and human-readable report.

The report answers in business language:

- what is allowed;
- what is blocked;
- why it is blocked or allowed;
- what evidence was used;
- what policy or authority boundary applies;
- what data or review is still missing.

The product principle is simple:

```text
EKOS should not answer when required enterprise inputs are missing.
EKOS should explain what is missing before judgment.
```

## 2. Core User Journey

The main user journey is:

1. Select workflow.
2. Enter case ID or business object.
3. Check required data sources.
4. Build or validate the source package.
5. Generate a decision packet if inputs are sufficient.
6. View the human-readable governance report.
7. Request human review or collect missing data.
8. Re-run after data is updated.
9. Archive the decision packet and provenance.

The user does not start by asking a model an open-ended question. The user
starts by selecting a governed workflow.

## 3. Product Modes

EKOS should expose three product modes.

### A. Check Mode

Question:

```text
Can EKOS safely evaluate this case?
```

Output:

- source package completeness;
- missing aliases;
- missing evidence;
- missing policy or approval mapping;
- source freshness problems;
- pass/fail result;
- recommended next source action.

Check Mode prevents false confidence. It is the product surface for minimum
source package validation.

### B. Decide Mode

Question:

```text
What is currently allowed or blocked?
```

Output:

- allowed action;
- blocked action;
- evidence;
- policy;
- blocking risks;
- authority boundary;
- decision packet;
- provenance.

Decide Mode should run only after Check Mode passes for the workflow-required
source package.

### C. Review Mode

Question:

```text
Can a human reviewer accept, reject, or request more data?
```

Output:

- `review_required`;
- reviewer notes;
- approval boundary;
- audit trail;
- next action;
- rerun requirement if source data changes.

Review Mode does not mean autonomous approval. It means EKOS provides a
reviewable packet and records human governance state.

## 4. User Roles

### A. SAP Operations User

The SAP operations user:

- selects the workflow;
- enters a case ID, delivery number, IDoc number, material/plant, or similar
  business object;
- checks what EKOS says is allowed or blocked;
- uploads missing operational data when needed;
- requests review from the appropriate owner.

This user should see business language first, not JSON or internal delegation
numbers.

### B. Team Lead / Approval Owner

The team lead or approval owner:

- reviews the decision packet;
- checks whether approval-preparation can proceed;
- confirms whether required preconditions are met;
- accepts, rejects, or asks for more data;
- does not rely on EKOS for autonomous execution.

This role owns the business decision boundary. EKOS should support the review,
not replace it.

### C. Audit / Control Reviewer

The audit or control reviewer:

- checks evidence lineage;
- checks policy and approval mapping;
- verifies why an action was blocked or allowed;
- reconstructs which source records and rules produced the packet;
- evaluates whether the review trail is reproducible.

This role needs drill-down access to provenance and validation failures.

### D. SAP Consultant / Process Expert

The SAP consultant or process expert:

- configures workflow mappings;
- defines source aliases;
- maps SAP fields to EKOS concepts;
- validates process semantics;
- confirms that source fields mean what EKOS says they mean.

This role should not be required for every individual case, but is needed to
onboard or revise workflows.

### E. IT / Security / Platform Team

The IT, security, or platform team:

- manages connectors;
- manages source access;
- controls deployment;
- monitors logs and security boundaries;
- enforces read-only or write restrictions;
- owns IAM and production controls.

This role ensures EKOS is deployed as governed enterprise infrastructure, not a
loose desktop script.

## 5. Current CLI Pipeline To Future Product UI

| Current CLI command | Product UI equivalent | User meaning |
| --- | --- | --- |
| `python -m ekos sap-assets dry-run` | Data source discovery | What SAP-like assets are available? |
| `python -m ekos sap-assets project --workflow ...` | Prepare workflow inputs | Convert raw assets into workflow records. |
| `python -m ekos sap-assets validate-source-package ...` | Check required data | Can EKOS evaluate this case? |
| `python -m ekos sap-assets build-source-package ...` | Add missing controlled sources | Complete the source package. |
| `python -m ekos data-onboard configured --config ...` | Generate decision packet | Apply workflow rules. |
| `python -m ekos demo --enterprise-workflow --decision-packet ...` | View governance report | See allowed/blocked action and why. |

The CLI is the current implementation interface. The future product UI should
hide command mechanics and expose the business workflow behind each command.

## 6. Example User Flow: Delivery Delay

Workflow:

```text
Delivery Delay / Carrier Confirmation
```

Plain-language flow:

1. User selects Delivery Delay / Carrier Confirmation.
2. User enters `Delivery 80001241`.
3. EKOS checks the data package.
4. EKOS says `carrier_updates` and `approval_matrix` are missing.
5. User uploads or connects the carrier update extract and approval matrix
   file.
6. EKOS validates the package again.
7. EKOS generates the decision packet.
8. The report says:
   - allowed: Recommendation only;
   - blocked: Prepare approval-ready correction workflow;
   - reason: carrier confirmation is stale and required review inputs are not
     complete;
   - status: `review_required`.

The important UX point is that the user sees missing source requirements before
EKOS attempts a decision. The failure is not hidden. It is the workflow control.

## 7. Example User Flow: IDoc Material Lock

Workflow:

```text
IDoc Material Lock / Reprocessing Collision Readiness
```

Plain-language flow:

1. User selects IDoc Material Lock / Reprocessing.
2. User enters an IDoc number, or a material/plant object.
3. EKOS checks IDoc events, lock events, incoming queue, and policy inputs.
4. EKOS derives material lock and collision risk evidence.
5. EKOS allows recommendation only.
6. EKOS blocks preparation of an approval-ready IDoc reprocessing plan.
7. EKOS keeps governance status at `review_required` without an explicit human
   review artifact.

This shows that the product workflow is not limited to delivery delay. The
business object, evidence rules, and blocked action differ, but the product
shape remains the same.

## 8. What The User Should Not See

The normal operations user should not have to understand:

- YAML config;
- `decision_packet.json`;
- `evidence_objects.json`;
- Python commands;
- internal delegation numbers first;
- provenance JSON.

Those are implementation and audit artifacts.

The normal user should see business language first:

- current allowed action;
- blocked action;
- why blocked;
- missing data;
- required next human action;
- governance status.

## 9. What Advanced Users May See

Advanced users may inspect deeper artifacts.

SAP consultants, auditors, and platform teams may inspect:

- YAML workflow config;
- source package report;
- evidence lineage;
- decision packet JSON;
- provenance;
- validation failures;
- source freshness and source trust details.

The product should support drill-down without forcing every user to operate at
the artifact level.

## 10. Deployment Model

### Phase 0: Local Demo / CLI

Current state.

Capabilities:

- local commands;
- synthetic SAP-like sources;
- projection;
- minimum source package validation;
- supplemental source package build;
- configured onboarding;
- decision packet generation;
- human-readable governance report.

Boundary:

- no production deployment;
- no live SAP integration claim;
- no autonomous approval.

### Phase 1: Internal Analyst Tool

Capabilities:

- controlled source upload;
- workflow selection;
- case/object input;
- source package validation;
- report generation;
- no live writes.

This phase turns the CLI path into an internal user workflow for analysts and
operations users.

### Phase 2: Read-only SAP-connected Pilot

Capabilities:

- read-only SAP extracts;
- governed policy mapping;
- governed approval matrix mapping;
- source freshness checks;
- audit-friendly provenance;
- no SAP write-back.

This phase tests source reliability and governance fit without changing SAP
state.

### Phase 3: Workflow-integrated Review System

Capabilities:

- human review queue;
- reviewer notes;
- audit logs;
- role-based access;
- approval workflow integration;
- rerun and source update controls.

Boundary:

- still no autonomous execution unless separately governed and approved.

### Phase 4: Enterprise Decision Infrastructure

Capabilities:

- multi-workflow onboarding;
- source connectors;
- configuration UI;
- governance dashboard;
- optional RAG support for SOP, policy, and historical lookup.

This phase is the long-term product direction. It is not proven by the current
demo.

## 11. UX Screens

### Workflow Selection Screen

Purpose:

- choose the governed business workflow.

Contents:

- workflow name;
- description;
- required source package;
- last configuration version;
- owner or process area;
- supported action boundary.

### Case Search / Object Input Screen

Purpose:

- identify the business case.

Contents:

- delivery ID, IDoc number, material/plant, or other primary object input;
- source system selector if needed;
- recent case lookup;
- validation-ready indicator.

### Source Package Status Screen

Purpose:

- show whether EKOS has enough data to evaluate the case.

Contents:

- required aliases;
- current readiness;
- missing sources;
- stale sources;
- policy mapping status;
- approval mapping status;
- pass/fail result.

### Missing Data Resolution Screen

Purpose:

- help the user close source gaps.

Contents:

- missing alias;
- why it matters;
- accepted source types;
- upload or connect option;
- required fields;
- owner or steward guidance.

### Decision Report Screen

Purpose:

- show the business decision boundary.

Contents:

- primary object;
- current allowed action;
- blocked action;
- why blocked or allowed;
- evidence summary;
- policy boundary;
- required next human action;
- governance status.

### Evidence And Policy Trace Screen

Purpose:

- support audit and expert review.

Contents:

- evidence objects;
- source records;
- policy references;
- approval matrix entry;
- lineage;
- provenance;
- timestamps and freshness state.

### Review Queue Screen

Purpose:

- route packets to humans.

Contents:

- pending packets;
- assigned reviewer;
- status;
- review notes;
- requested missing data;
- final review outcome.

### Configuration Screen For Experts

Purpose:

- maintain workflow configuration.

Contents:

- source aliases;
- field mappings;
- evidence rules;
- blocking risk rules;
- authority labels;
- report language;
- validation requirements;
- configuration version.

## 12. Product Principle

EKOS should not answer when required inputs are missing.

EKOS should explain what is missing and what must be supplied before judgment.

This principle is more important than producing an impressive answer. In
enterprise operations, a missing approval matrix or missing external carrier
confirmation is not a prompt problem. It is a decision-control problem.

## 13. RAG Boundary In UX

If RAG is later added, it should appear as:

```text
Find related SOP/policy/history
```

It should not appear as the decision authority.

RAG may help find candidate documents, paragraphs, historical examples, or
comments. The decision result must still come from:

- evidence;
- policy;
- authority boundary;
- source package validation;
- governance state.

The user-facing distinction should remain clear:

```text
RAG helps find relevant text.
EKOS decides what the enterprise is currently allowed to do with governed
evidence, policy, and authority constraints.
```

## 14. Claim Boundary

This document describes intended product workflow.

It does not prove production readiness.

It does not claim the current UI exists.

It does not claim live SAP integration.

It does not claim autonomous approval.

It does not claim no-code onboarding is complete.

It does not describe EKOS as a chatbot.

It does not make RAG the core product.

## 15. Next Product Step

Create a simple clickable prototype or static mockup specification for the main
user journey:

```text
Workflow selection
-> source package validation
-> missing data resolution
-> decision report
```

The prototype should focus on comprehension:

- Can an SAP operations user see what is allowed?
- Can the same user see what is blocked?
- Can the user understand what source is missing?
- Can an auditor trace why EKOS stopped or proceeded?
- Can a reviewer see why the status remains `review_required`?

The next product risk is not whether EKOS can produce JSON. It is whether a
normal enterprise user can operate the governed workflow without understanding
the implementation pipeline.
