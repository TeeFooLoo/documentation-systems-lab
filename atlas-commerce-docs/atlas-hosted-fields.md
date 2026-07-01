---
title: Atlas Hosted Fields
description: Securely collect payment information using PCI-compliant hosted payment fields.
---

# Atlas Hosted Fields

Atlas Hosted Fields allows you to securely collect payment information within your checkout experience while Atlas Commerce captures and processes sensitive cardholder data inside secure hosted iframes.

Your application maintains complete control over the checkout experience, while Atlas manages payment data collection, validation, and security. This approach reduces PCI DSS scope without sacrificing flexibility, making Atlas Hosted Fields suitable for everything from simple payment forms to fully customized checkout experiences.

---

## Why Atlas Hosted Fields?

Traditional payment integrations require merchants to directly handle sensitive payment data, increasing PCI compliance requirements and introducing additional security considerations.

Atlas Hosted Fields separates payment data collection from payment processing.

Your application controls the customer experience while Atlas securely collects and processes cardholder data.

### Key Benefits

- **Reduce PCI DSS scope** by isolating cardholder data inside Atlas-hosted iframes.
- **Maintain complete control** over your checkout layout using your own HTML and CSS.
- **Build faster** with a minimal integration that can be progressively customized.
- **Configure everything through the API**, from field layout to validation behavior.
- **Support advanced payment experiences** including 3D Secure authentication, tokenization, localization, fraud detection, and digital wallets.

---

## Architecture

Atlas Hosted Fields uses a server-driven configuration model.

Your backend creates a checkout session that defines how the payment experience should behave. Your frontend then loads the Hosted Fields JavaScript library, which renders secure payment fields directly into your checkout page.

```text
                    Atlas Hosted Fields

              Create Checkout Session
Merchant Backend ─────────────────────────────► Atlas Payments API
                                                │
                                                │ Creates secure payment session
                                                │
                                                ├── checkoutSessionToken
                                                ├── checkoutContextId
                                                └── hostedFieldsScript
                                                         │
                                                         ▼
Merchant Checkout Page ◄──────────────────────────────────┘
        │
        │ Load Hosted Fields
        │
        ▼
Customer enters payment information
        │
        ▼
Payment data captured securely by Atlas
        │
        ▼
Merchant submits payment for authorization
```

At no point does raw cardholder data pass through your application.

---

## Integration Workflow

Integrating Atlas Hosted Fields consists of three high-level phases.

### 1. Initialize

Your backend creates a checkout session using the Atlas Payments API.

During initialization, Atlas:

- Creates a secure checkout session
- Stores rendering configuration
- Returns a checkout session token
- Returns the Hosted Fields JavaScript library
- Generates a checkout context for payment processing

---

### 2. Render

Your frontend loads the Hosted Fields library using the returned session token.

Atlas securely renders payment inputs into your checkout page using the containers and configuration defined during initialization.

---

### 3. Pay

After the customer completes the payment form:

1. Atlas validates the hosted fields.
2. Optional authentication (such as 3D Secure) is performed when required.
3. Atlas signals that payment data is ready.
4. Your backend submits the payment request.
5. Atlas processes the transaction and returns the payment result.

---

## Documentation

### Getting Started

Learn how to build a basic checkout using Atlas Hosted Fields.

- Quickstart
- Initialize a Checkout Session
- Render Hosted Components

---

### Customize Your Checkout

Configure the appearance and behavior of hosted payment fields.

- Configure Hosted Fields
- Client Events
- Branding
- Optional Features
- 3D Secure

---

### Reference

Detailed technical reference material.

- Field Properties
- Example Payloads
- Error Handling

---

## Before You Begin

Before integrating Atlas Hosted Fields, ensure that:

- Your Atlas Commerce account is enabled for Hosted Fields.
- You have API credentials for the Atlas Payments API.
- Your backend can create checkout sessions.
- Your frontend can load external JavaScript assets.
- You understand your organization's PCI DSS compliance requirements.

---

## Design Principles

Atlas Hosted Fields is built around four core principles.

### Secure by Default

Sensitive payment information is captured directly by Atlas and never exposed to your application.

### API-Driven

The Checkout Sessions API defines what is rendered, how it behaves, and which payment features are enabled.

### Developer-Friendly

Start with a minimal implementation, then progressively customize layouts, validation rules, field behavior, branding, and advanced payment features.

### Flexible

Atlas Hosted Fields supports both simple integrations and highly customized checkout experiences without changing the underlying security model.

---

## Next Steps

If you're integrating Atlas Hosted Fields for the first time, continue with the **Quickstart**.

The Quickstart walks through creating your first checkout session, rendering hosted payment fields, collecting payment information, and submitting a payment using the default configuration.
