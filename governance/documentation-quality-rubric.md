# Documentation Quality Rubric

**Version:** 1.0  
**Status:** Draft  
**Repository:** Documentation Systems Lab

---

# Purpose

The Documentation Quality Rubric provides a structured method for evaluating the maturity of documentation.

Unlike a publication checklist, which determines whether documentation is ready to publish, this rubric evaluates **how well** documentation serves its audience. It is intended for documentation architects, technical writers, Developer Experience teams, engineering reviewers, and AI-assisted documentation review systems.

The rubric is designed to produce repeatable, evidence-based evaluations rather than subjective opinions.

---

# How to Use This Rubric

Each dimension is evaluated independently using five maturity levels.

| Level | Name | Description |
|-------|------|-------------|
| **1** | Basic | Documentation exists but provides minimal value. |
| **2** | Functional | Supports basic implementation but leaves important questions unanswered. |
| **3** | Developer Friendly | Supports successful implementation with reasonable confidence. |
| **4** | Excellent | Anticipates common questions, reduces support burden, and provides a complete implementation experience. |
| **5** | Documentation System | Fully integrated into a governed documentation ecosystem with reusable content, strong information architecture, and AI-ready structure. |

A page does not need every category to reach the same level. Reviewers should score each dimension independently and provide recommendations for improvement.

---

# Evaluation Dimensions

## 1. Purpose

| Level | Criteria |
|------|----------|
| 1 | Purpose is unclear or implied. |
| 2 | Purpose is stated but audience or objectives are vague. |
| 3 | Purpose and audience are clearly defined. |
| 4 | Documentation is aligned with user goals and expected outcomes. |
| 5 | Purpose is consistently reinforced through navigation, structure, and related documentation. |

---

## 2. Content Completeness

| Level | Criteria |
|------|----------|
| 1 | Major information is missing. |
| 2 | Covers the primary task but omits important context. |
| 3 | Includes concepts, workflow, examples, and reference information. |
| 4 | Answers common implementation and troubleshooting questions. |
| 5 | Anticipates edge cases, decision points, and follow-up questions without overwhelming the reader. |

---

## 3. Workflow Guidance

| Level | Criteria |
|------|----------|
| 1 | No workflow described. |
| 2 | Partial workflow with significant assumptions. |
| 3 | Complete workflow with prerequisites and outcomes. |
| 4 | Includes alternate paths, dependencies, and verification steps. |
| 5 | Integrates multiple capabilities into complete implementation journeys. |

---

## 4. Canonical Examples

| Level | Criteria |
|------|----------|
| 1 | No examples. |
| 2 | Partial or simplified examples. |
| 3 | Complete request and response examples. |
| 4 | Canonical examples covering recommended implementations and common errors. |
| 5 | Complete examples across workflows, SDKs, CLI, OpenAPI, and related documentation, validated for accuracy. |

---

## 5. Technical Accuracy

| Level | Criteria |
|------|----------|
| 1 | Unverified or outdated. |
| 2 | Generally accurate but missing validation. |
| 3 | Reviewed by SMEs and technically correct. |
| 4 | Synchronized with implementation and examples. |
| 5 | Continuously validated through automated checks and review processes. |

---

## 6. Information Architecture

| Level | Criteria |
|------|----------|
| 1 | Difficult to navigate. |
| 2 | Basic organization but inconsistent structure. |
| 3 | Consistent page structure and logical navigation. |
| 4 | Organized around user goals and capability-based navigation. |
| 5 | Integrated knowledge system with authoritative sources, reusable content, and minimal duplication. |

---

## 7. Discoverability

| Level | Criteria |
|------|----------|
| 1 | Difficult to locate. |
| 2 | Accessible through navigation only. |
| 3 | Searchable with meaningful headings and metadata. |
| 4 | Strong cross-linking and contextual navigation. |
| 5 | Optimized for navigation, search, metadata, and AI retrieval. |

---

## 8. Maintainability

| Level | Criteria |
|------|----------|
| 1 | High maintenance effort and duplicated content. |
| 2 | Some reusable content. |
| 3 | Consistent templates and limited duplication. |
| 4 | Reusable architecture with centralized concepts. |
| 5 | Governed documentation system supporting scalable maintenance and automation. |

---

## 9. AI Readiness

| Level | Criteria |
|------|----------|
| 1 | AI cannot reliably interpret the content. |
| 2 | Limited semantic context. |
| 3 | Sufficient context for accurate summarization. |
| 4 | Explicit relationships, workflows, and terminology support reliable AI reasoning. |
| 5 | Structured for AI-assisted authoring, validation, search, and knowledge retrieval. |

---

## 10. Developer Experience

| Level | Criteria |
|------|----------|
| 1 | Requires frequent external assistance. |
| 2 | Enables basic onboarding with gaps. |
| 3 | Developers can complete common tasks independently. |
| 4 | Documentation prevents common mistakes and minimizes support requests. |
| 5 | Documentation becomes the trusted source of truth for developers and internal teams alike. |

---

# Page Quality vs. System Quality

Documentation quality should be evaluated at two levels.

## Page Quality

Individual pages should be assessed for:

- Purpose
- Accuracy
- Completeness
- Workflow
- Examples
- Style
- Accessibility

## System Quality

Documentation systems should additionally be assessed for:

- Information architecture
- Navigation
- Discoverability
- Single source of truth
- Content reuse
- Governance
- Metadata
- AI readiness

Excellent pages cannot compensate for poor system architecture. Likewise, a strong documentation architecture cannot overcome incomplete or inaccurate content.

---

# Example Assessment

| Dimension | Score |
|----------|------:|
| Purpose | 5 |
| Completeness | 4 |
| Workflow | 5 |
| Examples | 2 |
| Technical Accuracy | 5 |
| Information Architecture | 4 |
| Discoverability | 4 |
| Maintainability | 4 |
| AI Readiness | 5 |
| Developer Experience | 4 |

**Overall Observation**

The documentation provides a strong conceptual model and workflow guidance but would benefit from additional canonical examples to improve implementation confidence and reduce support burden.

---

# What Level 5 Looks Like

Level 5 documentation is not merely accurate—it is an integrated knowledge system.

It anticipates user questions before they are asked, provides canonical examples that demonstrate recommended implementations, and organizes information around user goals rather than internal organizational structures. Every page has a clear purpose, follows a consistent content model, and contributes to a coherent documentation ecosystem rather than standing alone.

A Level 5 documentation system is governed by shared principles, writing standards, review processes, and reusable patterns. Concepts are documented once and reused throughout the documentation set. Navigation, metadata, and cross-linking work together to improve discoverability while reducing duplication and long-term maintenance effort.

Finally, Level 5 documentation is designed for both humans and machines. It contains enough semantic richness, context, and structural consistency that AI-assisted search, documentation review, and knowledge retrieval systems can reason over it accurately without replacing the judgment of experienced technical writers. Automation strengthens quality, but human expertise remains the final authority.

---

# Future Evolution

Future versions of this rubric may include:

- Weighted scoring
- Radar chart visualizations
- Automated scoring through AI review agents
- Repository-wide maturity reporting
- CI/CD quality gates
- Metrics correlated with support reduction and developer success
