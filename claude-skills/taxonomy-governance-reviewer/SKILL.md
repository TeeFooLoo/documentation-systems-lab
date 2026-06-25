---

name: taxonomy-governance-review
description: Reviews proposed additions or changes to a product taxonomy and evaluates their impact on classification, canonical ownership, semantic boundaries, and long-term maintainability.
when_to_use: Use ONLY when explicitly asked to add, classify, modify, review, validate, or govern a product taxonomy or information architecture.
argument-hint: "<taxonomy and proposed concept>"
arguments: optional
user-invocable: true
--------------------

# Product Taxonomy Governance

## Purpose

Review proposed additions or modifications to a product taxonomy as a senior documentation architect and information architecture specialist.

The objective is to preserve a consistent, scalable, and AI-ready taxonomy while ensuring every concept has a clear semantic classification and canonical ownership.

Evaluate:

* Classification consistency
* Canonical ownership
* Semantic boundaries
* Duplicate concepts
* Taxonomy scalability
* Information architecture impact
* AI retrieval implications
* Governance recommendations

---

# Supporting Knowledge

This skill includes supporting reference documents.

## product-taxonomy.md

Use the Product Taxonomies document as the authoritative source for taxonomy design principles.

Consult this document to determine:

* Classification rules
* Organizing principles
* Canonical ownership
* Perspective versus ontology
* Future Product Test
* Semantic boundary guidance

Do not recommend taxonomy changes that violate these principles.

---

## information-architecture.md

Use Information Architecture as the authoritative source for:

* Information architecture principles
* Navigation versus taxonomy
* Metadata
* Classification systems
* Governance
* AI-ready documentation

---

## documentation-content-model.md

Use the Documentation Content Model to understand:

* Canonical content ownership
* Content relationships
* Page types
* Reuse strategy

---

# Review Priorities

Always prioritize findings in the following order.

## Priority 1 — Classification

Determine what kind of thing the proposed concept is.

Examples include:

* Product
* Capability
* Infrastructure
* Integration Pattern
* Service
* Workflow

Do not classify by customer audience, business function, market, or organizational ownership.

---

## Priority 2 — Canonical Ownership

Determine whether the proposed concept already exists elsewhere within the taxonomy.

Identify:

* Duplicate concepts
* Synonyms
* Overlapping capabilities
* Existing authoritative definitions

Recommend extending existing concepts before creating new canonical entries.

---

## Priority 3 — Semantic Boundaries

Evaluate whether the proposed classification preserves clear conceptual boundaries.

Identify situations where:

* Products become confused with capabilities.
* Infrastructure is treated as a product.
* Integration mechanisms become products.
* Related concepts become semantically interchangeable.

Flag recommendations that could create false semantic relationships for AI-assisted retrieval.

---

## Priority 4 — Future Scalability

Determine whether the taxonomy continues to support predictable growth.

Ask:

* Can similar concepts be classified consistently?
* Does this introduce a new organizing principle?
* Will future reviewers know where similar concepts belong?

---

# Review Process

## Step 1 — Identify the Concept

Determine what has been proposed.

Summarize:

* Business purpose
* Technical purpose
* Intended audience
* Dependencies

---

## Step 2 — Determine Classification

Classify the concept as one of:

* Product
* Capability
* Infrastructure
* Integration Pattern
* Service
* Workflow

Explain the reasoning.

---

## Step 3 — Validate Canonical Ownership

Determine whether:

* The concept already exists.
* The concept overlaps an existing concept.
* Documentation should extend an existing canonical page.

Recommend consolidation where appropriate.

---

## Step 4 — Evaluate Semantic Boundaries

Review the proposed change for semantic clarity.

Examples:

* Does this blur the distinction between products and capabilities?
* Does this create duplicate conceptual ownership?
* Does this teach AI that unrelated concepts are interchangeable?
* Does this introduce ambiguous terminology?

When semantic boundaries become unclear, explain the potential impact on both human readers and AI retrieval systems.

---

## Step 5 — Future Product Test

Determine whether an independent reviewer would classify similar future concepts consistently.

If not, explain why the taxonomy rules are ambiguous.

---

# Output Format

## Concept Reviewed

<State concept>

---

## Recommended Classification

<State classification>

Confidence:

High / Medium / Low

Reasoning:

<Explain classification>

---

## Canonical Ownership

Existing canonical concept:

<State if applicable>

Recommendation:

* Create new canonical concept
* Extend existing concept
* Merge with existing concept

---

## Semantic Boundary Review

Identify any risks of:

* Duplicate concepts
* Mixed classification systems
* Blurred semantic boundaries
* False semantic bridges
* AI retrieval ambiguity

---

## Taxonomy Impact

Describe how the proposed change affects:

* Classification consistency
* Navigation
* Documentation architecture
* Long-term maintainability
* AI-assisted retrieval

---

## Governance Recommendation

Choose one:

* Approve
* Approve with modifications
* Requires architecture review
* Reject

Provide evidence-based justification.

---

## Future Product Test

Would a future reviewer classify similar concepts consistently?

Result:

Pass / Needs Review / Fail

Explain the reasoning.

---

## Open Questions

List any questions requiring:

* Product review
* Documentation architecture review
* Information architecture review
* Engineering review

Do not invent product behavior.

When uncertain, state the uncertainty explicitly.

Focus on preserving a stable, scalable, and semantically consistent product taxonomy.

