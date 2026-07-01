Here is your fully scrubbed, highly polished Markdown (`.md`) documentation tailored for **Atlas Commerce**. All references to FreedomPay, HPX, and proprietary endpoints have been completely abstracted into a clean, modern Developer Experience (DX) format aligned with industry best practices.

---

# Atlas Commerce - Element Flow

## Introduction

**Atlas Element Flow** allows merchants to embed secure, PCI DSS-compliant payment fields directly into their checkout experience while the **Atlas Commerce Platform** handles all sensitive data within hosted iframes.

By separating payment data capture from backend transaction processing, merchants maintain full control over layout, styling, and the overall user experience without directly handling cardholder data. This approach combines design flexibility with robust security, making it ideal for both simple and highly customized checkout implementations.

---

## PCI DSS Scope Reduction

Atlas Element Flow is architected to drastically reduce a merchant's **PCI DSS (Payment Card Industry Data Security Standard)** scope by preventing sensitive cardholder data from ever entering merchant-controlled servers.

> 💡 **Security Principle:** The more your infrastructure interacts with raw cardholder data, the greater your compliance and security responsibilities.

```
┌────────────────────────┐      ┌────────────────────────┐      ┌────────────────────────┐
│    Merchant Browser    │      │ Merchant Server Core   │      │ Atlas Commerce Vault   │
├────────────────────────┤      ├────────────────────────┤      ├────────────────────────┤
│ Renders Hosted Iframes │ ───► │ Never touches raw card │ ───► │ Captures, encrypts,    │
│ inside checkout page   │      │ data directly          │      │ and processes payloads │
└────────────────────────┘      └────────────────────────┘      └────────────────────────┘

```

### In the Element Flow Architecture:

* **Isolated Capture:** Payment data is securely captured inside Atlas-hosted iframes operating in an isolated execution context separate from the merchant page.


* **Direct Transmission:** Sensitive components (such as Card Numbers and CVVs) travel directly from the customer's browser to the Atlas Commerce Vault.


* **Zero Residual Risk:** Merchant servers never receive, log, or store raw cardholder data.



Because the merchant never handles card data directly, they do not need to secure the systems that process it, typically qualifying them for lower-level compliance requirements such as **SAQ-A** or **SAQ-A-EP**.

---

## Integration Workflow

The Atlas Element Flow integration consists of three distinct phases: **Initialize**, **Render**, and **Authorize**.

```
 Customer              Merchant UI             Merchant Server          Atlas API
    │                       │                         │                     │
    │─── Initiate Checkout ─►                         │                     │
    │                       │─── Request Session ────►│                     │
    │                       │                         │─── POST /sessions ─►│
    │                       │                         │    (Define Config)  │
    │                       │                         │◄── clientToken ─────│
    │                       │◄── Return Session Data ─│                     │
    │                       │                                               │
    │                       │─── Load sdk.js ──────────────────────────────►│
    │                       │◄── Inject Iframes ────────────────────────────│
    │                       │                                               │
    │─── Input Card Data ───►                                               │
    │─── Click Submit ──────►                                               │
    │                       │─── tokenize() ───────────────────────────────►│
    │                       │◄── onTokenReady (Tokenized Value) ────────────│
    │                       │                                               │
    │                       │─── Submit Payload ─────►│                     │
    │                       │                         │─── POST /payments ──►│
    │                       │                         │    (With Token)     │
    │                       │                         │◄── Auth Result ─────│
    │                       │◄── Success/Failure ─────│                     │
    │◄── View Confirmation ─│                         │                     │

```

### 1. Initialize

The transaction lifecycle begins when the merchant server initiates a checkout session via a server-to-server request.

* **Endpoint:** `POST /v1/checkout/sessions`
* **Purpose:** The merchant server outlines the security controls, supported fields, styling configurations, and target container selectors.


* **Output:** Atlas returns a unique, short-lived `clientToken` and the URL for the client-side software development kit (`sdk.js`).



### 2. Render

Once the client frontend receives the `clientToken`, it initialises the user-facing secure element components.

* **Script Ingestion:** The frontend asynchronously pulls down the client-side SDK.


* **Container Targeted Mapping:** The script automatically matches the active configuration against the designated target container IDs present in the merchant's checkout HTML document.


* **Iframe Injection:** Secure input fields render dynamically inside the targeted containers, giving merchants layout dominance via traditional CSS styling wrappers.



### 3. Authorize

The completion phase executes a two-step capture and authorize handshake.

* **Tokenization Challenge:** When the customer clicks pay, the frontend invokes the SDK's tokenize execution command. Atlas securely transmits the payload, aggregates validation rules, and pushes back an `onTokenReady` client state payload.


* **Backend Submission:** The client passes this non-sensitive token back to the merchant server, which then fires a server-to-server transaction request (`POST /v1/payments`) containing the token to finalize authorization.



---

## Content Controls and Custom Layouts

Atlas Elements are injected directly into merchant-defined DOM containers, giving you flexible structural layouts.

### Rendering Layout Approaches

1. **Default Stack Layout:** Atlas automatically leverages standard predefined container IDs and stacks fields in an optimized, vertical presentation layout.


2. **Custom Layout Mapping:** The merchant explicitly defines custom HTML container element targets in the initial session generation contract, facilitating precise grid, multi-column, or responsive row wrappers.



> ⚠️ **Implementation Note:** To maintain payment ecosystem state validation, only one card type detection element control may be actively loaded per initialization lifecycle execution.
> 
> 

---

## Configuration & Client-Side Events

Merchants can programmatically tailor input behaviors, custom masks, dynamic formatting boundaries, and error display thresholds.

### Field Grouping Modes

* **Individual Elements (Standard):** Each separate target input component (Card Number, Expiration, CVV) renders inside an independent, isolated iframe block.


* **Combined Elements (Compact Formats):** Multiple field values compress down dynamically to render safely inside a singular iframe container block.



### Handling State Events

Atlas exposes non-sensitive client event hooks to track UI interactions. The most crucial event listener hook is **`onTokenReady`**:

```javascript
atlas.on('onTokenReady', (event) => {
  // Triggered when all components pass client validation rules
  // Exposes non-sensitive token data ready for server processing
  submitPaymentToServer(event.token);
});

```

---

## Advanced Capabilities

### 3D Secure (3DS)

Atlas Element Flow natively wraps layered **3D Secure Strong Customer Authentication (SCA)** workflows inside the hosted iframe environment. This includes background frictionless risk evaluations, dynamic issuer bank challenge modals, and verification directory sync handshakes.

### Error Handling Framework

The system splits exceptions cleanly across distinct lifecycle points:

* **Session Lifecycle Errors:** Triggered by schema validation failures or authentication errors during server initialization.


* **Rendering Lifecycle Errors:** Triggered by DOM target conflicts or missing UI containers.


* **Execution/Auth Errors:** Processing rejections, card validation drops, or upstream authorization failures.



---

## Next Steps

1. **Initialize a Session:** Head over to the [Atlas Session Initialisation Guide](https://www.google.com/search?q=%23) to build your first server-side payload contract and establish your secure transaction context.


2. **Review UI Components:** Dive into our [Controls & Layouts Explorer](https://www.google.com/search?q=%23) to view live examples of custom responsive CSS mappings for modern checkout funnels.



### Support & Integration Assistance

* **Sandbox & UAT Review:** Contact our integrations onboarding team at `dx-support@atlascommerce.com`.


* **Production Operations:** For merchant onboarding and activation help, reach out to `go-live@atlascommerce.com`.
