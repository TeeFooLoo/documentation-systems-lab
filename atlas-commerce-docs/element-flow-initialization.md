# Session Initialization

The checkout process starts on your server by creating a secure checkout session. This setup step tells Atlas Commerce what payment options you want to show and prepares our system to safely handle your customer's card information.

You only need to complete this setup once per checkout session, right before the customer sees the payment form.

### Before You Start

Make sure your system has the following ready:

* **Account Credentials:** You have your `store_id` and `terminal_id` saved safely on your server.
* **Server Setup:** Your backend server is able to send requests to our API.
* **HTML Pages:** Your website's checkout page has empty placeholder boxes (like `<div id="card-element-container"></div>`) ready for us to drop the payment fields into.

## How the Setup Works

```
┌────────────────────────┐      ┌────────────────────────┐      ┌────────────────────────┐
│  1. Send Your Setup    │ ───► │   2. Secure the Session│ ───► │ 3. Get Your Launch Code│
├────────────────────────┤      ├────────────────────────┤      ├────────────────────────┤
│ Backend sends settings │      │ Atlas locks in rules   │      │ App gets a token and   │
│ and card form rules    │      │ and saves the session  │      │ a script to load forms │
└────────────────────────┘      └────────────────────────┘      └────────────────────────┘

```

Setting up a session follows three simple steps:

1. **Send Your Setup:** Your server sends a `POST` request to `/v1/checkout/sessions` telling us your preferences, language choice, and how you want the payment form to look.
2. **Secure the Session:** Our system reads your request, confirms the rules, and creates a unique, safe workspace for this specific purchase.
3. **Get Your Launch Code:** Our system sends back a temporary `clientToken` and a snippet of code (`renderScript`) that your website will use to display the secure payment boxes.

## Endpoint Reference

### Create a Checkout Session

`POST https://api.atlascommerce.com/v1/checkout/sessions`

#### Body Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `enabled` | boolean | **Yes** | Must be set to `true` to turn on the secure payment fields for this checkout. |
| `cultureCode` | string | No | The language setting (like `en-US`) used for automatic error and status messages. |
| `controls` | array | No | A list of settings that tells us which payment fields you want and where to place them on your page. |

### Request Example

This example shows a standard setup request. It turns on secure credit card fields, sets the language to English, and defines which card details to collect.

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

### Response Example

When your request succeeds, our system returns the temporary security codes and script text needed to show the payment form on your website.

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

## Adding the Script to Your Website

The response from your server includes a piece of code called `renderScript`. To keep the payment form safe and secure, **your frontend website code must drop this exact script text directly onto your web page without changing it**.

> ⚠️ **Important Security Rule:** Treat this script text as a finished piece. Never change the web address inside it, and do not remove the security tags (like `integrity` or `crossorigin`). If any parts are missing, web browsers will block the script from loading.

### Code Example

```javascript
// This is the response data sent from your backend server
const atlasResponse = {
  checkout_session: {
    hostedPaymentFields: {
      renderScript: '<script src="https://js.atlascommerce.com/v1/elements/atlas.render.js?clientToken=ct_9a8b7c6d5e4f3g2h1_sandbox" integrity="sha384-H4x7P..." crossorigin="anonymous" referrerpolicy="strict-origin-when-cross-origin"></script>'
    }
  }
};

// Create a placeholder block to hold the script
const scriptShell = document.createElement("div");
scriptShell.innerHTML = atlasResponse.checkout_session.hostedPaymentFields.renderScript;

// Add the script directly to your web page to load the secure fields immediately
document.body.appendChild(scriptShell.firstChild);

```

## Standard Settings (Fallbacks)

To help you get started quickly with minimal code, Atlas has standard built-in options. If you simply send `"enabled": true` and leave out the custom `controls` list, our system automatically builds a standard form for you.

| Settings Path | What Happened | How the System Responds |
| --- | --- | --- |
| `enabled` | Left out completely or set to `false` | The system stays turned off. No payment fields will appear on your page. |
| `controls` | The list is missing entirely | Renders a standard, clean credit card form automatically. |
| `container` (Credit Card) | List is active, but a container box name is missing | Looks for a box on your page named `#credit-container`. |
| `container` (Token View) | List is active, but a container box name is missing | Looks for a box on your page named `#token-container`. |
| `fieldGroupingType` | Setting is missing inside `fieldOptions` | Places every input field into its own separate box automatically. |

## Next Integration Steps

Move on to the following guides to finish setting up your payment page:

| Guide | Description |
| --- | --- |
| **[UI & Layout Controls](https://www.google.com/search?q=%23)** | Learn how to place your payment fields into grids and adjust form boxes on your page. |
| **[Field Styling Rules](https://www.google.com/search?q=%23)** | Add custom text hints, change field borders, and control how boxes look when clicked. |

#### Need help?

Reach out to our developer support team at `dx-support@atlascommerce.com` for assistance with your test environment.
