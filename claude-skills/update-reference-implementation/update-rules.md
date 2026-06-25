# OAS Update Rules

## Purpose

Maintain the Documentation Reference OAS using newly observed JSON payloads.

The Documentation Reference OAS is an internal documentation artifact used by the Documentation Team.

It supports:

* Documentation review
* Documentation generation
* Workflow analysis
* Schema discovery
* AI-assisted documentation tooling
* Documentation Reference OAS maintenance

The Documentation Reference OAS is not:

* An architecture-owned specification
* A customer-facing specification
* A production API contract
* A replacement for official product specifications

When conflicts occur, prioritize evidence and provenance over assumptions.

Unless the user explicitly states that a supplied payload is from the Canonical JSON Payload Library, treat newly supplied payloads as `observed-json`.

---

# Update Priorities

Always prioritize findings in the following order.

## Priority 1 — New Schema Behavior

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
* New 3DS workflow path

These findings are more important than minor schema refinements.

---

## Priority 3 — Schema Refinement

Examples:

* New enum values
* Type clarification
* Confidence adjustments
* Provenance improvements

These are lower priority than schema and workflow coverage.

---

# Scope

Review only the payloads supplied.

Do not:

* Reconstruct the entire OAS
* Rebuild unrelated workflows
* Invent undocumented behavior
* Generate a replacement specification
* Propose changes unsupported by evidence

Limit recommendations to changes supported by observed payloads.

---

# Update Categories

## Add Field

Add a field when:

* It appears in the supplied payload.
* It is absent from the Documentation Reference OAS.
* It appears to represent API schema behavior.

Examples:

```text
clientPaymentKey
networkData
redirectUrl
supportedVersions
```

Do not add:

* Merchant identifiers
* Store IDs
* Terminal IDs
* Request IDs
* Authorization codes
* Transaction-specific values

These are data values, not schema definitions.

---

## Existing Field Rule

Do not propose an Add Field update when:

* The field already exists in the Documentation Reference OAS.
* The field already exists in the Schema Inventory.

Instead:

* Confirm the field exists.
* Evaluate whether confidence should change.
* Evaluate whether provenance should change.
* Evaluate whether documentation examples should be updated.

Existing fields should not be reported as new fields.

---

## Add Schema

Add a schema when:

* A reusable object container appears.
* The container has workflow significance.
* The structure appears likely to recur.

Examples:

```text
networkData
tokenInformation
fraudCheckReply
walletInformation
```

---

## Add Enum Value

Add an enum value when:

* The field already behaves as an enum.
* The value represents workflow behavior.
* The value is reusable.

Examples:

```text
transactionType
decision
paymentMethod
entryMode
encMode
msrType
threeDS.result
```

Do not add:

* GUIDs
* Tokens
* URLs
* Timestamps
* Merchant-specific values

as enum values.

---

## Add Workflow Variant

Add a workflow variant when:

* The payload represents a materially different workflow.
* Existing workflow models cannot describe the behavior cleanly.

Examples:

* Browser 3DS Provider Error
* APM Cancelled Callback
* Token-Only Verification
* TOR Reversal
* Partial Capture

---

## Review Required

Use when:

* Evidence conflicts
* Existing schema behavior conflicts
* Confidence cannot be determined
* Multiple interpretations are possible

Do not force updates when evidence is weak.

---

# Confidence Rules

## High Confidence

Supported by:

* Documentation Reference OAS
* Schema Inventory
* Provenance Matrix
* Multiple workflow examples
* Multiple payload examples

Examples:

```text
workflow.contextId
merchantReferenceCode
transactionType
requestId
```

---

## Medium Confidence

Supported by:

* Canonical payloads
* One workflow family
* Consistent schema behavior

Examples:

```text
clientPaymentKey
supportedVersions
networkData
```

---

## Low Confidence

Supported only by:

* Single workflow
* Single payload
* Ambiguous structure

Low confidence findings should typically be flagged for review.

---

# Required Field Rules

Do not mark a field as required unless:

* It is already required in the Documentation Reference OAS
* It is required by the workflow definition
* It appears consistently across observed examples
* The workflow cannot function without it

When uncertain:

```text
Required Candidate – Needs Review
```

---

# Placeholder Handling

Ignore documentation placeholders when evaluating schema updates.

Examples:

```text
<storeId>
<requestId>
<paymentToken>
<contextId>
```

Infer the underlying data type.

Do not:

* Create schema fields from placeholders
* Create enum values from placeholders

---

# Source Label Rules

Use provenance labels consistently.

## observed-json

Use when:

* The field is observed in a newly supplied payload.
* The payload is not explicitly identified as canonical.
* The payload is a test payload.
* The payload is supplied during exploratory analysis.

Examples:

```text
User-supplied JSON
Engineering sample payload
Ad hoc testing payload
Feature investigation payload
```

Preferred provenance:

```yaml
x-source:
 - observed-json
```

---

## canonical-json

Use when:

* The field is observed in an approved canonical JSON example.
* The payload originates from the Canonical JSON Payload Library.
* The payload is identified as a documentation reference example.

Preferred provenance:

```yaml
x-source:
 - canonical-json
```

---

## legacy-oas

Use when:

* The field exists in a legacy legacy OpenAPI specification.
* The field is inherited from historical specification artifacts.

Preferred provenance:

```yaml
x-source:
 - legacy-oas
```

---

## documentation-reference-oas

Use when:

* The field already exists in the current Documentation Reference OAS.

Preferred provenance:

```yaml
x-source:
 - documentation-reference-oas
```

---

## Multiple Sources

When multiple sources support a field, include all relevant evidence.

Example:

```yaml
x-source:
 - canonical-json
 - legacy-oas
```

or

```yaml
x-source:
 - observed-json
 - documentation-reference-oas
```

---

## Source Label Restrictions

Do not label ad hoc test payloads as:

```text
canonical-json
```

unless the user explicitly identifies them as canonical examples.

When uncertain:

```text
observed-json
```

is the preferred source label.

---

# Provenance Rules

Every proposed update should include provenance.

Preferred examples:

```yaml
x-source:
 - canonical-json

x-documentation-confidence: medium
```

or

```yaml
x-source:
 - canonical-json
 - legacy-oas

x-documentation-confidence: high
```

Confidence should reflect evidence rather than assumptions.

---

# Performance Guardrails

The purpose of this skill is to identify schema changes supported by the supplied payloads.

Do not perform a full ecosystem audit unless explicitly requested.

---

## Small Payload Reviews

For payloads under approximately 50 lines:

* Limit analysis to fields present in the payload.
* Avoid full-file schema audits.
* Avoid reproducing large portions of supporting artifacts.
* Focus only on net-new observations.
* Search only relevant schemas.
* Search only relevant containers.
* Search only relevant enum definitions.

Do not summarize:

* The entire Documentation Reference OAS
* The entire Schema Inventory
* The entire Provenance Matrix

---

## Targeted Lookup Strategy

For each supplied payload:

1. Identify the workflow.
2. Identify the affected schema containers.
3. Search only for matching fields.
4. Search only for matching schemas.
5. Search only for matching enum definitions.
6. Report only relevant differences.

Avoid analysis of unrelated workflows.

---

## Large Payload Reviews

For larger payload sets:

* Consolidate duplicate findings.
* Group related schema updates.
* Avoid repeating existing inventory information.
* Focus on net-new observations.

---

## YAML Patch Generation

Generate only the minimal YAML changes required.

Prefer:

```yaml
networkTransactionId:
 type: string
```

over regeneration of entire schema blocks.

Do not regenerate the entire Documentation Reference OAS unless explicitly requested.

---
# Fragment Placement Rule

When the supplied payload is a partial fragment and does not contain enough context to identify whether it is request-side or response-side:

* Do not assign the field to a request or response base schema as final.
* Propose the reusable schema separately.
* Mark the placement as Review Required.
* Provide possible placement options only.

---

## Partial Payload Guidance

Examples:

```text
{
 "payment": {
 "walletInformation": {
 "walletProvider": "APPLEPAY"
 }
 }
}
```

```text
{
 "payment": {
 "networkData": {
 "networkTransactionId": "123456789"
 }
 }
}
```

These examples provide evidence that a field or schema exists.

They do not provide sufficient evidence to determine:

* Request placement
* Response placement
* Shared placement
* Required status
* Workflow-specific placement

---

## Schema-First Modeling

When reviewing partial fragments:

1. Identify the reusable schema.
2. Propose the schema definition.
3. Assign confidence.
4. Record provenance.
5. Mark placement as Review Required if placement cannot be determined.

Prefer:

```yaml
WalletInformation:
 type: object
 properties:
 walletProvider:
 type: string
```

over:

```yaml
PaymentResponseBase:
 walletInformation:
 $ref: '#/components/schemas/WalletInformation'
```

when payload context is incomplete.

---

## Placement Recommendations

When placement is uncertain:

Provide possible placement options.

Examples:

```text
Possible placement:
- PaymentResponseBase
- PaymentRequestBase
- SharedPaymentBase
```

Do not recommend a final placement without sufficient evidence.

---

## Confidence Guidance

A payload fragment may provide:

* High confidence that a field exists
* Medium confidence that a schema exists

A payload fragment does not automatically provide confidence regarding:

* Request-side placement
* Response-side placement
* Required status
* Workflow applicability

These should typically remain Review Required until additional evidence is available.

---

# Open Question Rules

Open Questions should be used when:

* Field purpose is unclear.
* Request/response placement is uncertain.
* Confidence cannot be determined.
* Workflow context is incomplete.

Open Questions should not block reporting a proposed schema update.

Instead:

1. Propose the update.
2. Assign confidence.
3. Record uncertainty.
4. Escalate through Open Questions.

---

# Internal Documentation Policy

The Documentation Reference OAS is an internal working model.

The objective is to maintain an accurate representation of observed API behavior.

When conflicts occur:

1. Preserve evidence.
2. Preserve provenance.
3. Assign confidence.
4. Escalate unresolved conflicts.

Favor:

* Evidence over assumptions
* Traceability over certainty
* Observed behavior over undocumented expectations

---

# Output Expectations

All proposed updates should include:

* Update category
* Supporting evidence
* Confidence level
* Provenance recommendation

Only propose changes supported by observed payloads.

Do not invent schema behavior.

When uncertain, state the uncertainty explicitly.
