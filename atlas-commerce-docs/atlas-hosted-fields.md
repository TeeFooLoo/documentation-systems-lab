# Atlas Element Flow Overview

Atlas Element Flow allows merchants to embed secure, PCI DSS-compliant payment fields directly into their checkout experience while the Atlas Commerce Platform handles all sensitive data within hosted iframes.

By separating payment data capture from backend transaction processing, merchants maintain full control over layout, styling, and the overall user experience without directly handling cardholder data.

### Why Element Flow?

* **SAQ-A Eligibility:** Drastically reduce your PCI DSS compliance scope. Because card data never touches your infrastructure, you qualify for simplified compliance verification.


* **Design Freedom:** Avoid the rigid, blocky forms of traditional gateways. Control layout and styling seamlessly via frontend containers and custom CSS.


* **Native Authentication:** Advanced capabilities like 3D Secure Strong Customer Authentication (SCA) are handled inline within the secure boundary.



### Core Integration Lifecycle

The Element Flow architecture splits the transaction lifecycle into three distinct, secure phases.

```
┌────────────────────────┐      ┌────────────────────────┐      ┌────────────────────────┐
│     1. Initialize      │ ───► │       2. Render        │ ───► │      3. Authorize      │
├────────────────────────┤      ├────────────────────────┤      ├────────────────────────┤
│ Server creates context │      │ SDK injects iframes    │      │ Client tokenizes data; │
│ & gets clientToken     │      │ into custom containers │      │ server executes charge │
└────────────────────────┘      └────────────────────────┘      └────────────────────────┘

```

#### 1. Initialize

Your backend server triggers a checkout session with a server-to-server request (`POST /v1/checkout/sessions`). This payload outlines the security rules, payment fields required, and container specifications. Atlas returns a short-lived `clientToken`.

#### 2. Render

Your frontend client initializes our asynchronous JavaScript SDK (`sdk.js`) using the `clientToken`. The SDK identifies your target HTML container IDs and securely injects custom input iframes directly into your layout.

#### 3. Authorize

When the user clicks submit, your frontend triggers the SDK's `tokenize()` function. The client-side event framework captures validation states, securely passes encrypted data to the Atlas vault, and returns an `onTokenReady` event containing a non-sensitive token string. Your backend server completes the payment via `POST /v1/payments`.

### Dive into the Documentation

Explore the individual implementation guides to build, style, and launch your checkout integration.

| Guide | Description |
| --- | --- |
| **[Session Initialization](https://www.google.com/search?q=%23)** | Learn how to request an upstream checkout token from your server and customize the session scope.

 |
| **[UI & Layout Controls](https://www.google.com/search?q=%23)** | Explore responsive layout approaches, grid mapping, and styling your secure fields.

 |
| **[Client-Side Event Handling](https://www.google.com/search?q=%23)** | Hook into non-sensitive state updates, manage custom masks, and capture the validation loop.

 |
| **[Advanced Features & 3DS](https://www.google.com/search?q=%23)** | Implement dynamic currency features, localized language engines, and native 3D Secure challenge workflows.

 |
| **[Error Framework Reference](https://www.google.com/search?q=%23)** | Debug schema exceptions, layout mapping drops, and backend processing rejections.

 |

#### Need help during integration?

* **Sandbox Testing:** Connect with our technical onboarding desk at `dx-support@atlascommerce.com`.


* **Production Launch:** Contact the operations team at `go-live@atlascommerce.com` to review your go-live status.
