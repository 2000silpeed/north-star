# EKOS Design System User Journey Plan

Status: Product/design planning specification
Repository role: North Star product strategy and UX direction
Implementation repository: `2000silpeed/ekos-sap-knowledge-os`
Local design-system reference: `design-ontology-harness`
Reference issue: North Star Issue #4, "Product Gap: real enterprise data onboarding for EKOS decision packets"
Snapshot date: 2026-07-08

This document defines the first EKOS main user journey mockup specification and
design-system implementation plan. It uses the local `design-ontology-harness`
project as the design-system foundation.

It does not implement UI code, modify EKOS, create React components, create a
benchmark, claim production readiness, claim live SAP integration, claim
autonomous approval, or claim that the UI already exists.

## 1. design-ontology-harness Inspection Findings

The local `design-ontology-harness` project is not a finished EKOS UI. It is a
design-system synthesis and validation harness. It can generate and package
design-system artifacts such as:

- `system_spec.md`
- `token_schema.json`
- `component_inventory.json`
- `component_specs.md`
- `system_ontology.json`
- adapter outputs for Next.js, Tailwind, and shadcn-style projects

The relevant inspected files and directories were:

- `README.md`
- `TASK.md`
- `pyproject.toml`
- `adapters/nextjs-tailwind-shadcn/README.md`
- `adapters/nextjs-tailwind-shadcn/adapter.json`
- `design_ontology_harness/graph_schema.py`
- `design_ontology_harness/component_specs.py`
- `design_ontology_harness/advanced_components.py`
- `schemas/brand_profile.schema.json`
- `presets/dashboard--corporate-trust/`
- `presets/monitoring-ops--corporate-trust/`

The most relevant preset for EKOS is `dashboard--corporate-trust`, with
`monitoring-ops--corporate-trust` as a secondary reference. The dashboard
preset is especially close to EKOS because it is built around operational,
observable, governed, precise, structured, calm, and trustworthy product
surfaces.

Key findings:

- Design philosophy: token-led, component-contract-led, accessibility-first,
  provenance-aware design-system generation.
- Token structure: core, semantic, and component token layers.
- Visual language: dense operational data surfaces, compact navigation,
  restrained accents, status tags, review queues, source chips, audit tables,
  evidence graphs, and inspector drawers.
- Component patterns: data tables, source cards, citation drawers, status
  badges, exception queues, audit timelines, evidence graphs, decision record
  cards, approval rails, split panes, filter builders, and review controls.
- Layout conventions: app shell, sidebar/topbar navigation, data-review
  surfaces, split pane, drill-down inspector, table/list first surfaces, and
  secondary context rails.
- Naming conventions: semantic naming by category, intent, state, component,
  slot, and property. The harness warns against inventing arbitrary token
  names or hard-coding visual values.
- Interaction patterns: check, inspect, trace, review, approve/edit/reject,
  assign, filter, drill down, and reconstruct.
- Ontology concepts: typed graph nodes and edges for brand, principle, color
  token, component, component state, layout pattern, interaction pattern,
  accessibility rule, source reference, governance rule, and implementation
  failure pattern.
- UI primitive direction: buttons, inputs, navigation, feedback, overlays,
  data-display components, source cards, evidence graphs, audit timelines,
  decision record cards, and inspector drawers.
- Implementation direction: Next.js/Tailwind/shadcn is supported through an
  adapter contract, but EKOS should not implement UI code until a static
  mockup is approved.

What EKOS should reuse:

- the `dashboard--corporate-trust` design-system direction;
- the token layering model;
- the component contract approach;
- the audit/review/evidence component families;
- the operational data-surface density;
- accessibility and responsive resilience guardrails;
- the rule that visual references are advisory and cannot override product IA,
  tokens, copy, or governance.

What EKOS should avoid:

- marketing landing-page composition;
- chat-first layout;
- decorative AI glow or abstract graph visuals;
- generic card walls;
- exposing internal routing or internal delegation numbers as the first user
  concept;
- copying the harness example brand, palette, or product copy directly.

## 2. Executive Summary

EKOS UI should be a governed workflow interface, not a chat-first interface.

The interface should help a user understand:

- what workflow they are in;
- what business object is being evaluated;
- what source data is present, partial, missing, stale, or manually mapped;
- what EKOS allows;
- what EKOS blocks;
- why the action is allowed or blocked;
- what requires human review.

The most important product surface is not a prompt box. It is the combination
of source package status and decision boundary. A normal user should be able to
see, in business language, whether EKOS can safely evaluate the case before any
decision packet is generated.

The design-system direction should therefore be:

```text
operational app shell
-> workflow selection
-> source package status
-> missing data resolution
-> decision boundary
-> evidence and policy trace
-> human review
```

## 3. Product UX Principle

EKOS should not answer when required enterprise inputs are missing.

EKOS should explain what is missing before judgment.

This must be visible in the UI. The product should make the following state
obvious:

```text
Required source package incomplete
-> decision packet blocked
-> missing source explained
-> user supplies or connects source
-> EKOS rechecks
-> decision packet generated only after source package passes
```

The source package status and decision boundary are the primary UX. Chat, RAG,
or related SOP lookup can be supporting surfaces later, but they must not become
the decision authority.

## 4. Design System Foundation

The EKOS design-system foundation should come from the local
`design-ontology-harness`, especially `dashboard--corporate-trust`.

| Design-ontology-harness concept | Relevance to EKOS | Reuse / Extend / Avoid | Notes |
| --- | --- | --- | --- |
| `dashboard--corporate-trust` preset | Closest match for governed operational workflow surfaces | Reuse | Use as the primary design-system direction. Do not copy the example product brand. |
| `monitoring-ops--corporate-trust` preset | Useful secondary reference for operational monitoring and evidence review | Extend | Use for status panels, source cards, inspector drawers, and evidence review surfaces. |
| Core -> semantic -> component token layering | EKOS needs stable roles for source status, risk, evidence, governance, and review | Reuse | Preserve token layering rather than styling screens ad hoc. |
| Component inventory | Provides tables, badges, source cards, evidence graphs, audit timelines, review queues, and overlays | Reuse | Map EKOS primitives to existing component families before inventing new components. |
| Advanced components | Contains `source-card`, `evidence-graph`, `audit-timeline`, `decision-record-card`, `exception-queue`, and `approval-rail` | Reuse | These are directly relevant to source package validation and governance review. |
| App shell and data-review surface | EKOS needs workflow navigation plus dense operational status | Reuse | First viewport should show task state, not marketing copy. |
| Visual reference governance | Images and references are advisory only | Reuse | Product IA, copy, policy, and authority boundaries stay EKOS-led. |
| Next.js/Tailwind/shadcn adapter | Potential future implementation target | Extend later | Useful for a static prototype, but no UI code is implemented in this document. |
| Dense dark console default | May fit expert/audit users but not all SAP operations users | Extend | EKOS should support light-first manager surfaces and dark expert mode only if useful. |
| Chat and copilot artifact patterns | Useful for later support panels | Avoid as primary | EKOS should not open as a chat-only product. |
| Decorative graph or AI glow guardrails | Prevents EKOS from looking like a generic AI dashboard | Reuse | Evidence graphs must explain traceability, not decorate the screen. |

## 5. EKOS Design System Concepts

### A. Workflow Card

Represents a governed business workflow.

Content:

- workflow name;
- business description;
- required source package;
- process owner;
- config version;
- supported action boundary;
- configuration status.

Design-system basis:

- data-display card;
- status badge;
- source/package metadata rows;
- restrained action button.

### B. Case Object Header

Represents the primary business object under evaluation, such as Delivery
80001241 or IDoc 0000000000123456.

Content:

- object type;
- object ID;
- workflow name;
- case status;
- source package status;
- current governance status.

Design-system basis:

- app shell header;
- breadcrumb;
- status badge;
- compact metadata strip.

### C. Source Package Status

Shows whether required inputs are present, partial, missing, stale, or manually
mapped.

Content:

- required source alias;
- readiness state;
- policy for partial or manual mapping;
- source owner;
- last updated timestamp, when available;
- pass/fail result.

Design-system basis:

- data table;
- status badge;
- inline alert;
- evidence/source card;
- exception queue.

### D. Missing Source Card

Explains what source is missing and why it matters.

Content:

- source alias;
- why it is required;
- accepted source types;
- required fields;
- suggested owner or steward;
- trust level;
- upload/connect action.

Design-system basis:

- source-card;
- inline alert;
- action panel;
- form section.

### E. Decision Boundary Card

Shows allowed action and blocked action in business language.

Content:

- current allowed action;
- blocked action;
- reason;
- active blocking risks;
- required next human action;
- governance status.

Design-system basis:

- decision-record-card;
- risk-summary-card;
- status badge;
- action panel.

### F. Evidence Card

Shows an evidence object, its source, freshness, and decision impact.

Content:

- evidence ID;
- business meaning;
- source alias;
- source kind;
- freshness;
- lineage summary;
- how it changes the decision boundary.

Design-system basis:

- source-card;
- evidence-graph;
- citation drawer;
- inspector drawer.

### G. Policy / Authority Card

Shows policy mapping, approval matrix, required role, and authority boundary.

Content:

- policy ID;
- policy version;
- SOP reference;
- approval matrix version;
- required approver role;
- allowed action;
- blocked action;
- execution boundary.

Design-system basis:

- policy-matrix;
- approval-rail;
- decision-record-card.

### H. Governance Status Badge

Shows governance state in business language.

States:

- `needs_more_data`;
- `review_required`;
- `ready_for_review`;
- `approved_by_human`;
- `rejected`;
- `archived`.

Design-system basis:

- status-badge;
- non-color status encoding;
- accessible label and icon.

### I. Trace Drawer

Shows lineage and provenance for audit users.

Content:

- source records;
- source fields;
- source provider;
- config path;
- rule ID;
- evidence lineage;
- blocking risk provenance;
- packet provenance.

Design-system basis:

- inspector-drawer;
- citation-drawer;
- audit-timeline;
- evidence-graph.

### J. Review Action Panel

Shows actions available to a human reviewer.

Actions:

- request more data;
- accept recommendation-only boundary;
- approve preparation if evidence supports it;
- reject packet;
- add reviewer note.

Boundary:

- no autonomous execution;
- no approval without explicit compatible review artifact.

Design-system basis:

- approval-rail;
- action panel;
- form section;
- audit timeline.

## 6. Design Tokens

EKOS should not invent a detached palette or component style. It should derive
candidate token categories from `design-ontology-harness` and then bind EKOS
semantics to those categories.

### Color Roles

Use role-based color tokens:

- `canvas`;
- `surface`;
- `surface-muted`;
- `surface-elevated`;
- `border`;
- `border-strong`;
- `text`;
- `text-muted`;
- `text-subtle`;
- `focus`;
- `selection`;
- `source`;
- `policy`;
- `evidence`;
- `authority`;
- `risk`;
- `review`;
- `governance`.

Do not choose final hex values in this document. The harness already warns that
colors should come from color reference, extracted tokens, or brand guidance,
not arbitrary invention.

### Typography Roles

Use typography roles, not page-specific font choices:

- `heading`;
- `section-heading`;
- `body`;
- `label`;
- `metadata`;
- `tabular`;
- `code-or-provenance`;
- `status`;
- `button`;
- `helper-text`.

EKOS should prioritize utility, legibility, tabular figures for IDs/timestamps,
and compact metadata. Large hero-scale type should not be used inside workflow
screens.

### Spacing Roles

Use the harness spacing scale as the baseline:

- 0;
- 2;
- 4;
- 8;
- 12;
- 16;
- 24;
- 32;
- 48;
- 64.

EKOS should use compact spacing for dense evidence/status views and slightly
more breathing room for manager-facing decision reports.

### Border And Radius Roles

Use restrained radius:

- `none`;
- `sm`;
- `md`;
- `lg`;
- `pill`.

For EKOS workflow surfaces, `sm` or `md` should be the default. Repeated cards,
status badges, inputs, and panels should stay within an 8px default unless a
future installed preset explicitly defines otherwise.

### Status Colors

Define status as semantic roles:

- `available`;
- `partial`;
- `manual_mapping`;
- `missing`;
- `stale`;
- `blocked`;
- `ready`;
- `review_required`;
- `approved_by_human`;
- `rejected`.

Status must not rely on color alone. Each status needs text and icon or shape
support.

### Risk And Severity Colors

Define risk roles:

- `info`;
- `watch`;
- `warning`;
- `blocking`;
- `critical`.

For EKOS v1, risk color should support reviewability. It must not imply
approval or execution authority.

### Governance Status Colors

Governance roles:

- `needs_more_data`;
- `review_required`;
- `ready_for_review`;
- `approved_by_human`;
- `rejected`;
- `archived`.

`approved_by_human` must be visually distinct from `review_required`. A
synthetic onboarding demo must not render as approved without an explicit
compatible review artifact.

### Evidence Freshness Indicators

Freshness roles:

- `current`;
- `stale`;
- `missing`;
- `unknown`;
- `manual`;
- `projected`;
- `supplemental`.

Freshness indicators should appear in source package status, evidence cards,
and trace drawers.

## 7. Screen 1: Workflow Selection

Purpose:

Let the user choose the governed business workflow.

Cards:

- Delivery Delay / Carrier Confirmation
- IDoc Material Lock / Reprocessing
- Future: FO Planning Block
- Future: Invoice Block

Each Workflow Card should show:

- workflow name;
- short business description;
- required source package;
- process owner;
- config version;
- supported action boundary;
- status badge: configured, draft, or needs validation.

Primary CTA:

```text
Start check
```

Design-system notes:

- Use the `dashboard--corporate-trust` app shell.
- Use a data-review layout, not a marketing grid.
- Workflow cards may be repeated objects, but the page should not become a
  homogeneous card wall.
- Include a compact filter or saved view if workflow count grows.
- Show status badges next to workflow names for scanability.

## 8. Screen 2: Case Input

Purpose:

Identify the business case.

Delivery Delay fields:

- Delivery ID;
- optional plant;
- optional carrier;
- optional date range.

IDoc Lock fields:

- IDoc number;
- material;
- plant;
- optional message type.

Visible copy:

```text
EKOS will first check whether the required enterprise inputs are available. It
will not produce a decision if required sources are missing.
```

Primary CTA:

```text
Check required data
```

Design-system notes:

- Use form-section, text-field, search-field, select, and helper text.
- Keep the warning about required inputs near the CTA.
- Do not show a chat composer as the main input.
- Show the selected workflow and config version in the Case Object Header.

## 9. Screen 3: Source Package Status

Purpose:

Show whether EKOS has enough data to evaluate the case.

For the delivery-delay example:

- `deliveries`: available / partial accepted;
- `tracking_events`: available / partial accepted;
- `policies`: curated mapping;
- `carrier_updates`: missing or ready;
- `approval_matrix`: missing or ready.

### A. Failed State

State:

- `carrier_updates` missing;
- `approval_matrix` missing;
- decision packet will not be generated.

Primary CTAs:

- Add missing source;
- View required fields;
- Export validation report.

Disabled CTA:

```text
Generate decision packet
```

Explanation:

```text
EKOS cannot safely evaluate this case yet. Required source data is missing.
```

### B. Passed State

State:

- all required aliases present;
- partial/manual mappings accepted where policy allows;
- decision packet can be generated.

Primary CTA:

```text
Generate decision packet
```

Explanation:

```text
All required sources are available. EKOS can generate a decision packet.
```

Design requirement:

This screen should be visually central. It is the main difference between EKOS
and a chatbot. EKOS first checks whether the enterprise input package is
sufficient before it produces a governed decision artifact.

## 10. Screen 4: Missing Data Resolution

Purpose:

Help the user close source gaps.

Each Missing Source Card should show:

- missing source name;
- why it matters;
- accepted source types;
- required fields;
- suggested owner or steward;
- upload/connect option;
- trust level.

Example: `carrier_updates`

Why needed:

```text
EKOS cannot determine whether external carrier confirmation is current.
```

Accepted sources:

- LE tracking event table;
- 3PL or carrier extract;
- curated carrier update CSV.

Required fields:

- `delivery_id`;
- `carrier_id`;
- `updated_at`;
- `confirmation_status`;
- `update_source`.

Example: `approval_matrix`

Why needed:

```text
EKOS cannot determine who may prepare, review, or approve the requested action.
```

Accepted sources:

- governed approval matrix CSV;
- workflow customizing export;
- policy or governance maintained table.

Design-system notes:

- Use source-card plus inline alert.
- Let the user inspect required fields before upload.
- Show owner/steward fields so missing data becomes an accountable action.
- Do not let upload imply approval.

## 11. Screen 5: Decision Report

Purpose:

Show the business decision boundary.

Delivery 80001241 example:

| Field | Value |
| --- | --- |
| Primary object | Delivery 80001241 |
| Current allowed action | Recommendation only |
| Blocked action | Prepare approval-ready correction workflow |
| Why blocked | Carrier confirmation is stale and governance review is required. |
| Governance status | `review_required` |
| Required next action | Request current carrier confirmation and submit for human review. |

Required sections:

- What EKOS allows now;
- What EKOS blocks now;
- Why;
- Required next human action;
- Review status.

Design-system notes:

- Do not lead with numeric delegation levels.
- Use business labels before internal metadata.
- Render internal delegation levels only in an advanced details section.
- Keep `review_required` visually different from approval.
- Use a Decision Boundary Card as the manager-facing anchor.

## 12. Screen 6: Evidence And Policy Trace

Purpose:

Allow auditors and advanced users to inspect why the result happened.

Show:

- evidence objects;
- source records;
- source freshness;
- policy mapping;
- approval matrix entry;
- source-to-evidence lineage;
- packet provenance;
- blocking risk provenance.

Design-system notes:

- Use a drill-down Evidence and Policy Trace screen, not the default manager
  view.
- Use evidence cards for scanability.
- Use Trace Drawer for source fields, row keys, config paths, and rule IDs.
- Use Evidence Graph only when relationships add clarity. A simple ledger/table
  is preferable when the trace is linear.

## 13. Screen 7: Review Request

Purpose:

Route the decision packet to a human.

Show:

- assigned reviewer;
- required role;
- packet status;
- reviewer note;
- review due date, if configured;
- prior packet ID, if a rerun occurred.

Action options:

- request more data;
- accept recommendation-only boundary;
- approve preparation, if evidence supports it;
- reject packet.

Boundary:

- no autonomous execution;
- no external customer commitment;
- no SAP-changing action without separately governed execution approval.

Design-system notes:

- Use approval-rail and audit-timeline.
- Require reviewer note for approval or rejection.
- Keep reviewer action separate from EKOS-generated recommendation.

## 14. Key UX Copy

Missing source state:

```text
EKOS cannot safely evaluate this case yet. Required source data is missing.
```

Ready state:

```text
All required sources are available. EKOS can generate a decision packet.
```

Blocked action:

```text
EKOS blocks approval-ready preparation because required evidence or authority
is incomplete.
```

Review required:

```text
This result is not an approval. A human review is required.
```

RAG boundary:

```text
Related SOPs or past cases may help with context, but they do not replace the
governed decision packet.
```

No autonomous execution:

```text
EKOS may prepare a reviewable decision packet, but it does not execute or
externally commit this action.
```

## 15. User Role Variants

| Role | Default view | Secondary view | What should be hidden by default |
| --- | --- | --- | --- |
| SAP operations user | Source package status, decision report, missing data actions | Evidence summary | YAML config, raw JSON, full provenance |
| Team lead / approval owner | Decision report, review request, policy/authority summary | Evidence trace | Connector logs and internal config mechanics |
| Audit/control reviewer | Evidence trace, policy mapping, provenance, review log | Decision report | Marketing copy or simplified-only explanations |
| SAP consultant/process expert | Configuration status, source alias mapping, validation failures | Source package reports | Manager-only simplifications |
| IT/security/platform team | Source access, connector status, audit logs, no-write boundary | Deployment controls | Business-only report copy without lineage |

The normal user should see business language first. Advanced users can inspect
YAML workflow config, source package report, evidence lineage, decision packet
JSON, provenance, and validation failures.

## 16. Implementation Plan Based On Design System

### Phase 0: Design-system Inventory

Use `design-ontology-harness` as the design-system foundation.

Outputs:

- selected preset direction;
- mapped EKOS primitives;
- token category mapping;
- component reuse list;
- gaps requiring new EKOS-specific primitives.

### Phase 1: Static Mockup Specification

This document is the first static specification.

No UI code is implemented in this phase.

### Phase 2: Static HTML Or Local Prototype

Build a static HTML or local prototype using harness primitives.

It should cover:

- workflow selection;
- source package failed state;
- missing data resolution;
- source package passed state;
- decision report.

The prototype should reuse the harness structure, component contracts, and token
categories. It should not copy the Agent Org Network example brand, product
copy, or palette as EKOS identity.

### Phase 3: EKOS Component Library

Candidate components:

- `WorkflowCard`;
- `CaseObjectHeader`;
- `SourcePackageStatusPanel`;
- `MissingSourceCard`;
- `DecisionBoundaryCard`;
- `EvidenceCard`;
- `PolicyAuthorityCard`;
- `GovernanceStatusBadge`;
- `TraceDrawer`;
- `ReviewActionPanel`.

Components should map back to the harness component families rather than
inventing a disconnected visual language.

### Phase 4: Connect Prototype To Generated EKOS Artifacts

Connect the prototype to existing EKOS artifacts:

- `source_package_validation_report.md/json`;
- `source_package.json`;
- `decision_packet.json`;
- `evidence_objects.json`;
- `human_readable_report.md`;
- `governance_decision_log.json`.

The prototype should render generated state. It should not fabricate human
approval or hidden source availability.

### Phase 5: Interactive Analyst Tool

Later, package the flow into an analyst tool:

- workflow selection;
- case search;
- source package validation;
- missing source upload;
- packet generation;
- review queue;
- audit trace.

This phase requires implementation planning, security review, and explicit
repository work in the EKOS implementation repository.

## 17. Success Criteria For Clickable Prototype

A reviewer should understand:

- EKOS starts with workflow selection, not free chat.
- EKOS checks required inputs before judgment.
- EKOS refuses to decide when the source package is incomplete.
- EKOS explains missing data.
- EKOS shows allowed and blocked actions in business language.
- EKOS keeps `review_required` separate from approval.
- EKOS supports evidence and policy trace for auditors.
- EKOS is governed decision infrastructure, not Enterprise RAG.

Minimum prototype flow:

```text
Workflow selection
-> source package failed state
-> missing data resolution
-> source package passed state
-> decision report
```

## 18. Claim Boundary

This is a mockup and design-system planning specification.

It does not implement UI.

It does not claim production readiness.

It does not claim live SAP integration.

It does not claim autonomous approval.

It does not claim the current UI exists.

It does not claim no-code onboarding is complete.

It does not make RAG the core product.

`design-ontology-harness` is used as a local design-system reference, not as
proof that EKOS UI exists.

## 19. Recommended Next Step

Create a static HTML or Figma-style prototype based on
`design-ontology-harness`, with `dashboard--corporate-trust` as the primary
foundation.

The first prototype should cover:

```text
Workflow selection
-> source package failed state
-> missing data resolution
-> source package passed state
-> decision report
```

The prototype should stay artifact-backed. It should use current EKOS generated
outputs where possible and represent missing or unavailable states honestly.
