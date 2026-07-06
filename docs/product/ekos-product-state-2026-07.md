# EKOS Product State - July 2026

Status: Product-state snapshot
Repository role: North Star product strategy and cross-repository synthesis
Implementation repository: `2000silpeed/ekos-sap-knowledge-os`
Reference issue: North Star Issue #4, "Product Gap: real enterprise data onboarding for EKOS decision packets"
Snapshot date: 2026-07-06

This document updates North Star's product narrative after the latest EKOS
implementation milestones. It is not an implementation plan, benchmark, demo,
production-readiness claim, live SAP validation, or human correctness
validation.

## 1. Executive Summary

EKOS has moved from a research artifact and hand-authored decision packet demo
toward a configurable enterprise decision-packet platform.

The current proof chain is:

```text
SAP-like CSV or EKOS fragment fixtures
-> configurable mapping
-> evidence objects
-> blocking risks
-> authority boundary
-> decision packet
-> human-readable governance report
```

This is a meaningful product-state change.

Earlier North Star documents correctly identified that a polished decision
packet demo was not enough. The core product risk was upstream onboarding:

```text
Can EKOS produce decision-grade packets from enterprise-shaped source data,
rather than only consume hand-authored packets?
```

The EKOS implementation repository now contains a narrow answer for the current
delivery-delay / carrier-confirmation workflow:

- table-shaped synthetic SAP-like CSV extracts can become evidence objects and
  a decision packet;
- the packet can produce a manager-readable governance report;
- workflow-specific mapping can live in YAML configuration rather than only
  bespoke Python logic;
- source-to-evidence lineage and decision packet provenance exist for
  configured onboarding;
- local EKOS-like fragment fixtures can feed the same configured mapping path
  through `FragmentSourceProvider`.

This does not prove production readiness, live SAP integration, broad no-code
onboarding, or human correctness validation. It does mean North Star should now
describe EKOS as a configurable governed decision-packet platform in progress,
not only as a research concept or hand-authored demo.

## 2. Current Product Definition

EKOS is governed decision infrastructure that turns enterprise objects,
evidence, policies, and authority boundaries into reviewable decision packets
for AI-assisted enterprise operations.

Clarifications:

- EKOS is not a chatbot.
- EKOS is not Enterprise RAG.
- EKOS is not an autonomous execution system.
- EKOS is not production approval infrastructure yet.
- EKOS is not a claim that AI can safely run enterprise operations without
  human governance.

The product output is not merely an answer. The output is a governed enterprise
decision packet that preserves:

- primary business object;
- evidence;
- policy constraint;
- blocking risk;
- authority boundary;
- allowed action;
- blocked action;
- governance state;
- reviewable explanation.

## 3. What Has Been Proven So Far

The statements below are scoped to the implemented EKOS demos, fixtures, and
tests. They should not be generalized to production SAP landscapes.

### Hand-authored Packets Can Move Through The Workflow Demo

The original enterprise workflow demo showed that a prebuilt decision packet
can move through:

```text
Scenario
-> Decision
-> Enterprise Explanation
-> Explanation Quality Check
-> Enterprise Answer
-> Human Review
-> Final Governance State
```

This proved the downstream value chain once structured decision artifacts
already exist.

### SAP-like CSV Extracts Can Generate Decision Packets

The EKOS implementation now includes a synthetic SAP-like onboarding path for
the delivery-delay / carrier-confirmation workflow. It uses table-shaped inputs
such as delivery, tracking event, carrier update, policy, and approval matrix
extracts to produce:

- `decision_packet.json`
- `evidence_objects.json`
- `onboarding_report.md`

This proves the current workflow can start from structured SAP-like extracts
instead of only hand-authored packet artifacts.

### Manager-readable Reports Improve Comprehension

The SAP-like workflow demo now produces a human-readable manager report that
does not lead with internal delegation numbers. It translates the internal
boundary into business language:

- current allowed action: Recommendation only;
- blocked action: Prepare approval-ready correction workflow;
- governance result: `review_required`.

This is a product communication improvement, not a correctness proof.

### Basic Comprehension Has Been Observed

The first manager-report comprehension check indicates that a viewer can
understand:

- what EKOS allows;
- what EKOS blocks;
- why preparation is blocked;
- why the result remains `review_required`.

This should be treated as an early product-understanding signal only. It is not
human validation of correctness, production readiness, or operational ROI.

### RAG Is Not Required For The Structured SAP-like Demo

The current SAP-like demo uses structured facts:

- delivery ID;
- delivery status;
- carrier update timestamp;
- confirmation window;
- policy requirement;
- approval boundary;
- blocking risks.

The stale confirmation decision is derived deterministically from structured
data and policy fields. RAG is not needed to explain why stale carrier
confirmation blocks approval-preparation in this slice.

### YAML-configured Onboarding Can Reproduce The Current Workflow

The implementation now includes a configured onboarding command:

```bash
python -m ekos data-onboard configured \
  --config configs/onboarding/delivery_delay_confirmation.yaml
```

The current CSV configuration re-expresses the existing delivery-delay workflow
without changing the decision logic or adding new SAP logistics fields.

This proves that at least one workflow can move some workflow-specific mapping
from hard-coded Python into YAML configuration.

It does not prove full no-code onboarding.

### Config Validation Exists

The configured onboarding path validates the workflow configuration before
packet construction. The current contract validates required sections, source
aliases, primary object mappings, evidence rules, blocking risk rules,
authority boundary, decision packet fields, report labels, supported
operators, duplicate IDs, unknown source aliases, and detectable missing field
references.

This makes the configuration more auditable than an unchecked YAML file.

### Source-to-evidence Lineage And Packet Provenance Exist

Configured onboarding now emits evidence lineage and decision packet
provenance. Evidence objects can show:

- source alias;
- source file;
- source kind;
- source fields;
- physical fields;
- source row key.

Decision packets include provenance that records workflow ID, config path,
source provider, source files, evidence lineage, and blocking risk provenance.

This is important because reviewability requires traceability from packet
claims back to source records and rules.

### Fragment Fixtures Can Feed The Same Configured Mapping Path

The implementation now includes `FragmentSourceProvider` and a fragment config:

```bash
python -m ekos data-onboard configured \
  --config configs/onboarding/delivery_delay_confirmation_fragments.yaml
```

The fragment path uses local synthetic EKOS-like fragment fixtures and produces
the same key decision result as the CSV configured path:

- primary object: `Delivery 80001241`;
- evidence includes `delivery_status_event`, `carrier_update_stale`, and
  `policy_confirmation_required`;
- maximum safe delegation level: `1`;
- action category: `recommend`;
- business allowed action: Recommendation only;
- business blocked action: Prepare approval-ready correction workflow.

This proves that configured onboarding is not limited to CSV files. It does not
prove live FragmentStore integration or live SAP ingestion.

## 4. What Has Not Been Proven

The following remain unproven:

- live SAP integration into decision packets;
- production readiness;
- human validation of correctness;
- scalable onboarding across many workflows;
- real approval workflow integration;
- security and IAM integration;
- UI-assisted no-code configuration;
- automatic discovery of source mappings;
- real policy/SOP governance;
- operational ROI;
- full FragmentStore-driven onboarding;
- RFC-row provider integration into configured onboarding;
- second workflow onboarding without engine changes.

The strongest honest claim is:

```text
EKOS now has a narrow, tested path from synthetic CSV or fragment-shaped inputs
to governed decision packets and reports for one workflow.
```

Nothing stronger is currently supported.

## 5. Product Architecture State

The current architecture is best understood as four layers.

### Layer 1: SAP Ingestion Foundation

Existing implementation surface in the EKOS repository includes foundations
for:

- SAP read/source modules;
- DDIC and table rows;
- ADT/RFC/OData paths;
- EKOS fragments;
- FragmentStore and lineage.

Current state:

- Implemented or scaffolded ingestion foundations exist.
- The current product demo does not yet prove live SAP-to-decision-packet
  ingestion.
- The fragment fixture path is a bridge toward using those ingestion outputs,
  not a replacement for them.

### Layer 2: Configurable Onboarding

Current implementation includes:

- YAML workflow configuration;
- `SourceProvider` boundary;
- `CsvSourceProvider`;
- `FragmentSourceProvider`;
- config validation;
- source-to-evidence lineage;
- decision packet provenance.

This layer is the most important recent product change. It moves EKOS away
from a purely bespoke SAP logistics rule demo and toward a shared configurable
decision-packet builder.

### Layer 3: Decision Packet

The current decision packet represents:

- evidence;
- policy;
- blocking risks;
- execution preconditions;
- authority boundary;
- allowed action;
- blocked action;
- maximum safe delegation level;
- review/governance state.

For the current workflow, the packet explains why the requested
approval-preparation action is blocked and why the safe action remains
recommendation only.

### Layer 4: Human-readable Governance Report

The current report layer translates internal packet structure into
manager-facing language:

- manager view;
- allowed action;
- blocked action;
- why blocked;
- evidence used;
- policy and authority boundary;
- required next human actions;
- governance result: `review_required`.

This report is not production approval. It is a reviewable communication layer
over the decision packet.

## 6. Relationship Between North Star And EKOS Repo

North Star owns:

- product strategy;
- architecture direction;
- research synthesis;
- claim boundaries;
- validation planning;
- roadmap;
- public narrative.

The EKOS implementation repository owns:

- implementation;
- demos;
- commands;
- tests;
- generated artifacts;
- runbooks;
- runnable source providers;
- configured onboarding engine.

North Star should summarize product state and preserve reasoning. It should
not recreate EKOS commands, fixtures, source providers, or demos inside this
repository.

## 7. RAG Boundary

Current product line:

```text
RAG retrieves relevant text.
EKOS decides what the enterprise is allowed to do with it.
```

The word "decides" means governed decision-packet construction under explicit
evidence, policy, authority, review, and governance constraints. It does not
mean autonomous production approval.

RAG is optional support for:

- SOP lookup;
- policy paragraph retrieval;
- historical case retrieval;
- audit comment retrieval;
- exception playbook lookup.

RAG is not the core decision authority. Retrieval may suggest relevant text or
candidate references, but those references must still be grounded into EKOS
objects, evidence, policy constraints, and authority boundaries before they can
affect a decision packet.

For the current structured SAP-like demo, RAG is not required.

## 8. Why Configurable Onboarding Matters

The key product risk was that EKOS might become a custom-coded SAP logistics
rule engine.

That would be the wrong product direction because each new workflow would need
bespoke code for:

- source fields;
- evidence rules;
- policy mappings;
- blocking risks;
- authority labels;
- report wording.

Configurable onboarding changes the direction:

```text
from bespoke Python logic per workflow
to workflow-specific configuration interpreted by a shared engine
```

This matters because evidence rules, policy constraints, blocking risks, and
authority boundaries become reviewable product artifacts instead of hidden
implementation details.

Claim boundary:

- This is an early prototype.
- It supports one current workflow.
- It is not a full no-code platform.
- It does not prove scalable onboarding across many enterprise processes.

## 9. Why FragmentSourceProvider Matters

CSV proves table-shaped demo input.

Fragments prove EKOS ingestion-style assets can feed the same configured
decision-packet path.

This bridges:

```text
existing SAP ingestion foundation
-> EKOS fragments or fragment-shaped assets
-> configurable mapping
-> decision packet
-> governed report
```

That bridge matters because EKOS already has fragment and FragmentStore
foundations. The product should not depend permanently on CSV demo extracts if
the long-term architecture is enterprise ingestion into EKOS fragments.

Claim boundary:

- The implemented provider reads local synthetic fragment fixtures.
- It does not query a production FragmentStore.
- It does not connect to live SAP.
- It does not prove production ingestion.

## 10. Current Best Demo Narrative

The current 5-minute product story should be:

1. Show SAP-like CSV source extracts or EKOS-like fragment fixtures.
2. Show the YAML configuration that maps source aliases, evidence rules,
   policy constraints, blocking risks, and authority boundary.
3. Show evidence objects produced from the configured mapping.
4. Show the decision packet with evidence lineage and provenance.
5. Show the human-readable manager report.
6. Explain that the safe current action is Recommendation only.
7. Explain that preparing an approval-ready correction workflow is blocked
   because carrier confirmation is stale, policy requires current external
   confirmation, and delay owner verification is not recorded.
8. Explain that governance remains `review_required` because no explicit human
   review artifact is available.
9. Explain why this is not RAG: retrieval may help later, but this slice is
   governed decision-packet construction from structured evidence, policy, and
   authority.

The concise spoken version:

```text
EKOS is not showing another chatbot answer. It starts from enterprise-shaped
source artifacts, maps them through explicit configuration into evidence,
policy, risk, and authority, then produces a decision packet and manager report.
The result is not "AI says approve." The result is "recommendation only is
allowed; approval-preparation is blocked; review is required; here is why."
```

## 11. Next Product Risks

Priority order:

### P1: Can existing SAP ingestion outputs feed configured onboarding from real or realistic assets?

The fragment fixture path is a bridge. The next risk is whether actual EKOS
ingestion outputs, such as materialized FragmentStore records or RFC-row
payloads, can feed the same configured layer without workflow-specific engine
changes.

### P2: Can a second workflow be configured without new engine logic?

One configured workflow is not enough to prove repeatability. A second workflow
should test whether the current configuration contract is general enough or
still overfit to delivery-delay confirmation.

### P3: Can real SAP operations users validate the required fields?

The v2 data model is still a hypothesis. Real SAP logistics users need to
review which fields are must-have blockers, which fields affect reviewability,
which fields are context only, and which fields are hard to extract.

### P4: Can policy/SOP/versioning be governed?

Normalized policy constraints are useful only if their source documents,
versions, owners, and effective dates are governed.

### P5: Can review workflow and audit log be integrated?

The demo correctly keeps governance at `review_required` when no compatible
review artifact exists. Productization requires real review workflow
integration, reviewer identity, timestamps, and durable audit logs.

### P6: Can this be packaged for non-developer users?

YAML configuration is an engineering interface. Enterprise adoption will
eventually require safer tooling for process owners, SAP consultants, audit
reviewers, and platform administrators.

## 12. Recommended Next Steps

### Step 1: Keep North Star updated with product-state snapshots

After major EKOS implementation milestones, North Star should record what
changed, what is now supported, and what remains unproven.

### Step 2: In EKOS, test FragmentStore or RFC-row provider as the next source provider

The next implementation question should be whether the provider boundary can
consume materialized FragmentStore outputs or RFC-row payloads without changing
workflow-specific YAML logic.

### Step 3: Configure a second workflow without changing engine logic

A second workflow is the strongest near-term test of whether configurable
onboarding is repeatable or still delivery-delay-specific.

### Step 4: Validate with a real SAP operations user

Use the existing v2 data model worksheet to ask whether the current packet and
report contain the fields needed for real review.

## 13. Claim Boundary

This document is a product-state snapshot.

It is not a production readiness claim.

It is not live SAP validation.

It is not human correctness validation.

It is not a benchmark.

It does not claim EKOS is no-code today.

It does not claim RAG is unnecessary for all enterprise use cases.

It does not claim `FragmentSourceProvider` proves production ingestion.

It only records the current strongest supported interpretation:

```text
EKOS has advanced from hand-authored decision packet demos toward a narrow,
configurable, provenance-aware decision-packet onboarding platform. The next
frontier is proving that real or realistic SAP ingestion outputs and a second
workflow can use the same configured layer without bespoke engine logic.
```
