# Session Initialization

The transaction lifecycle begins on your backend server by initiating a secure checkout session. During initialization, Atlas Commerce establishes an isolated payment context that pre-configures and drives the frontend elements responsible for capturing sensitive cardholder data safely.

Initialization occurs exactly once per checkout session, prior to the customer interacting with any payment UI inputs.

### Prerequisites

Before invoking the initialization handshake, verify your backend integration satisfies the following constraints:

* **API Authentication:** Your platform credentials (`store_id`, `terminal_id`) are configured securely environment side.


* **Server Architecture:** Your system is prepared to handle upstream server to server HTTP POST actions.


* **DOM Structure:** Target container wrapper hooks (like `#card-element-container`) are present in your client checkout markup layout.



## The Session Handshake

```
┌────────────────────────┐      ┌────────────────────────┐      ┌────────────────────────┐
│  1. Request Session    │ ───► │ 2. Create Secure Vault │ ───► │  3. Return Client Hook │
├────────────────────────┤      ├────────────────────────┤      ├────────────────────────┤
│ Backend sends metadata │      │ Atlas locks state data │      │ App obtains token and  │
│ and component arrays   │      │ and structures routing │      │ dynamic render script  │
└────────────────────────┘      └────────────────────────┘      └────────────────────────┘

```

The initialization process moves through three structural steps:

1. **Request Session:** Your backend submits a `POST /v1/checkout/sessions` payload specifying configuration overrides, localized text preferences, and element configurations.


2. **Create Secure Vault:** The Atlas engine receives the payload schema contract, aggregates state rules, and provisions a unique backend transaction mapping context.


3. **Return Client Hook:** The response returns a short lived `clientToken` and a dynamic `<script>` snippet (`renderScript`) containing custom delivery metadata tailored specifically for that merchant checkout attempt.



Ah, looking closely at how the table is displaying, the markdown parser is choking because there are no blank lines around the table blocks. Markdown processors are notorious for failing to parse a grid if the table syntax is running right against headers or raw prose text.

Let's fix that by adding explicit block line breaks above and below the tables to force the markdown parser to compile them properly.

Here is the updated **Endpoint Reference** and **Default Configuration Fallbacks** text blocks with isolated formatting:

---
Looking at `image_094d7c.png`, I see exactly why it's still breaking. The issue isn't white space around the block—it's that the double leading pipes (`||`) are still corrupting rows 2 and 3. In Markdown table syntax, rows must start with a *single* vertical pipe (`|`), not a double pipe (`||`). When a markdown parser sees `||`, it completely drops out of table layout mode and drops back into raw text.

Let's clean that syntax constraint up completely. Here are the fully corrected raw blocks with single leading pipes.

---

Ah, GitHub Markdown! That explains it perfectly. GitHub uses its own specific flavor of Markdown (GFM), and its rendering engine can sometimes get incredibly stubborn if there is a tiny formatting mismatch or if it tries to render inside certain nested preview tools.

Since you are writing documentation directly for a GitHub repo (or a tool like GitHub Pages/Wiki), let's pivot away from standard lists and build a high-end layout using **HTML `<details>` blocks** and **bold description text**.

This gives a beautiful, clean presentation that is native to GitHub, completely bulletproof against rendering bugs, and reads like premium documentation.

---

## Endpoint Reference

### Create a Checkout Session

`POST [https://api.atlascommerce.com/v1/checkout/sessions](https://api.atlascommerce.com/v1/checkout/sessions)`

**Body Parameters**

**enabled** | *boolean* | **Required**




Must be set to `true` to provision secure Element infrastructure for the transaction lifecycle.

**cultureCode** | *string* | *Optional*




Standard locale formatting tag (like `en-US`) determining frontend message translation defaults.

**controls** | *array* | *Optional*




An array of element target configurations mapping specific UI form components into designated frontend HTML DOM wrapper tags.

---

## Default Configuration Fallbacks

To minimize initial boilerplate code overhead, Atlas supports comprehensive component fallbacks. If you submit the baseline contract declaration (`"enabled": true`) while completely omitting the `controls` property block, Atlas generates a standard stacked layout using default target container mappings.

**Initialization Trigger**




When `enabled` is completely omitted or declared `false`, the system remains totally uninitialized and no secure elements render.

**Controls Array Omission**




When the `controls` array block is omitted entirely, a standard, fully optimized credit card form interface renders.

**Credit Card Container Fallback**




If a credit control is provided without an explicit container property, it targets the default HTML selector string hook `#credit-container`.

**Token View Container Fallback**




If a token control is provided without an explicit container property, it targets the default HTML selector string hook `#token-container`.

**Field Grouping Default**




If the `fieldGroupingType` property is absent inside `fieldOptions`, the system defaults automatically to independent field context parsing (`"individual"`).

---

Because this relies on simple `<br>` and bold text instead of markdown grid structures, it will render exactly like this in your GitHub repository without breaking. Should we apply this same native style to the rest of the file layout?
---

## Default Configuration Fallbacks

To minimize initial boilerplate code overhead, Atlas supports comprehensive component fallbacks. If you submit the baseline contract declaration (`"enabled": true`) while completely omitting the `controls` property block, Atlas generates a standard stacked layout using default target container mappings.

| Override Path | Condition Trigger | Default Execution Behavior |
| --- | --- | --- |
| `enabled` | Completely omitted or declared `false` | System remains totally uninitialized. No secure elements render.

 |
| `controls` | Array block omitted entirely | Renders a standard, fully optimized credit card form interface.

 |
| `container` (Credit Card) | Array active, container property omitted | Targets default HTML selector string hook: `#credit-container`.

 |
| `container` (Token View) | Array active, container property omitted | Targets default HTML selector string hook: `#token-container`.

 |
| `fieldGroupingType` | Property absent inside `fieldOptions` | Defaults automatically to independent field context parsing (`"individual"`).

 |
---

### Request Payload Example

This request showcases a comprehensive initialization configuring localized parameters, currency tracking layers, and structured field parameters mapping target elements directly into single frame container controls.

```json
{
  "checkout_session": {
    "enabled": true,
    "cultureCode": "en-US",
    "dcc_conversion": {
      "enabled": true
    },
    "controls": [
      {
        "cardType": "credit",
        "container": "#credit-card-element",
        "fieldOptions": {
          "fieldGroupingType": "grouped",
          "fields": [
            {
              "fieldType": "Pan",
              "displayCardBrand": true,
              "validationType": "required",
              "maskingType": "maskOnBlur",
              "maskingCharacter": "dot"
            },
            {
              "fieldType": "ExpDate",
              "validationType": "required"
            },
            {
              "fieldType": "SecurityCode",
              "validationType": "required"
            }
          ]
        }
      }
    ]
  },
  "transaction_metadata": {
    "invoice_id": "inv_9831720",
    "purchase_totals": {
      "chargeAmount": 30.00
    },
    "transaction_type": "sale"
  },
  "credentials": {
    "store_id": "store_sandbox_01",
    "terminal_id": "term_sandbox_99"
  }
}

```

### Response Payload Example

Upon schema validation approval, the engine returns the session tokens and isolated execution markers required to handle rendering pipelines.

```json
{
  "checkout_session": {
    "clientToken": "ct_9a8b7c6d5e4f3g2h1_sandbox",
    "hostedPaymentFields": {
      "renderScript": "<script src=\"https://js.atlascommerce.com/v1/elements/atlas.render.js?clientToken=ct_9a8b7c6d5e4f3g2h1_sandbox\" integrity=\"sha384-H4x7P...\" crossorigin=\"anonymous\" referrerpolicy=\"strict-origin-when-cross-origin\"></script>"
    }
  }
}

```

## Frontend Script Injection

The response payload yields a literal `renderScript` `<script>` tag element string block. To ensure client side isolation boundaries match strict compliance parameters, **your frontend code must inject this raw string block directly into the Document Object Model (DOM) exactly as provided**.

> ⚠️ **Critical Security Constraint:** Treat the script tag payload as an opaque, unmodifiable asset. Never parse or peel out the `src` attribute string manually, and do not strip or alter the security tags (`integrity`, `crossorigin`, or `referrerpolicy`), as missing headers will drop browser script authorization.
> 
> 

### Implementation Example

```javascript
// Server response consumption hook
const atlasResponse = {
  checkout_session: {
    hostedPaymentFields: {
      renderScript: '<script src="https://js.atlascommerce.com/v1/elements/atlas.render.js?clientToken=ct_9a8b7c6d5e4f3g2h1_sandbox" integrity="sha384-H4x7P..." crossorigin="anonymous" referrerpolicy="strict-origin-when-cross-origin"></script>'
    }
  }
};

// Create an isolated injection shell element
const scriptShell = document.createElement("div");
scriptShell.innerHTML = atlasResponse.checkout_session.hostedPaymentFields.renderScript;

// Append directly to document body context to trigger immediate runtime initialization
document.body.appendChild(scriptShell.firstChild);

```

## Default Configuration Fallbacks

To minimize initial boilerplate code overhead, Atlas supports comprehensive component fallbacks. If you submit the baseline contract declaration (`"enabled": true`) while completely omitting the `controls` property block, Atlas generates a standard stacked layout using default target container mappings.

| Override Path | Condition Trigger | Default Execution Behavior |
| --- | --- | --- |
| `enabled` | Completely omitted or declared `false` | System remains totally uninitialized. No secure elements render.

 |
| `controls` | Array block omitted entirely | Renders a standard, fully optimized credit card form interface.

 |
| `container` (Credit Card) | Array active, container property omitted | Targets default HTML selector string hook: `#credit-container`.

 |
| `container` (Token View) | Array active, container property omitted | Targets default HTML selector string hook: `#token-container`.

 |
| `fieldGroupingType` | Property absent inside `fieldOptions` | Defaults automatically to independent field context parsing (`"individual"`).

 |

## Next Integration Steps

Explore subsequent guides to complete your UI implementation workflow:

| Guide | Description |
| --- | --- |
| **[UI & Layout Controls](https://www.google.com/search?q=%23)** | Map element configurations to advanced grids and learn layout container nesting overrides.

 |
| **[Field Styling Rules](https://www.google.com/search?q=%23)** | Inject dynamic placeholder styles, customize field borders, and control focus behavior states.

 |

#### Need help during integration?

Reach out to the Developer Experience engineering desk at `dx-support@atlascommerce.com` for help evaluating sandbox context responses.
