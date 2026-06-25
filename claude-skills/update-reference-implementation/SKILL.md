# update-reference-specification.md

```yaml
---
name: update-reference-oas-from-json
description: Reviews JSON payloads and proposes updates to the Documentation Reference OAS, Schema Inventory, and Provenance Matrix based on newly observed schema behavior.
when_to_use: Use ONLY when explicitly asked to update, extend, compare, analyze, or maintain a Documentation Reference OAS using new JSON request, response, callback, webhook, introspect, wallet, tokenization, APM, 3DS, or other API payloads.
argument-hint: "<json payload>"
arguments: optional
user-invocable: true
---
```

# Documentation Reference OAS Updater

## Purpose

Review JSON request and response payloads as a senior technical writer and schema analyst.

The objective is to identify updates required for an internal Documentation Reference OpenAPI Specification (OAS) while keeping recommendations evidence-based, traceable, and workflow-focused.

Evaluate:

* Schema completeness
* Workflow coverage
* New fields
* New schemas
* New enum values
* Type conflicts
* Workflow variants
* Provenance confidence
* Impact on supporting documentation assets

---

# Supporting Knowledge

This skill includes the following reference files.

## documentation-reference-oas.yaml

Use the Documentation Reference OAS as the authoritative schema baseline.

Consult this file to determine:

* Existing schemas
* Existing fields
* Existing workflow models
* Existing enum values
* Existing type definitions

Do **not** propose changes without comparing against the current OAS.

---

## schema-inventory.md

Use the Schema Inventory as the authoritative catalog of known fields and containers.

Consult this file to determine:

* Known fields
* Known containers
* Known schema organization
* Existing workflow structures

The Schema Inventory takes precedence over assumptions.

---

## provenance-matrix.md

Use the Provenance Matrix as the authoritative source for:

* Field confidence
* Schema confidence
* Source evidence
* Existing provenance assignments

Do **not** increase confidence levels without supporting evidence.

---

## update-rules.md

Use **update-rules.md** as the authoritative source for:

* Field addition rules
* Schema addition rules
* Enum addition rules
* Confidence assignment rules
* Workflow variant rules

These rules take precedence over inferred behavior.

When assigning `x-source`, follow **update-rules.md** exactly.

Do **not** assign `canonical-json` unless the user explicitly identifies the supplied payload as canonical.

---

# Review Priorities

Always prioritize findings in the following order.

## Priority 1 — Schema Gaps

Examples:

* New field observed
* New container observed
* New schema observed
* New workflow structure observed

These findings are more important than enum additions.

Report these first.

---

## Priority 2 — Workflow Coverage

Examples:

* New workflow variant
* New callback structure
* New webhook structure
* New API lifecycle event
* New authentication workflow variation

These findings are more important than minor schema improvements.

---

## Priority 3 — Schema Refinement

Examples:

* New enum values
* Type clarification
* Confidence updates
* Documentation improvements

These are lower priority than schema and workflow coverage.

---

# Scope

Review **only** the payloads supplied.

Do **not**:

* Rebuild the entire OAS
* Reconstruct unrelated workflows
* Review unrelated capabilities
* Generate a complete replacement specification
* Invent undocumented schema behavior

Limit recommendations to changes supported by evidence.

---

# Multiple Payloads

The user may provide:

* Request payloads
* Response payloads
* Callback payloads
* Webhook payloads
* Introspect payloads
* Multiple workflow examples

Review each payload independently.

Then consolidate findings.

Do **not** assume all payloads belong to the same workflow.

---

# Review Process

## Step 1 — Classify Payload Type

Determine whether each payload is:

* Request
* Response
* Callback
* Webhook
* Introspect Request
* Introspect Response
* Unknown

If uncertain, state that clearly.

---

## Step 2 — Identify Workflow

Determine which workflow the payload most closely matches.

Examples include:

* Sale
* Authorization
* Capture
* Void
* Refund
* Verification
* Token Creation
* Existing Token
* Wallet Payment
* Redirect Payment
* Mobile Authentication
* Browser Authentication
* Dynamic Currency Conversion
* Fraud Check
* Partial Capture
* Webhook
* Introspect

If the workflow cannot be determined, state that clearly.

---

## Step 3 — Compare Against Existing Assets

Consult:

* documentation-reference-oas.yaml
* schema-inventory.md
* provenance-matrix.md

Identify:

* New fields
* New containers
* New schemas
* New enum values
* New workflow variants
* Type conflicts
* Structural conflicts

Report **only** differences supported by evidence.

---

## Step 4 — Evaluate Evidence

For every proposed update assign a confidence level.

### High Confidence

Supported by one or more of:

* Existing OAS
* Schema Inventory
* Provenance Matrix
* Multiple workflows
* Multiple payload examples

### Medium Confidence

Supported by:

* Canonical payloads
* One workflow family
* Consistent schema behavior

### Low Confidence

Supported only by:

* Single payload
* Ambiguous field usage
* Limited evidence

---

# Update Categories

## Add Field

Examples:

* New request field
* New response field
* New callback field

---

## Add Schema

Examples:

* AuthenticationResult
* FraudAssessment
* TokenInformation

---

## Add Enum Value

Examples:

* transactionType
* decision
* paymentMethod
* entryMode
* authenticationResult
* lifecycleState

Do **not** treat the following as enum candidates:

* GUIDs
* Tokens
* URLs
* Timestamps
* Merchant values
* Customer values

---

## Add Workflow Variant

Examples:

* New authentication path
* New callback type
* New lifecycle event
* New tokenization pattern
* New payment pattern

---

## Review Required

Use this category when:

* Evidence conflicts
* Existing OAS behavior conflicts
* Confidence cannot be determined

---

# Confidence Rules

## High

Strong evidence from:

* Multiple sources
* Existing OAS
* Existing Inventory
* Existing Provenance Matrix

---

## Medium

Moderate evidence from:

* Canonical payloads
* Consistent workflow behavior

---

## Low

Limited evidence from:

* Single payload
* Ambiguous structure

---

# Required Field Rules

Do **not** mark a field as required unless:

* It is required by the current OAS.
* It is required by the workflow definition.
* It appears consistently across observed workflow examples.
* It is structurally necessary.

When uncertain, report:

```text
Required Candidate – Needs Review
```

---

# Placeholder Handling

Ignore documentation placeholders when evaluating schema updates.

Examples:

* `<id>`
* `<requestId>`
* `<token>`
* `<contextId>`

Evaluate only the underlying data type.

Do **not**:

* Create enums from placeholders.
* Create schema fields from placeholders.

---

# Output Format

## Workflow Detected

<State workflow>

## Payload Type

<State payload type>

## Proposed OAS Updates

### Add Fields

<List>

### Add Schemas

<List>

### Add Enum Values

<List>

### Add Workflow Variants

<List>

### Review Required

<List>

---

## Suggested YAML Patch

Provide **only** the minimal YAML additions or modifications required.

Do **not** regenerate the entire OAS unless explicitly requested.

---

## Provenance Assessment

For each major change include:

* Confidence level
* Supporting evidence
* Source justification

---

## Impact Assessment

Identify impacted assets.

Examples:

* Documentation Reference OAS
* Schema Inventory
* Provenance Matrix
* Example Review Skill
* Schema Validator
* Workflow Matrix

---

## Open Questions

List issues requiring:

* Product review
* Engineering review
* Architecture review
* Additional payload evidence

Focus on evidence-based updates.

Do **not** invent schema behavior.

When uncertainty exists, state it explicitly.

