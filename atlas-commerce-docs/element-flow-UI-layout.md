## UI and Layout Controls

Atlas Commerce utilizes dynamic secure layout rendering to inject secure payment fields into a merchant's checkout experience based on the configuration provided during initialization.

The structure of the request dictates what components are rendered, where they appear in the Document Object Model (DOM), and how they behave.

### 1. Rendering Workflow

Rendering occurs sequentially after an initial backend payment request creates a secure context. This context stores the configuration that Atlas Commerce reads when the merchant frontend loads the core rendering script using a unique client session token.

```
Merchant Server             Customer            Merchant Frontend            Render Script            Hosted Fields             APIs
       |                       |                        |                          |                        |                 |
       |-----------------------|------------------------|--------------------------|------------------------|---------------->|
       |                       |                        |  POST /payment request   |                        |                 |
       |<----------------------|------------------------|--------------------------|------------------------|-----------------|
       | Return token & URL    |                        |                          |                        |                 |
       |-----------------------|----------------------->|                          |                        |                 |
       | Pass token to browser |                        |                          |                        |                 |
       |                       |                        |------------------------->|                        |                 |
       |                       |                        | Load script with token   |                        |                 |
       |                       |                        |                          |------------------------|---------------->|
       |                       |                        |                          | Request configuration  |                 |
       |                       |                        |                          |<-----------------------|-----------------|
       |                       |                        |                          | Return control layout  |                 |
       |                       |                        |<-------------------------|                        |                 |
       |                       |                        | Inject secure iframes    |                        |                 |
       |                       |                        |                          |                        |                 |
       |                       |                        |--------------------------|----------------------->|                 |
       |                       |                        | Customer enters data     |                        |                 |
       |                       |                        |                          |                        |---------------->|
       |                       |                        |                          |                        | Send encrypted  |
       |                       |                        |                          |                        | raw payment data|
       |                       |                        |                          |<-----------------------|                 |
       |                       |                        |<-------------------------| Emit field events      |                 |
       |                       |                        | Update checkout state    | (state, validation)    |                 |
       |                       |                        | (enable submit button)   |                        |                 |
       |<----------------------|------------------------|                          |                        |                 |
       | Submit checkout form  |                        |                          |                        |                 |
       |-----------------------|------------------------|--------------------------|------------------------|---------------->|
                                                           Final authorization request with session token

```

Each designated control element results in one or more hosted `iframe` elements. These iframes isolate and securely capture sensitive payment data, transmitting it directly to the card vault networks. The merchant application never has access to raw account details at any point in the flow.

---

### 2. Layout Controls

Controls define the specific UI components rendered in the checkout experience. Each entry within the configuration array represents an explicit layout component.

Supported component types include:

* **Credit Card Form:** Standard layout for capturing traditional account data.
* **Gift Card Form:** Layout optimized for localized card networks and gift options.
* **Token Update Form:** Minimized interfaces for updating existing payment credentials.
* **Platform Branding:** Secure rendering elements for compliance logos.

> ### ⚠️ Crucial Control Rules
> 
> 
> * **Optional Defaults:** Controls are optional when utilizing out-of-the-box rendering behavior. If omitted, a standard vertical stacked form is applied to pre-designated system containers.
> * **Required Conditions:** Layout controls are explicitly required when a merchant wants to customize which components appear, dictate precise placement via targeted container selectors, or alter field grouping logic.
> * **Container Mapping:** Each control maps to exactly one DOM container element.
> * **Type Constraints:** Only one payment card type may be rendered per view. Multiple controls can specify the same card type only if they are distributing distinct fields belonging to the same single form.
> 
> 

---

### 3. Rendering Modes

The application supports two distinct rendering options, configured via the `fieldGroupingType` property. This selection alters how fields are packaged inside iframes and dictates the structural flexibility available to your CSS layouts.

#### Individual Fields (`individual`)

* Each input field is assigned its own independent `iframe`.
* Offers maximum structural flexibility.
* Highly recommended for responsive web layouts and custom checkout themes.

#### Grouped Fields (`grouped`)

* All fields are rendered inside a single consolidated `iframe`.
* Simplifies implementation overhead.
* Limits layout positioning, forcing a standard rigid box flow within the container.

---

### 4. Implementation Layout Examples

#### Example A: Minimal Default Configuration (Stacked Vertical)

When no custom parameters or containers are passed, the core engine automatically targets a fallback selector and renders a vertically stacked layout.

```json
{
  "eCommerce": {
    "securePaymentFields": {
      "enabled": true
    }
  }
}

```

#### Example B: Custom Split Containers (Horizontal Flow)

To position payment fields side-by-side or across different rows, set `fieldGroupingType` to `"individual"` and declare explicit target DOM selectors via the `container` property.

**HTML Markup:**

```html
<div class="checkout-form-wrapper">
  <div id="atlas-card-number-row"></div>
  <div class="split-row">
    <div id="atlas-expiry-box"></div>
    <div id="atlas-cvv-box"></div>
  </div>
</div>

```

**JSON Request Configuration:**

```json
{
  "eCommerce": {
    "securePaymentFields": {
      "enabled": true,
      "controls": [
        {
          "cardType": "credit",
          "container": "#atlas-card-number-row",
          "fieldOptions": {
            "fieldGroupingType": "individual",
            "fields": [
              { "fieldType": "Pan" }
            ]
          }
        },
        {
          "cardType": "credit",
          "container": "#atlas-expiry-box",
          "fieldOptions": {
            "fieldGroupingType": "individual",
            "fields": [
              { "fieldType": "ExpDate" }
            ]
          }
        },
        {
          "cardType": "credit",
          "container": "#atlas-cvv-box",
          "fieldOptions": {
            "fieldGroupingType": "individual",
            "fields": [
              { "fieldType": "SecurityCode" }
            ]
          }
        }
      ]
    }
  }
}

```

---

### 5. Next Steps

Now that you are familiar with structural rendering behavior and container mapping, explore these dependent topics to finalize your setup:

1. **Payment Field Configuration:** See the **Fields Configuration Guide** to learn how to apply deep field properties, custom validation rules, placeholder text, and dynamic masking inputs.
2. **Client-Side Event Handlers:** Refer to the **Client Lifecycle Events Index** to wire listener hooks (`onPaymentReady`, state updates, validation changes) directly into your global web application state.
