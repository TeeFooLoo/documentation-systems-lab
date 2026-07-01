Here is the updated **Overview** page. The tech jargon has been dialed back into clear, simple language, and a new section has been added to highlight our supported markets (including the US, UK, and EU), channels, and built-in features like 3D Secure (3DS).

---

# Atlas Element Flow Overview

Atlas Element Flow lets you add secure payment boxes directly to your website's checkout page. While you retain complete control over how your checkout looks and feels, our secure platform handles all the sensitive card data in the background.

By keeping card data completely separate from your server, you can design a seamless shopping experience without having to handle or store risky credit card numbers yourself.

### Why Use Element Flow?

* **Easier Security Compliance:** Because sensitive card data never touches your website's servers, your annual security compliance process becomes incredibly simple.


* **Complete Design Freedom:** You are never locked into rigid, blocky forms. You can completely customize the layout, text, and design to match your brand using standard web styles.


* **Built-In Protection:** Advanced security rules, like 3D Secure (3DS) and Strong Customer Authentication (SCA), work automatically right inside the payment boxes to keep transactions safe.



## Global Coverage & Channels

Element Flow is built to support your business wherever you and your customers are located.

* **Supported Markets:** Fully optimized for businesses operating in the **United States**, the **United Kingdom (UK)**, and the **European Union (EU)**.
* **Multi-Currency:** Accept local payments smoothly with built-in tools that manage international card formatting and currency displays automatically.
* **Flexible Channels:** Built to power standard desktop checkout experiences, responsive mobile web layouts, and tablet-based shopping setups.

## How the Integration Works

The payment process moves through three simple phases.

```
┌────────────────────────┐      ┌────────────────────────┐      ┌────────────────────────┐
│      1. Setup          │ ───► │       2. Display       │ ───► │      3. Pay            │
├────────────────────────┤      ├────────────────────────┤      ├────────────────────────┤
│ Server creates session │      │ Code loads secure      │      │ Form safely swaps card │
│ and gets a token       │      │ boxes onto your page   │      │ data for a safe token  │
└────────────────────────┘      └────────────────────────┘      └────────────────────────┘

```

#### 1. Setup

Your backend server sends a request to Atlas (`POST /v1/checkout/sessions`) to start a new purchase session. You send along your basic preferences and layout rules, and Atlas sends back a temporary `clientToken`.

#### 2. Display

Your website loads our simple JavaScript library using that `clientToken`. The library finds the empty placeholder boxes on your checkout page and safely drops our secure payment inputs right into your layout.

#### 3. Pay

When your customer clicks the pay button, our script automatically packages the card data, saves it securely in our vault, and hands your website a safe, non-sensitive token text string. Your server then uses that safe token to complete the charge via `POST /v1/payments`.

## Dive into the Documentation

Explore our setup guides to build, style, and launch your payment experience.

| Guide | Description |
| --- | --- |
| **[Session Initialization](https://github.com/TeeFooLoo/documentation-systems-lab/blob/main/atlas-commerce-docs/element-flow-initialization.md)** | Learn how to request a temporary session token from your server and set your basic preferences.|
| **[UI & Layout Controls](https://www.google.com/search?q=%23)** | See how to arrange form fields, use custom grids, and map layouts on your page.|
| **[Client-Side Event Handling](https://www.google.com/search?q=%23)** | Find out how to catch input mistakes, show error messages, and track when fields are filled out.|
| **[Advanced Features & 3DS](https://www.google.com/search?q=%23)** | Turn on dynamic currency choices, change languages, and set up 3D Secure identity checks.|
| **[Error Framework Reference](https://www.google.com/search?q=%23)** | Troubleshoot declined payments, missing fields, or setup issues.|

#### Need help during integration?

* **Sandbox Testing:** Reach out to our developer support team at `dx-support@atlascommerce.com` for help while building.
* **Production Launch:** Contact our onboarding team at `go-live@atlascommerce.com` when you are ready to start taking real payments.
