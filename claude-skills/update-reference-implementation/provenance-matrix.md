# API Provenance Matrix

## Purpose

This matrix documents the evidence supporting each field, schema, and container included in a Documentation Reference OpenAPI Specification (OAS). It compares available source specifications with canonical payload libraries to support documentation review, schema reconstruction, governance, and future updates.

This is **not** an implementation-owned specification. It is a documentation evidence map derived from available reference sources.

---

## Source Columns

- **Reference OAS A** — Primary available OpenAPI specification
- **Reference OAS B** — Secondary available OpenAPI specification
- **Canonical Payload Library — Core**
- **Canonical Payload Library — Tokenization**
- **Canonical Payload Library — Wallets**
- **Canonical Payload Library — Authentication**
- **Canonical Payload Library — Lifecycle**
- **Canonical Payload Library — Optional Features**

> Replace these source names with project-specific artifacts as needed.

---

## Confidence Legend

- **High** — Field appears in at least one reference specification and at least one canonical payload source.
- **Medium-High** — Field appears in multiple canonical payload collections but not in the available specifications.
- **Medium** — Field appears only in canonical payloads.
- **Specification-only** — Field appears only in the available specifications and was not observed in canonical payloads.

---

## Reading the Matrix

- **✓** indicates the field was observed in the corresponding source.
- Specification-only fields may still be valid but should be reviewed before publication if they are not observed in current payload examples.
- Canonical-only fields are strong candidates for inclusion in the Documentation Reference OAS.

---

## Summary

| Confidence | Field Count |
|---|---:|
| High | _Calculated_ |
| Medium-High | _Calculated_ |
| Medium | _Calculated_ |
| Specification-only | _Calculated_ |

---

## Candidate Fields for Documentation Reference OAS

List fields that appear in canonical payloads but not in the available reference specifications. These represent candidate additions to the documentation reference model and should be reviewed using documented governance rules before inclusion.

---

## Container Sections

Organize fields by logical container (for example `workflow`, `credentials`, `consumer`, `payment`, etc.) using the following structure.

| Field | Type | Reference OAS A | Reference OAS B | Canonical Payload Set 1 | Canonical Payload Set 2 | Canonical Payload Set 3 | Confidence |
|---|---|---|---|---|---|---|---|

Repeat for each container discovered during analysis.

---

## Workflow Coverage Notes

Summarize the workflow families represented by the available payload collections.

Examples include:

- Core transaction workflows
- Tokenization workflows
- Authentication workflows
- Wallet workflows
- Redirect workflows
- Callback workflows
- Webhook workflows
- Lifecycle workflows
- Optional service workflows

---

## Recommended Usage

Use this provenance matrix together with the Schema Inventory and Documentation Reference OAS when reconstructing or maintaining documentation.

Fields with **High** confidence can generally be included directly.

Fields with **Medium** or **Medium-High** confidence should include provenance metadata such as `x-source` and `x-documentation-confidence`.

Fields marked **Specification-only** should be reviewed before inclusion if they are not relevant to the current documentation set.
