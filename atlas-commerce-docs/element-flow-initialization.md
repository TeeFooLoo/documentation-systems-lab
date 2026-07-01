---
description: Learn how Atlas Hosted Elements are activated, configured,
  and rendered within your checkout experience.
title: Session Activation & Configuration
---

# Session Activation & Configuration

## Overview

Before Atlas can securely collect payment information, your application
must create a checkout session. During session activation, your backend
defines the checkout configuration, requests the Hosted Elements that
should be rendered, and receives the information required to securely
initialize the customer-facing payment experience.

This guide explains the complete activation lifecycle, how the request
JSON is organized, and how each configuration property influences the
behavior of Atlas Hosted Elements.

------------------------------------------------------------------------

# Core Integration Concepts

Understanding the relationship between Hosted Elements, Controls,
Containers, and Checkout Sessions makes the remaining documentation much
easier to follow.

## Hosted Elements

Hosted Elements are secure payment inputs rendered inside isolated
Atlas-hosted iframes. Examples include:

  Hosted Element           Purpose
  ------------------------ --------------------------
  `Pan`                    Card Number
  `ExpDate`                Combined Expiration Date
  `ExpMonth` / `ExpYear`   Split Expiration Fields
  `SecurityCode`           CVV / CVC
  `RoutingNumber`          Bank Routing Number
  `AccountNumber`          Bank Account Number

Because Hosted Elements execute inside secure iframes, raw payment data
never becomes accessible to your application.

## Controls

Controls are logical collections of Hosted Elements. Instead of
requesting each Hosted Element individually, merchants request a Control
such as `credit`, `giftcard`, or `token`.

## Containers

Containers are HTML placeholders that exist in your page before
initialization.

``` html
<div id="credit-container"></div>
```

Atlas injects Hosted Elements into these containers during activation.

## Element Options

Each Control may include an `elementOptions` object. This object
controls grouping, validation, masking, placeholders, labels, and other
rendering behavior.

------------------------------------------------------------------------

# Activation Workflow

Setting:

``` json
"elementFlow": {
  "enabled": true
}
```

activates Atlas Hosted Elements for the checkout session.

If omitted or set to `false`, Atlas performs payment processing without
rendering Hosted Elements.

## Request Breakdown

  -----------------------------------------------------------------------
  JSON Section                        Purpose
  ----------------------------------- -----------------------------------
  `eCommerce.elementFlow`             Enables Hosted Elements and defines
                                      checkout behavior.

  `controls[]`                        Specifies which Controls Atlas
                                      should render.

  `elementOptions`                    Customizes Hosted Element behavior.

  `credentials`                       Authenticates the merchant.
  -----------------------------------------------------------------------

## Lifecycle

1.  Your backend sends a Session Activation request.
2.  Atlas validates credentials and creates a Checkout Session.
3.  Atlas returns:
    -   `activationKey`
    -   `renderScript`
4.  Your frontend executes `renderScript`.
5.  Atlas renders Hosted Elements into the configured Containers.
6.  Customers enter payment information.
7.  Atlas emits lifecycle events such as `onPaymentReady`.

> **Important**
>
> Always execute the returned `renderScript` exactly as received. Do not
> construct the URL manually or extract the activation key yourself.

------------------------------------------------------------------------

# Configuration Reference

## Controls (`cardType`)

  Value        Description
  ------------ ------------------------------
  `credit`     Credit and debit card form
  `token`      Update stored payment method
  `giftcard`   Gift card entry
  `private`    Private-label cards
  `loyalty`    Loyalty identifiers

## Validation (`validationType`)

Determines whether a Hosted Element must contain valid data before
checkout may proceed.

Supported values:

-   `required`
-   `optional`
-   `optionalExplicit`

## Grouping (`elementGroupingType`)

  -----------------------------------------------------------------------
  Value                               Description
  ----------------------------------- -----------------------------------
  `individual`                        Each Hosted Element renders inside
                                      its own iframe. Used by the sample
                                      configuration.

  `grouped`                           Atlas groups multiple Hosted
                                      Elements into a single iframe.
  -----------------------------------------------------------------------

## Labels

`controlLabelPosition` controls where labels appear.

## Placeholders

`placeHolderType` controls placeholder text.

## Masking

`maskingType` controls when values are masked.

`maskingCharacter` controls which masking character is used.

## Validation Feedback

`validationMessageDisplayType` controls how validation errors are
displayed.

------------------------------------------------------------------------

# Merchant Credentials

  Property       Description
  -------------- ------------------------------------------
  `merchantId`   Merchant account identifier.
  `channelId`    Checkout channel or integration profile.

------------------------------------------------------------------------

# Walking Through the Sample Request

The activation request is organized into four logical layers.

``` text
eCommerce
└── elementFlow
    ├── enabled
    ├── controls[]
    │   ├── cardType
    │   ├── container
    │   └── elementOptions
    │       ├── elementGroupingType
    │       └── elements[]
    │           ├── elementType
    │           ├── validationType
    │           ├── maskingType
    │           ├── maskingCharacter
    │           ├── placeHolderType
    │           ├── controlLabelPosition
    │           └── validationMessageDisplayType
└── credentials
```

The sample configuration requests a **Credit Card Control** that renders
three Hosted Elements (`Pan`, `ExpDate`, and `SecurityCode`) inside
`#credit-container`.

Each Hosted Element:

-   is required,
-   displays outline labels,
-   provides inline validation,
-   uses appropriate placeholder behavior,
-   and applies masking where appropriate.

------------------------------------------------------------------------

# Rendering the Checkout

Create the required HTML container.

``` html
<div id="credit-container"></div>
```

Execute the returned `renderScript`.

Atlas locates every configured Container and injects the requested
Hosted Elements.

------------------------------------------------------------------------

# Real-Time Validation

Atlas validates Hosted Elements continuously while customers type.

When all required Hosted Elements contain valid values, Atlas emits
`onPaymentReady`.

A common implementation pattern is to disable the checkout button until
`onPaymentReady` is received.

------------------------------------------------------------------------

# Best Practices

> **Tip**
>
> Start with a single `credit` Control using default behavior before
> introducing custom layouts.

> **Note**
>
> Hosted Elements form a security boundary. Never attempt to inspect
> iframe contents or intercept sensitive payment values.

> **Warning**
>
> Activation keys are single-use. Always request a new Checkout Session
> after a failed or abandoned checkout.

------------------------------------------------------------------------

# Next Steps

-   Initialize a Checkout Session
-   Controls and Rendering
-   Hosted Element Properties
-   Client Events
-   3D Secure
-   Error Handling
