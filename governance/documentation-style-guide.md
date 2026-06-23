# Documentation Style Guide

## Purpose

This guide defines the writing and formatting conventions used throughout Documentation Systems Lab.

The objective is to produce documentation that is:

- Consistent
- Accurate
- Scannable
- Maintainable
- Accessible
- Reusable

These guidelines apply to all documentation in this repository, including architecture guides, API references, templates, examples, AI workflows, and Claude Skills.

This repository favors complete, context-rich documentation over overly compressed content, especially when the documentation may be used by AI assistants, search systems, or downstream documentation automation.

---

# Guiding Principles

Good documentation should prioritize understanding over completeness.

When writing documentation:

- Explain concepts before implementation.
- Prefer clarity over cleverness.
- Use progressive disclosure.
- Write for readers who are solving a problem.
- Remove unnecessary complexity.
- Avoid duplication whenever possible.
- Keep examples realistic and complete.

---

# Voice and Tone

Documentation should be:

- Professional
- Clear
- Direct
- Friendly
- Confident
- Neutral

Avoid:

- Marketing language
- Hyperbole
- Humor that may not translate
- Internal jargon
- Assumptions about reader expertise

---

## Explain Enough Context

Documentation should be concise, but not terse.

Readers and AI tools both need enough context to understand how a feature works, when to use it, and what decisions to make. Overly brief documentation may look clean, but it often fails when users need to troubleshoot, compare options, or ask AI tools to reason across the documentation set.

Do not remove important explanations only to make a page shorter.

Good documentation should:

- Explain what the feature does.
- Explain when to use it.
- Explain when not to use it.
- Describe important dependencies or constraints.
- Include complete examples.
- Preserve workflow context.
- Define terms before using them.
- Link to related concepts.

Avoid documentation that only lists fields, parameters, or steps without explaining how they fit together.

Concise documentation removes clutter.

Terse documentation removes meaning.
---
# Document Types

Documentation generally falls into four categories.

## Tutorials

Help users accomplish a learning objective.

Characteristics:

- Sequential
- Beginner-friendly
- Explains why
- Assumes little prior knowledge

---

## How-to Guides

Help users complete a task.

Characteristics:

- Goal-oriented
- Practical
- Assumes basic familiarity
- Focuses on successful completion

---

## Reference

Describe facts.

Characteristics:

- Complete
- Precise
- Searchable
- Minimal narrative

Examples include:

- API endpoints
- Field definitions
- Error codes
- Configuration options

---

## Concepts

Explain ideas.

Characteristics:

- Educational
- Contextual
- Explains relationships
- Supports deeper understanding

---

# Document Structure

Most documents should follow this structure:

Overview

Prerequisites

Concepts

Procedure

Examples

Related Topics

---

# Headings

Use sentence case.

Good

## Configure authentication

Avoid

## Configuring Authentication

---

# Lists

Use numbered lists when order matters.

Use bullets when order does not matter.

Keep list items parallel.

---

# Code Blocks

Always specify a language.

Good

```json
{
  "example": true
}
```

Prefer complete examples over fragments.

Examples should be executable whenever practical.

---

# JSON Examples

Examples should:

- Be syntactically valid
- Include realistic values
- Use consistent formatting
- Avoid placeholders unless necessary

Prefer

```json
{
  "customerId": "cust_12345"
}
```

instead of

```json
{
  "id": "123"
}
```

---

# API Examples

Every endpoint should include:

Purpose

Request

Response

Field explanations

Common errors

Related endpoints

---

# Tables

Use tables for structured comparisons.

Avoid tables containing large paragraphs.

---

# Images

Images should support the text rather than replace it.

Prefer diagrams that explain workflows over screenshots of interfaces.

---

# Cross-linking

Link to related concepts rather than repeating information.

Documentation should form a connected system rather than isolated pages.

---

# Accessibility

Use descriptive headings.

Avoid relying solely on color.

Provide meaningful alt text for images.

Use descriptive link text.

---

# Inclusive Language

Prefer:

allowlist

denylist

primary

secondary

they

their

Avoid language that excludes readers or assumes background knowledge.

---

# AI-Assisted Authoring

AI can accelerate documentation but should not replace technical review.

When using AI:

- Verify technical accuracy.
- Preserve product terminology.
- Review examples manually.
- Confirm workflows with subject matter experts.
- Ensure generated content aligns with repository standards.

AI should improve author productivity—not replace editorial judgment.

---

# Documentation Review Checklist

Before publishing:

- Technical accuracy verified
- Examples validated
- Terminology consistent
- Links tested
- Formatting consistent
- Accessibility reviewed
- Grammar checked
- Cross-links added

---

# Future Improvements

This guide will evolve as new documentation patterns, tooling, and AI-assisted workflows emerge.
