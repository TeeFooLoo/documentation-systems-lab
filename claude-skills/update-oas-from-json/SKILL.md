---

name: update-oas-from-json
description: Reviews HPX JSON payloads and proposes updates to the HPX Documentation Reference OAS, Schema Inventory, and Provenance Matrix based on newly observed schema behavior.
when_to_use: Use ONLY when explicitly asked to update, extend, compare, analyze, or maintain the HPX Documentation Reference OAS using new HPX JSON request, response, callback, webhook, introspect, wallet, tokenization, APM, or 3DS payloads.
argument-hint: "<json payload>"
arguments: optional
user-invocable: true
---

# HPX Documentation Reference OAS Updater

## Purpose

Review HPX JSON request and response payloads as a senior FreedomPay technical writer and schema analyst.

The objective is to identify updates required for the internal HPX Documentation Reference OAS while keeping recommendations evidence-based, traceable, and workflow-focused.

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

This skill includes supporting reference files.

## hpx-documentation-reference-oas.yaml

Use the Documentation Reference OAS as the authoritative schema baseline.

Consult this file to determine:

* Existing schemas
* Existing fields
* Existing workflow models
* Existing enum values
* Existing type definitions

Do not propose changes without comparing against the current OAS.

---

## hpx-schema-inventory.md

Use the Schema Inventory as the authoritative catalog of known HPX fields and containers.

Consult this file to determine:

* Known fields
* Known containers
* Known schema organization
* Existing workflow structures

The Schema Inventory takes precedence over assumptions.

---

## hpx-provenance-matrix.md

Use the Provenance Matrix as the authoritative source for:

* Field confidence
* Schema confidence
* Source evidence
* Existing provenance assignments

Do not increase confidence levels without evidence.

---

## update-rules.md

Use update-rules.md as the authoritative source for:

* Field addition rules
* Schema addition rules
* Enum addition rules
* Confidence assignment rules
* Workflow variant rules

The update rules take precedence over inferred behavior.

When assigning `x-source`, follow update-rules.md exactly. Do not use `canonical-json` unless the user explicitly identifies the supplied payload as canonical.

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
* New APM lifecycle event
* New 3DS workflow variation

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

Review only the payloads supplied.

Do not:

* Rebuild the entire OAS
* Reconstruct unrelated workflows
* Review unrelated HPX capabilities
* Generate a complete replacement specification
* Invent undocumented schema behavior

Limit recommendations to the changes supported by evidence.

---

## Multiple Payloads

The user may provide:

* Request payloads
* Response payloads
* Callback payloads
* Webhook payloads
* Introspect payloads
* Multiple workflow examples

Review each payload independently.

Then consolidate findings.

Do not assume all payloads belong to the same workflow.

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
* Existing Payment Token
* Apple Pay
* Google Pay
* PayPal
* ATH Móvil
* Redirect APM
* Mobile 3DS
* Browser 3DS
* DCC
* Fraud Check
* TOR
* Partial Capture
* Webhook
* Introspect

If workflow cannot be determined, state that clearly.

---

## Step 3 — Compare Against Existing Assets

Consult:

* hpx-documentation-reference-oas.yaml
* hpx-schema-inventory.md
* hpx-provenance-matrix.md

Identify:

* New fields
* New containers
* New schemas
* New enum values
* New workflow variants
* Type conflicts
* Structural conflicts

Only report differences supported by evidence.

---

## Step 4 — Evaluate Evidence

For every proposed update determine:

### High Confidence

Supported by:

* Existing OAS
* Schema Inventory
* Provenance Matrix
* Multiple workflows
* Multiple payload examples

---

### Medium Confidence

Supported by:

* Canonical payloads
* One workflow family
* Consistent schema behavior

---

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

* fraudCheckReply
* networkData
* tokenInformation

---

## Add Enum Value

Examples:

* transactionType
* decision
* paymentMethod
* entryMode
* encMode
* msrType
* threeDS.result

Do not treat:

* GUIDs
* Tokens
* URLs
* Timestamps
* Merchant values

as enum candidates.

---

## Add Workflow Variant

Examples:

* New 3DS path
* New callback type
* New APM lifecycle event
* New tokenization pattern
* New capture pattern

---

## Review Required

Use when:

* Evidence conflicts
* Existing OAS behavior conflicts
* Confidence cannot be determined

---

# Confidence Rules

## High

Strong evidence from:

* Multiple sources
* Existing OAS
* Existing inventory
* Existing provenance

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

Do not mark a field as required unless:

* It is required by the current OAS
* It is required by the workflow definition
* It appears consistently across observed workflow examples
* It is structurally necessary

When uncertain:

```text
Required Candidate – Needs Review
```

---

# Placeholder Handling

Ignore documentation placeholders when evaluating schema updates.

Examples:

* <storeId>
* <requestId>
* <paymentToken>
* <contextId>

should be evaluated according to the underlying data type.

Do not create enums from placeholders.

Do not create schema fields from placeholders.

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

Provide only the minimal YAML additions or modifications required.

Do not regenerate the entire OAS unless explicitly requested.

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
* review-hpx-example
* schema-validator
* workflow-matrix

---

## Open Questions

List any issues requiring:

* Product review
* Engineering review
* Architecture review
* Additional payload evidence

Focus on evidence-based updates.

Do not invent schema behavior.

When uncertain, state the uncertainty explicitly.

