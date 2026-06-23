# Getting Started

Welcome to Atlas Commerce.

This guide is designed to help you make your first successful API request as quickly as possible while introducing the core concepts you'll need to build a production-ready integration.

Whether you're processing a payment, creating customer records, handling webhooks, or integrating recurring billing, the Getting Started section provides the foundation for everything that follows.

Rather than introducing every feature at once, this guide follows a progressive learning path. Each topic builds upon the previous one, allowing you to understand the platform before exploring more advanced capabilities.

---

# Before You Begin

Before integrating with Atlas Commerce, you should have:

- A basic understanding of REST APIs.
- Familiarity with JSON request and response payloads.
- A tool for making HTTP requests, such as curl, Postman, or your preferred API client.
- A development environment capable of making outbound HTTPS requests.

No prior knowledge of Atlas Commerce is assumed.

---

# Learning Path

We recommend working through the following topics in order.

## 1. Authentication

Learn how applications authenticate with Atlas Commerce, obtain access tokens, and securely call protected APIs.

Topics include:

- Authentication model
- Access tokens
- API credentials
- Environments
- Scopes and permissions
- Security best practices

---

## 2. Make Your First API Call

Use your authentication credentials to make your first successful API request.

This guide introduces:

- Request headers
- Authorization
- JSON responses
- Common response codes
- Error handling

By the end of this guide, you'll have successfully communicated with the Atlas Commerce platform.

---

## 3. Process Your First Payment

Build your first end-to-end payment workflow.

You'll learn how to:

- Create a payment
- Submit payment details
- Interpret the response
- Handle common failures

This tutorial demonstrates how the core payment workflow fits together before exploring more advanced capabilities.

---

## 4. Understand Core Concepts

Once you've completed your first payment, explore the platform's major concepts.

These include:

- Payments
- Customers
- Orders
- Refunds
- Payment Methods
- Idempotency
- Webhooks
- Error Handling

These concept guides explain why each capability exists and how it fits into the overall platform.

---

## 5. Explore the API Reference

After you're comfortable with the platform concepts, use the API Reference to explore available endpoints, request schemas, response payloads, and error models.

Reference documentation complements the implementation guides by providing detailed technical information for each API operation.

---

# Recommended Learning Journey

```text
Create a Developer Account
          │
          ▼
Authenticate
          │
          ▼
Make Your First API Call
          │
          ▼
Process Your First Payment
          │
          ▼
Understand Core Concepts
          │
          ▼
Explore the API Reference
          │
          ▼
Build Your Integration
```

---

# What Makes This Documentation Different?

The Atlas Commerce documentation is intentionally organized around developer goals rather than internal product boundaries.

Instead of asking you to understand the platform's architecture before you begin, the documentation guides you through practical implementation tasks first, introducing additional concepts only as they become relevant.

Throughout the documentation you'll find:

- Complete implementation workflows
- Canonical request and response examples
- Cross-linked concepts
- Progressive disclosure of advanced topics
- Consistent page structure
- AI-friendly documentation designed for both human readers and AI-assisted developer tools

The objective is simple:

Help you build a successful integration with confidence while minimizing the need for external support.

---

# Next Step

Begin with **Authentication**, where you'll learn how to securely connect your application to the Atlas Commerce platform and make your first authenticated API request.
