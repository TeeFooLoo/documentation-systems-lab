---
name: taxonomy-governance-review
description: Reviews proposed additions or changes to a product taxonomy and evaluates their impact on classification, canonical ownership, semantic boundaries, AI readiness, and long-term maintainability.
when_to_use: Use ONLY when explicitly asked to add, classify, modify, review, validate, or govern a product taxonomy or information architecture.
argument-hint: "<existing taxonomy and proposed concept>"
arguments: optional
user-invocable: true
---

# Product Taxonomy Governance

## Purpose

Review proposed additions or modifications to a product taxonomy as a senior documentation architect and information architecture specialist.

The objective is to preserve a consistent, scalable, and AI-ready taxonomy while ensuring every concept has a clear semantic classification and canonical ownership.

Evaluate:

- Classification consistency
- Canonical ownership
- Semantic boundaries
- Duplicate concepts
- Taxonomy scalability
- Information architecture impact
- AI retrieval implications
- Governance recommendations

---

# Supporting Knowledge

This skill should consult repository guidance including:

## product-taxonomies.md

Use as the authoritative source for:

- Taxonomy design principles
- Classification rules
- Canonical ownership
- Perspective versus ontology
- Future Product Test
- Semantic boundary guidance

---

## information-architecture.md

Consult for:

- Information architecture principles
- Navigation versus taxonomy
- Metadata
- Classification systems
- Governance
- AI-ready documentation

---

## documentation-principles.md

Consult for:

- Documentation philosophy
- System design principles
- Architectural consistency

---

## documentation-content-model.md

Consult for:

- Canonical content ownership
- Content relationships
- Reuse strategy
- Page types

---

# Classification Rules

Always classify concepts according to **what they are**, not **how they are viewed**.

Classify by ontology, not perspective.

Never classify concepts according to:

- Organizational ownership
- Department
- Business unit
- Customer segment
- Marketing terminology
- Sales motion
- Product launch strategy

If the proposed taxonomy mixes organizing principles, explain why it reduces scalability and recommend a consistent classification model.

---

# Review Priorities

## Priority 1 — Classification

Determine whether the proposed concept is a:

- Product
- Capability
- Infrastructure
- Integration Pattern
- Service
- Workflow

---

## Priority 2 — Canonical Ownership

Determine whether:

- The concept already exists
- It duplicates another concept
- Existing documentation should be extended
- A new canonical concept is justified

---

## Priority 3 — Semantic Boundaries

Determine whether the proposal:

- Blurs products and capabilities
- Treats infrastructure as products
- Confuses integration mechanisms with products
- Introduces ambiguous terminology
- Creates false semantic bridges that could mislead AI retrieval systems

---

## Priority 4 — Future Scalability

Determine whether the taxonomy remains predictable as additional concepts are introduced.

Apply the Future Product Test.

---

# Scope

Review only the proposed taxonomy changes.

Do not:

- Rebuild the entire taxonomy unless requested
- Invent undocumented concepts
- Reclassify unrelated areas
- Introduce new organizing principles without justification

Keep recommendations evidence-based.

---

# Review Process

## Step 1 — Identify the Concept

Summarize:

- Business purpose
- Technical purpose
- Intended audience
- Existing relationships

---

## Step 2 — Determine Classification

Recommend exactly one primary classification.

Explain why competing classifications were rejected.

---

## Step 3 — Validate Canonical Ownership

Determine whether the concept:

- Already exists
- Overlaps another concept
- Should extend an existing canonical definition
- Requires a new canonical definition

---

## Step 4 — Evaluate Semantic Boundaries

Review whether the proposed change preserves clear conceptual boundaries.

Highlight any potential false semantic bridges that could cause humans or AI systems to infer incorrect relationships.

---

## Step 5 — Future Product Test

Determine whether an independent reviewer would classify similar future concepts consistently.

If not, explain which taxonomy rules are ambiguous.

---

# Output Format

## Concept Reviewed

<State concept>

---

## Recommended Classification

Classification:

Confidence:

Reasoning:

---

## Canonical Ownership

Recommendation:

- Create new canonical concept
- Extend existing concept
- Merge with existing concept

Reasoning:

---

## Semantic Boundary Review

Identify risks including:

- Duplicate concepts
- Mixed classification systems
- Ambiguous terminology
- False semantic bridges
- AI retrieval ambiguity

---

## AI Impact Assessment

Evaluate whether the proposal strengthens or weakens the semantic model used by:

- Developer copilots
- Retrieval-Augmented Generation (RAG)
- Documentation assistants
- Intelligent search

---

## Taxonomy Impact

Assess the effect on:

- Classification consistency
- Information architecture
- Navigation
- Documentation reuse
- Long-term maintainability

---

## Governance Recommendation

Choose one:

- Approve
- Approve with modifications
- Requires architecture review
- Reject

Provide evidence-based justification.

---

## Future Product Test

Result:

- Pass
- Needs Review
- Fail

Explain the reasoning.

---

## Taxonomy Health (Optional)

Provide an overall assessment:

- Classification Consistency
- Canonical Ownership
- Semantic Boundaries
- AI Readiness
- Scalability

Overall Assessment:

Healthy / Needs Improvement / At Risk

---

## Open Questions

List issues requiring:

- Product review
- Architecture review
- Documentation review
- Additional evidence

When uncertain, state the uncertainty explicitly.

Focus on preserving a stable, scalable, and semantically consistent taxonomy rather than simply placing new concepts into a hierarchy.
