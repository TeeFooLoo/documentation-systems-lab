# Session Activation & Configuration

## 1. Core Integration Concepts

Before setting up your code, it helps to understand how these three core pieces work together to orchestrate your checkout page:

* **Elements:** These are the individual, secure UI components (such as the Card Number element, Expiration Date element, or Security Code element) where a customer types their billing details. Instead of being hosted directly on your website, these elements are securely served from an isolated environment inside an invisible wrapper (`iframe`). This keeps raw card data completely off your servers.


* **Controls:** A control is a pre-configured *group* of elements bundled together for a specific workflow. For example, instead of forcing you to build an entire credit card form element-by-element, you simply request a `"credit"` control, and Atlas automatically packages and displays the necessary elements for you.


* **Containers:** A container is an empty structural placeholder (like a standard HTML `<div>`) that you place directly into your website's layout template. During activation, the secure runtime script locates this specific element on your page and injects your requested control—and its underlying elements—right into it.



```
[ Your Webpage Layout ]
└── <div id="credit-container">  <--- THE CONTAINER (Your empty placeholder)
    └── [ Secure Iframe ]       <--- THE CONTROL   (Injected bundle)
        ├── Card Number         <--- ELEMENT \
        ├── Expiration Date     <--- ELEMENT  |- Secure UI components
        └── Security Code       <--- ELEMENT /

```

## 2. The Activation Workflow

Session activation is the behind-the-scenes handshake that happens every time a customer reaches your checkout page.

Before showing any secure elements, your server talks to Atlas to register your layout choices, choose your security rules, and get a temporary, single-use security key. This ensures the checkout page environment is safely provisioned before the customer ever types a single digit.

### Architectural Lifecycle

The setup requires a quick three-step handoff between your server, the customer’s browser, and the payment system:

sequenceDiagram
    autonumber
    participant Server as Your Server
    participant Atlas as Atlas Engine
    participant Browser as Customer's Browser

    Server->>Atlas: POST /activation (Payload configurations & containers)
    Atlas-->>Server: Return renderScript & activationKey
    Server->>Browser: Inject script block into checkout page DOM
    Browser->>Atlas: Hydrate elements inside target containers


1. **The Request:** When your checkout page loads, your backend server sends a secure server-to-server `POST` request to Atlas. This payload defines what kind of control you want to activate and which placeholder elements on your page should hold them.


2. **The Key:** Atlas registers your layout request and sends back a response containing a unique, one-time security script link powered by a temporary `clientPaymentKey`.


3. **The Display:** Your website's frontend layout runs this script, which automatically targets your empty **containers** and hydrates the secure, isolated elements directly into them.


## 3. Configuration Profiles

### Control Types (`cardType`)

When setting up your session, you use the `controls` array to choose exactly what kind of payment experience to display to your customer. You map each control to its corresponding **container** element via its CSS selector.

| Form Type | What It Does | JSON Property Value|
| --- | --- | --- |
| **Credit Card Form** | Displays secure elements for standard credit and debit cards.| `"cardType": "credit"` |
| **Token Update Form** | Displays a specialized configuration to update or refresh a card already saved on file.| `"cardType": "token"` |
| **Gift Card Form** | Displays elements specifically formatted for gift card codes and balances.| `"cardType": "giftcard"` |
| **Private Label Form** | Displays custom entry elements for store-branded or closed-loop credit cards.| `"cardType": "private"` |
| **Loyalty Layout** | Displays an element for entering points cards, rewards profiles, or membership IDs.| `"cardType": "loyalty"` |

### Element Types (`fieldType`)

When customizing your control's internal components, you can select from these specific input types inside your `fields` array configuration:

* **`Pan` (Primary Account Number):** The core card number element. It automatically detects card brands (Visa, Mastercard, Amex, etc.) and runs mathematical validation checks. Supports masking properties (e.g., `maskOnBlur` using dots or asterisks to hide digits).

* **`ExpDate` (Expiration Date):** The expiration date element. It can be configured as a single combined element or separated into individual month (`ExpMonth`) and year (`ExpYear`) components. Accepts layout configurations to match your preferred placeholder style (`MM/YY` or `MM/YYYY` formats).

* **`SecurityCode` (Card Security Code):** The CVV/CVC security element. It automatically adjusts maximum character lengths (3 or 4 digits) based on the card brand detected in the `Pan` element.

* **`RoutingNumber` (ABA Routing Number):** Used for electronic check (ACH) or direct-debit processing forms. Enforces standard 9-digit checking rules.

* **`AccountNumber` (Bank Account Number):** The bank account entry element used alongside routing configurations for direct banking check paths.

## 4. Fine-Tuning Element Properties

Every element object inside your layout can include optional properties to fine-tune validation, design rules, and masking behaviors. If omitted, Atlas automatically applies optimized standard fallbacks.

### Grouping Style (`fieldGroupingType`)

Determines how elements are structurally packaged inside the container:

* **`individual`:** Each input element gets its own independent secure iframe wrapper. This provides absolute freedom for custom responsive CSS layouts and grid placements.


* **`grouped`:** Combines the elements together inside a single, pre-arranged iframe layout box to simplify implementation.

### Label & Placeholder Alignment

* **`controlLabelPosition`:** Choose where text labels reside. Options include `outlineLegend` (floating borders) or `inFieldTopAligned` (top-aligned inside the box bounds).


* **`placeHolderType`:** Sets background hint text. Options include `default` (standard format guides like `MM/YY`), `blank` (clean elements), or specialized formatting guides like `cvc` or `message`.



### Input Masking

* **`maskingType`:** Controls how digits are hidden from view. Use `maskOnBlur` to turn sensitive numbers into symbols the moment a customer clicks out of an element.


* **`maskingCharacter`:** Dictates the masking symbol. Choose between `dot` (●) or `asterisk` (*).



### Error Display Styles

* **`validationMessageDisplayType`:** Controls how immediate typing alerts are shown. Use `feedback` to output clear text alerts below the container, `tooltip` for popups, or `none` if you prefer to build your own custom error framework using client events.



---

## 5. Step-by-Step Implementation

### Step 1: Your Server Requests Activation

Your backend sends a JSON payload to request the activation session. This is where you declare your required controls, hook them to your placeholder container selectors, and style element rules.

```json
{
  "eCommerce": {
    "elementFlow": {
      "enabled": true,
      "controls": [
        {
          "cardType": "credit",
          "container": "#credit-container",
          "elementOptions": {
            "elementGroupingType": "individual",
            "elements": [
              {
                "elementType": "Pan",
                "validationType": "required",
                "maskingType": "maskOnBlur",
                "maskingCharacter": "dot",
                "placeHolderType": "message",
                "controlLabelPosition": "outlineLegend",
                "validationMessageDisplayType": "feedback"
              },
              {
                "elementType": "ExpDate",
                "validationType": "required",
                "placeHolderType": "default",
                "controlLabelPosition": "outlineLegend",
                "validationMessageDisplayType": "feedback"
              },
              {
                "elementType": "SecurityCode",
                "validationType": "required",
                "placeHolderType": "cvc",
                "controlLabelPosition": "outlineLegend",
                "validationMessageDisplayType": "feedback"
              }
            ]
          }
        }
      ]
    }
  },
  "credentials": {
    "merchantId": "MERCH_99203",
    "channelId": "CHN_88102"
  }
}
```

### Step 2: Atlas Responds with the Activation Script

When successful, your server receives a single-use script block from the engine containing your session's signature key:

```json
{
  "eCommerce": {
    "activationKey": "ack_9876543210_alpha_omega",
    "elementFlow": {
      "renderScript": "<script src=\"https://hpx.atlascommerce.com/scripts/v1/render.js?activationKey=ack_9876543210_alpha_omega\"></script>"
    }
  }
}

```

> ### ⚠️ Critical Integration Rules
> 
> 
> * **Do Not Alter the Script:** Treat the returned `renderScript` string as completely unchangeable. Do not try to extract pieces of it or pull out the key manually; injecting the raw script exactly as given is required for security integrity checks to pass.
> 
> 
> * **One-Time Use Only:** A session key is only good for a single checkout attempt. If a transaction fails or is declined by the bank, your server must request a brand-new activation payload before the customer tries again.
> 
> 
> 
> 

### Step 3: Your Frontend Displays the Elements

First, create an empty HTML **container** placeholder on your checkout page matching the exact CSS selector ID string you provided in Step 1:

```html
<form id="checkout-form">
  <!-- THE CONTAINER: Secure elements will be injected directly inside here -->
  <div id="credit-container"></div>

  <button type="submit" id="submit-btn" disabled>Pay Now</button>
</form>

```

Next, use JavaScript to take the `renderScript` string provided by your server and append it to your document body. This instantly runs the script, completes token provisioning, and loads the secure elements into your placeholder layout:

```javascript
function loadPaymentForm(apiResponse) {
  const scriptMarkup = apiResponse.eCommerce.hostedPaymentFields.renderScript;
  
  // Create a temporary staging element
  const wrapper = document.createElement("div");
  wrapper.innerHTML = scriptMarkup;
  
  // Append to document body to run the activation loader
  document.body.appendChild(wrapper.firstChild);
}

```

---

## 6. Real-Time Element Validation

The script continuously monitors typing inputs inside your containers in real time. It checks formatting rules, valid date configurations, and card-brand numbering formulas before allowing checkout forms to submit.

The runtime library broadcasts a green-light lifecycle event called `onPaymentReady` the exact moment all element parameters are completely and accurately filled out. It is a recommended practice to keep your checkout "Pay Now" button locked (`disabled`) until this event fires, ensuring customers don't submit empty or broken entries.
