# EKOS RAG Boundary And Support Layer

Status: Product architecture boundary
Repository role: North Star product strategy and architecture
Implementation repository: `2000silpeed/ekos-sap-knowledge-os`
Reference issue: North Star Issue #4, "Product Gap: real enterprise data onboarding for EKOS decision packets"

This document defines where retrieval-augmented generation (RAG) belongs in
EKOS product architecture. It does not implement RAG, create a benchmark, create
a demo, or claim production readiness.

## 1. Executive Summary

EKOS does not require RAG for the current SAP-like structured-data demo.

The current demo works through deterministic transformation:

```text
SAP-like structured extracts
-> evidence objects
-> decision packet
-> human-readable report
-> governance status
```

This is intentionally not a RAG flow.

That boundary matters. The current EKOS product value is not that it retrieves
text well. The value is that it turns enterprise facts, policies, evidence, and
authority boundaries into a governed decision packet that humans can review.

RAG may become useful later as an auxiliary support layer for unstructured or
semi-structured enterprise knowledge. But RAG must not become the core decision
authority. Retrieval may help find candidate references. EKOS must still ground,
normalize, constrain, explain, and govern the decision.

## 2. Why The Current Demo Works Without RAG

The current SAP-like logistics slice uses structured facts:

- `delivery_id`
- delivery status
- carrier update timestamp
- confirmation window
- policy requirement flag
- approval boundary
- blocking risks

These facts are better handled through deterministic evidence and policy
transformation than through text retrieval.

For the current demo, EKOS does not need to ask a model to infer whether carrier
confirmation is stale. It can compute freshness from:

```text
decision_time - carrier_eta_update_time > confirmation_window_hours
```

Likewise, EKOS does not need a text answer to decide whether preparation should
be blocked. It can derive a governed packet from explicit source rows:

```text
stale carrier update
+ policy requires current external confirmation
+ delay owner verification not recorded
-> approval-preparation blocked
-> recommendation only
-> review_required
```

Using RAG for this slice would add uncertainty where the product needs
determinism, provenance, and auditability.

## 3. EKOS Core

EKOS core is:

- object grounding
- evidence normalization
- policy grounding
- authority/delegation boundary
- decision packet construction
- explanation quality check
- human review/governance state

EKOS core is not retrieval.

EKOS core is governed decision packet construction.

The product question EKOS answers is not:

```text
What text should the model retrieve?
```

The product question is:

```text
Given the enterprise object, evidence, policy, and authority boundary, what is
the enterprise currently allowed to do?
```

## 4. Where RAG Becomes Useful

RAG becomes useful when relevant enterprise knowledge is unstructured or
semi-structured, for example:

- SOP PDFs
- policy manuals
- exception handling guides
- email threads
- incident notes
- approval history narratives
- audit comments
- historical similar cases
- user-written reason codes

Those sources often contain useful language, historical examples, or policy
context that is not already normalized into structured fields. RAG can help find
candidate passages, references, and similar cases. But the retrieved text is not
itself an authority boundary.

## 5. RAG As Auxiliary Support Layer

The correct architecture separates structured data transformation from
unstructured knowledge discovery.

Structured enterprise data path:

```text
Structured enterprise data
-> deterministic EKOS evidence objects
-> decision packet
```

Unstructured knowledge support path:

```text
Unstructured documents
-> RAG retrieval
-> candidate policy/evidence references
-> EKOS grounding
-> decision packet support
```

RAG may retrieve or suggest relevant policy and evidence candidates.

RAG must not directly grant authority or determine final delegation level.

Before a retrieved passage can affect an EKOS decision packet, it should pass
through EKOS grounding:

- Which policy or evidence object does it support?
- What source document and version does it come from?
- Is it current?
- Does it apply to this object, process, and jurisdiction?
- Does it permit recommendation, preparation, approval, confirmation, or
  execution?
- Who has authority to rely on it?
- Can a reviewer reconstruct the path later?

## 6. Bad Architecture To Avoid

Avoid this architecture:

```text
RAG answer
-> enterprise decision
```

This is risky because it can create:

- hallucinated authority
- weak auditability
- unstable policy interpretation
- poor provenance
- unclear responsibility
- difficult governance review
- another enterprise chatbot that sounds confident but cannot be governed

The failure is not that retrieval is useless. The failure is treating retrieval
as if it were the decision system.

In enterprise operations, finding a relevant paragraph is not the same as being
allowed to prepare a correction workflow, contact a customer, change SAP state,
or approve an exception.

## 7. Good Architecture

Prefer this architecture:

```text
Evidence + policy + authority boundary
-> decision packet
-> governed workflow
```

RAG may support evidence discovery, but the decision must pass through EKOS
objects and governance checks.

The good architecture preserves the product distinction:

- RAG helps discover relevant text.
- EKOS grounds that text into enterprise objects, policy constraints, evidence
  records, and authority boundaries.
- EKOS produces a decision packet.
- Human reviewers and governance state remain explicit.

This makes the answer reviewable. It also prevents the RAG layer from silently
becoming a policy interpreter, approver, or workflow actor.

## 8. v1 Product Boundary

For EKOS v1, do not add RAG to the current SAP-like onboarding demo.

EKOS v1 should remain:

```text
SAP-like structured CSV
-> deterministic onboarding
-> evidence objects
-> decision packet
-> manager report
-> review_required governance state
```

This boundary keeps the demo honest. It shows that structured enterprise data
can become governed decision packets without relying on text retrieval.

It also keeps the product message clear:

```text
EKOS is not Enterprise RAG. EKOS is governed decision infrastructure.
```

## 9. v1.5 / v2 RAG Roadmap

RAG can be added later as an optional support layer.

### v1.5: Policy And SOP Lookup Support

Candidate v1.5 capabilities:

- policy/SOP lookup support
- retrieve relevant policy paragraphs
- attach candidate policy references to decision packets
- require human or deterministic grounding before retrieved text affects
  authority

v1.5 should not allow retrieved text to directly change the maximum safe
delegation level. A passage may become a candidate reference. It must still be
grounded into an EKOS policy or evidence object before it affects the decision.

### v2: Historical And Governance Context Support

Candidate v2 capabilities:

- historical case retrieval
- audit comment retrieval
- exception playbook retrieval
- similarity search over past decision packets
- RAG-assisted policy authoring workflow

These capabilities may improve reviewer context and policy maintenance. They
should still preserve EKOS authority boundaries. Similarity to a past case is
useful evidence for review, not automatic permission to act.

## 10. Product Messaging

Bad positioning:

```text
EKOS is Enterprise RAG.
```

Better positioning:

```text
EKOS is governed decision infrastructure for enterprise AI.
```

Best concise line:

```text
RAG retrieves relevant text.
EKOS decides what the enterprise is allowed to do with it.
```

The word "decides" here means decision-packet construction under explicit
evidence, policy, authority, review, and governance constraints. It does not
mean autonomous production approval.

## 11. Relationship To Current Demo

The current SAP-like demo proves:

```text
structured data can become evidence objects and decision packets without RAG.
```

Future RAG support would enrich:

- policy references
- SOP grounding
- historical examples
- audit context

But RAG is not needed to explain why stale carrier confirmation blocks
approval-preparation.

The current demo already has the necessary structured elements:

- delivery identity: `Delivery 80001241`
- evidence: delayed delivery, stale carrier update, policy confirmation
  requirement
- policy: `delay_confirmation_policy`
- authority boundary: recommendation only
- blocked action: prepare approval-ready correction workflow
- governance status: `review_required`

Those elements are enough to explain the result without retrieving text from a
document corpus.

## 12. Claim Boundary

- This is a product architecture boundary document.
- It is not a RAG implementation.
- It is not a benchmark.
- It does not prove production readiness.
- It does not claim RAG is unnecessary for all enterprise use cases.
- It only states that RAG is not the core of EKOS.
- It does not claim human validation.
- It does not claim the current SAP-like demo is live SAP integration.
