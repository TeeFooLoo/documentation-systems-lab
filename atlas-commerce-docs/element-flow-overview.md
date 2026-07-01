# Atlas Element Flow

Atlas Element Flow enables merchants to securely embed payment Elements directly into their checkout experience while the Atlas Platform captures, validates, and processes sensitive payment data.

Instead of collecting cardholder data within your own application, Atlas renders secure Elements inside isolated iframes. This architecture allows you to build fully customized checkout experiences while keeping raw payment data outside your environment, reducing PCI DSS scope without limiting design flexibility.

Whether you're building a simple payment page or a highly customized checkout, Atlas Element Flow provides a secure, API-driven framework for rendering payment Elements, validating customer input, performing optional authentication, and submitting transactions.

---

# Why Use Atlas Element Flow?

Atlas Element Flow separates payment data collection from payment processing.

Your application remains responsible for the customer experience, while the Atlas Platform securely manages payment data collection, validation, and security. This architecture provides a consistent integration model regardless of how your checkout is designed.

| Benefit | Description |
|----------|-------------|
| **Reduced PCI DSS Scope** | Cardholder data is captured directly by Atlas and never passes through merchant-controlled systems. |
| **Complete Checkout Control** | Build your own checkout using standard HTML, CSS, and JavaScript without sacrificing payment security. |
| **Flexible Rendering** | Render individual Elements wherever they best fit your checkout experience. |
| **API-Driven Configuration** | Configure Elements, validation, layouts, and optional services during activation. |
| **Built-In Security** | Supports 3D Secure (3DS), Strong Customer Authentication (SCA), fraud services, and secure tokenization. |

---

# Global Coverage & Channels

Atlas Element Flow is designed for merchants operating across multiple regions and customer experiences.

| Capability | Description |
|------------|-------------|
| **Supported Markets** | United States, United Kingdom, and European Union. |
| **Multi-Currency Support** | Supports localized payment experiences and international payment processing. |
| **Responsive Checkout** | Compatible with desktop, mobile web, tablet, and responsive checkout experiences. |

---

# Integration Workflow

Every checkout follows the same three-stage lifecycle.

```text
                Atlas Element Flow Lifecycle

┌────────────────────┐
│ 1. Activation      │
└─────────┬──────────┘
          │
          │ Merchant backend sends an activation request
          ▼
      Atlas Platform
          │
          │ Returns:
          │ • activationKey
          │ • renderScript
          ▼

┌────────────────────┐
│ 2. Render          │
└─────────┬──────────┘
          │
          │ Browser executes renderScript
          │
          ▼
     Secure Elements
     rendered into
     merchant containers

┌────────────────────┐
│ 3. Pay             │
└─────────┬──────────┘
          │
          │ Customer enters payment information
          │
          │ Atlas validates Elements
          │
          │ onPaymentReady
          ▼
     Merchant submits
     payment request
```

---

## Phase 1 — Activation

Your backend begins the checkout by sending an activation request to the Atlas Platform.

During activation, Atlas:

- Creates the activation context.
- Registers your requested Controls.
- Maps Elements to their Containers.
- Applies validation and rendering rules.
- Returns an `activationKey`.
- Returns the `renderScript` required by the frontend.

For implementation details, continue to **Session Activation & Configuration**.

---

## Phase 2 — Render

Your frontend executes the returned `renderScript`.

The Atlas Platform locates each configured Container and securely injects the requested Elements into your checkout page.

Because Elements execute inside isolated iframes:

- Cardholder data never enters your application.
- Atlas controls validation and formatting.
- Sensitive payment information remains isolated from merchant systems.

---

## Phase 3 — Pay

After the customer completes the checkout form:

1. Atlas validates each configured Element.
2. Optional services such as 3D Secure may execute.
3. Atlas emits the `onPaymentReady` event.
4. Your backend submits the payment request.
5. Atlas authorizes and processes the transaction.

This separation allows merchants to maintain complete control over the checkout experience while Atlas manages payment security.

---

# Documentation Guide

The Atlas Element Flow documentation follows the same lifecycle used during every integration, from activation through payment processing and advanced checkout capabilities.

| Guide | Description |
|-------|-------------|
| **Activation** | Create an activation request, configure Controls and Elements, receive an activation key, and initialize the checkout experience. |
| **Pay** | Learn how Atlas validates customer input, emits the `onPaymentDataCollected` event, and submits payment requests for authorization and processing. |
| **Optional Features** | Extend your checkout experience with localization, Dynamic Currency Conversion (DCC), fraud services, digital wallets, and other optional capabilities. |
| **3D Secure (3DS)** | Configure browser-based cardholder authentication, understand frictionless and challenge flows, and integrate 3DS into the payment lifecycle. |

---

# Security Model

Atlas Element Flow uses a secure iframe architecture to isolate payment data from merchant-controlled systems.

Key characteristics include:

- Payment information is captured only inside Atlas-hosted Elements.
- Raw cardholder data is never exposed to the merchant application.
- Validation occurs within the secure Element runtime.
- Communication between Elements and your application occurs through a controlled event model.
- Merchants receive only non-sensitive status information required to manage the checkout experience.

This architecture significantly reduces PCI DSS scope while maintaining a flexible, fully customizable checkout experience.

---

# Next Steps

If you're integrating Atlas Element Flow for the first time, continue with **Session Activation & Configuration**.

The next guide explains how to create an activation request, configure Controls and Elements, receive an activation key, and render secure payment Elements within your checkout page.
