# Documentation Patterns

Documentation Patterns are reusable solutions to common documentation challenges.

Just as software engineers use design patterns to solve recurring architectural problems, documentation teams can apply documentation patterns to create consistent, maintainable, and user-centered documentation across products, APIs, and developer platforms.

Rather than prescribing exact wording or page layouts, a documentation pattern captures the reasoning behind an effective documentation solution. It explains the problem being solved, when the pattern should be used, how it should be implemented, and the trade-offs involved.

Patterns promote consistency without limiting flexibility. Individual documentation pages may vary in content, but recurring documentation problems should be solved using consistent approaches.

## Objectives

The Documentation Patterns collection aims to:

- Capture proven approaches to recurring documentation challenges.
- Promote consistency across documentation systems.
- Improve discoverability and usability.
- Reduce documentation design decisions through reusable patterns.
- Support scalable documentation architecture.
- Establish best practices that can be validated through AI-assisted review workflows.

## Relationship to the Content Model

The Documentation Content Model defines **what types of pages exist** within a documentation system.

Documentation Patterns define **how common documentation problems should be solved** within those page types.

For example:

- A **Capability Page** is defined by the Content Model.
- An **Authentication Pattern** describes the information that should appear when documenting authentication as a capability.

The Content Model provides structure.

Documentation Patterns provide implementation guidance.

## Pattern Structure

Each pattern in this directory follows a consistent structure.

| Section | Purpose |
|----------|---------|
| **Overview** | Introduces the documentation challenge. |
| **Problem Statement** | Describes the recurring problem the pattern addresses. |
| **When to Use** | Explains when the pattern is appropriate. |
| **When Not to Use** | Identifies situations where another approach is preferable. |
| **Recommended Structure** | Describes the suggested organization of the documentation. |
| **Examples** | Demonstrates the pattern using fictional examples. |
| **Common Mistakes** | Highlights frequent implementation errors. |
| **AI Review Considerations** | Suggests criteria that AI-assisted review tools can validate. |
| **Future Improvements** | Identifies opportunities to evolve the pattern over time. |

## Planned Patterns

This directory will grow over time as additional patterns are documented.

Planned patterns include:

### API Documentation

- Authentication
- Authorization
- Pagination
- Rate Limiting
- Error Documentation
- Versioning
- Deprecation
- Idempotency
- Long-Running Operations
- Asynchronous Processing
- Polling
- Callbacks
- Webhooks
- Event-Driven APIs
- SDK Documentation

### Developer Experience

- Getting Started
- Quickstart Guides
- Installation
- Configuration
- Environment Setup
- Migration Guides
- Troubleshooting
- Release Notes

### Information Architecture

- Cross-Linking
- Progressive Disclosure
- Capability Pages
- Workflow Pages
- Concept Pages
- Reference Pages
- Landing Pages

### Documentation Systems

- Canonical Example Pattern
- Content Reuse
- Documentation Ownership
- Navigation Design
- Search Optimization
- AI-Assisted Documentation
- Documentation Review Workflows

## Design Philosophy

Documentation patterns are intended to capture architectural knowledge rather than editorial preference.

A pattern should explain *why* a documentation solution works, not simply prescribe *what* should be written. Contributors are encouraged to adapt patterns to their own products and documentation platforms while preserving the underlying principles.

Patterns should be technology-agnostic whenever practical and focus on improving the developer experience regardless of the tools, frameworks, or documentation platforms involved.

## Relationship to Other Repository Sections

This repository separates documentation architecture into complementary layers.

| Section | Purpose |
|---------|---------|
| **Documentation Principles** | Defines the philosophy behind documentation systems. |
| **Documentation Content Model** | Defines the types of documentation that exist. |
| **Documentation Style Guide** | Defines writing and editorial standards. |
| **Documentation Patterns** | Defines reusable solutions to recurring documentation problems. |
| **Templates** | Provides reusable starting points for creating documentation. |
| **AI Workflows** | Demonstrates how AI can assist documentation creation and review. |
| **Claude Skills** | Implements reusable AI capabilities that validate documentation against the repository's standards and patterns. |

Together, these layers form a complete documentation system, enabling documentation to remain consistent, maintainable, and scalable as products, teams, and technologies evolve.
