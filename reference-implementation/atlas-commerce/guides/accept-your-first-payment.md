# Accept Your First Payment

**Page Type:** Tutorial

Processing your first payment is the quickest way to understand how Atlas Commerce works.

In this tutorial, you'll build a complete end-to-end payment workflow using the Atlas Commerce Sandbox environment. You'll authenticate your application, submit a payment request, receive an authorization response, and understand what happens behind the scenes as your request moves through the platform.

Rather than introducing every payment feature at once, this guide focuses on the smallest successful payment transaction. Once you've completed this tutorial, you'll understand the overall payment lifecycle and be ready to explore more advanced capabilities such as refunds, stored payment methods, recurring billing, and webhooks.

By the end of this guide, you'll have successfully completed your first Atlas Commerce integration and gained a solid foundation for building production-ready payment workflows.

---

# Estimated Time

**15–20 minutes**

---

# Prerequisites

Before beginning this tutorial, ensure that you have completed the following steps:

- Completed the **Authentication** guide.
- Registered an Atlas Commerce Sandbox application.
- Obtained a valid OAuth access token.
- Installed a REST client such as curl, Postman, or Insomnia.
- Have a basic understanding of HTTP requests and JSON payloads.

If you have not yet authenticated your application, complete the **Authentication** guide before continuing.

---

# What You'll Build

In this tutorial you will build a complete payment authorization workflow.

By the end of this guide, you will have:

- Authenticated with Atlas Commerce.
- Submitted your first payment request.
- Received a successful authorization response.
- Learned how Atlas Commerce validates and processes payment requests.
- Understood the basic payment lifecycle.

Although this example uses a simple card payment, the same workflow forms the foundation for nearly every payment capability supported by Atlas Commerce.

---

# Before You Begin

It's helpful to understand what this tutorial does—and what it intentionally leaves out.

This guide demonstrates the simplest successful payment request. It does not include advanced capabilities such as:

- Saved payment methods
- Digital wallets
- 3-D Secure authentication
- Installment payments
- Partial captures
- Refunds
- Recurring billing

Those capabilities build upon the workflow introduced here and are covered in their own guides.

Keeping this first example intentionally simple allows you to understand the overall payment flow before introducing additional complexity.

---

# Payment Workflow Overview

Every payment follows the same high-level journey through the Atlas Commerce platform.

1. Your application submits a payment request.
2. Atlas Commerce authenticates the request.
3. The request is validated.
4. Atlas Commerce routes the authorization request to the appropriate payment network or processor.
5. The authorization response is normalized.
6. Atlas Commerce returns a consistent API response to your application.

The following diagram illustrates this workflow.

```mermaid
sequenceDiagram

participant Merchant as Your Application
participant Atlas as Atlas Commerce
participant Network as Payment Network
participant Issuer as Card Issuer

Merchant->>Atlas: Payment Request
Atlas->>Atlas: Authenticate Request
Atlas->>Atlas: Validate Payload
Atlas->>Network: Authorization Request
Network->>Issuer: Authorization
Issuer-->>Network: Approval
Network-->>Atlas: Authorization Response
Atlas-->>Merchant: Payment Response
```

Although several systems participate in processing a payment, Atlas Commerce provides a single, consistent API that abstracts these implementation details from your application.

As your integration grows, additional capabilities—such as tokenization, fraud screening, and digital wallets—extend this same workflow rather than replacing it.

---

# What You'll Learn

Throughout this tutorial, we'll explain not only **how** to submit a payment, but also **why** each step is necessary.

Specifically, you'll learn:

- Which fields are required to authorize a payment.
- How Atlas Commerce validates payment requests.
- How authorization responses are interpreted.
- What a successful payment response looks like.
- Common mistakes that occur during initial integrations.
- Where to go next after your first successful transaction.

Understanding these concepts will make future integrations significantly easier because every advanced payment workflow builds upon the same core transaction lifecycle introduced in this guide.

---

# Learning Journey

This tutorial is part of the Atlas Commerce Getting Started path.

```text
Authentication
      │
      ▼
Accept Your First Payment   ← You are here
      │
      ▼
Issue a Refund
      │
      ▼
Store Payment Methods
      │
      ▼
Receive Webhooks
      │
      ▼
Build Production Workflows
```

If you're following the recommended learning path, the next section walks through creating your first payment request using the Sandbox environment.

# Step 1: Create a Payment Request

With your application authenticated, you're ready to create your first payment.

A payment request tells Atlas Commerce what transaction you want to process. At a minimum, every payment request must answer four questions:

- **Who** is submitting the payment?
- **How much** should be charged?
- **What payment method** should be used?
- **What type of transaction** is being performed?

Atlas Commerce validates each of these elements before attempting to authorize the payment. If any required information is missing or invalid, the request is rejected before it reaches the payment processor.

The following sections introduce each part of the request before presenting the complete JSON example.

---

# Request Structure

A payment request is organized into several logical sections.

```text
Payment Request
│
├── Workflow
├── Credentials
├── Payment
│   ├── Merchant Information
│   ├── Transaction Details
│   ├── Amount
│   └── Payment Method
└── Metadata (optional)
```

Grouping related information makes requests easier to understand while allowing Atlas Commerce to validate each portion independently.

---

# Workflow Information

The workflow section identifies the request and provides information used for tracing and idempotency.

Example:

```json
{
  "workflow": {
    "requestId": "req_01J5RZKQ5T4Y8W7X9A2B3C4D5E"
  }
}
```

The `requestId` uniquely identifies this payment request.

Atlas Commerce uses this value for logging, troubleshooting, and preventing duplicate transaction processing.

> **Best Practice**
>
> Generate a new request identifier for every payment attempt. Reusing request identifiers can make troubleshooting more difficult and may interfere with duplicate request detection.

---

# Credentials

The credentials section identifies the merchant account processing the transaction.

Example:

```json
{
  "credentials": {
    "merchantId": "merchant_demo",
    "terminalId": "terminal_001"
  }
}
```

These values tell Atlas Commerce which merchant account should receive the authorization request.

In a production environment, these values are assigned during merchant onboarding and remain relatively stable.

---

# Payment Details

The payment section contains the business information required to process the transaction.

For a basic payment, this includes:

- Merchant reference
- Transaction type
- Amount
- Currency
- Payment method

Each field contributes to the authorization request ultimately sent to the payment processor.

---

# Merchant Reference

Every payment should include a merchant reference that uniquely identifies the transaction within your own system.

Example:

```json
{
  "merchantReference": "ORDER-100045"
}
```

Merchant references simplify reconciliation, reporting, customer support, and troubleshooting.

Choose values that remain meaningful to your business rather than relying on automatically generated identifiers.

---

# Transaction Type

Atlas Commerce supports several transaction types.

For this tutorial, you'll create a standard purchase.

```json
{
  "transactionType": "sale"
}
```

A **sale** both authorizes and captures funds in a single operation, making it the simplest payment workflow for a first integration.

More advanced transaction types—such as authorization-only, capture, refund, or void—are covered in separate guides.

---

# Amount

Every payment specifies both an amount and a currency.

Example:

```json
{
  "amount": {
    "currency": "USD",
    "value": 42.50
  }
}
```

Providing both values explicitly avoids ambiguity and allows Atlas Commerce to support multiple currencies using a consistent request model.

---

# Payment Method

Finally, provide the payment method.

For this tutorial, you'll use a test card.

```json
{
  "paymentMethod": {
    "cardNumber": "4111111111111111",
    "expirationMonth": "12",
    "expirationYear": "2030",
    "securityCode": "123"
  }
}
```

> **Sandbox Only**
>
> The card values shown throughout this guide are test data intended exclusively for the Atlas Commerce Sandbox environment. Never use production payment card information when developing or testing integrations.

---

# Complete Request

Now that you've seen each section individually, here's the complete payment request.

```json
{
  "workflow": {
    "requestId": "req_01J5RZKQ5T4Y8W7X9A2B3C4D5E"
  },
  "credentials": {
    "merchantId": "merchant_demo",
    "terminalId": "terminal_001"
  },
  "payment": {
    "merchantReference": "ORDER-100045",
    "transactionType": "sale",
    "amount": {
      "currency": "USD",
      "value": 42.50
    },
    "paymentMethod": {
      "cardNumber": "4111111111111111",
      "expirationMonth": "12",
      "expirationYear": "2030",
      "securityCode": "123"
    }
  }
}
```

Notice that the request is organized around logical business concepts rather than low-level implementation details.

As your integration grows, additional capabilities—such as tokenization, wallets, fraud screening, or recurring billing—extend this same request model instead of replacing it.

---

# Before Continuing

Before submitting the request, verify that:

- ✓ Your OAuth access token is still valid.
- ✓ The payment amount and currency are correct.
- ✓ The merchant reference is unique.
- ✓ You're using Sandbox credentials.
- ✓ You're using Sandbox test card data.

In the next step, you'll submit this request to the Atlas Commerce Payments API and examine the authorization response.

# Step 2: Submit the Payment

Your payment request is complete.

Now it's time to submit it to the Atlas Commerce Payments API.

When Atlas Commerce receives your request, it performs a series of validation and processing steps before returning a response. Understanding this workflow will help you troubleshoot issues later and better understand the payment lifecycle.

---

# Submit the Request

Send the payment request to the Payments endpoint using an authenticated HTTP POST request.

```http
POST https://api.sandbox.atlas-commerce.example/v1/payments
```

Include the following HTTP headers.

```http
Authorization: Bearer <access_token>
Content-Type: application/json
Accept: application/json
```

---

# Example cURL Request

The following example submits the payment request created in the previous step.

```bash
curl --request POST \
  --url https://api.sandbox.atlas-commerce.example/v1/payments \
  --header "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.example-token" \
  --header "Content-Type: application/json" \
  --header "Accept: application/json" \
  --data '{
    "workflow": {
      "requestId": "req_01J5RZKQ5T4Y8W7X9A2B3C4D5E"
    },
    "credentials": {
      "merchantId": "merchant_demo",
      "terminalId": "terminal_001"
    },
    "payment": {
      "merchantReference": "ORDER-100045",
      "transactionType": "sale",
      "amount": {
        "currency": "USD",
        "value": 42.50
      },
      "paymentMethod": {
        "cardNumber": "4111111111111111",
        "expirationMonth": "12",
        "expirationYear": "2030",
        "securityCode": "123"
      }
    }
}'
```

---

# What Happens Behind the Scenes?

Submitting a payment involves much more than sending JSON over HTTP.

Atlas Commerce coordinates several operations before returning a response.

```mermaid
sequenceDiagram

participant Merchant as Your Application
participant Atlas as Atlas Commerce
participant Processor as Payment Processor
participant Issuer as Card Issuer

Merchant->>Atlas: Payment Request

Atlas->>Atlas: Validate Access Token

Atlas->>Atlas: Validate Request

Atlas->>Processor: Authorization Request

Processor->>Issuer: Request Approval

Issuer-->>Processor: Approved

Processor-->>Atlas: Authorization Response

Atlas-->>Merchant: Payment Response
```

Although these processing steps occur automatically, understanding them provides valuable context when diagnosing payment issues or interpreting responses.

---

# Validation

Before Atlas Commerce contacts the payment processor, it validates your request.

Validation includes checks such as:

- Required fields are present.
- JSON structure is valid.
- The access token is valid.
- Required scopes are available.
- Merchant credentials are valid.
- Currency and amount are supported.
- The payment method is properly formatted.

If validation fails, the request is rejected immediately without contacting the payment processor.

This helps identify implementation issues quickly while avoiding unnecessary downstream processing.

---

# Successful Response

If the payment is successfully authorized, Atlas Commerce returns a response similar to the following.

```http
HTTP/1.1 201 Created
Content-Type: application/json
```

```json
{
  "paymentId": "pay_123456789",
  "merchantReference": "ORDER-100045",
  "status": "authorized",
  "amount": {
    "currency": "USD",
    "value": 42.50
  },
  "authorizationCode": "A34781",
  "created": "2026-06-23T15:42:18Z"
}
```

---

# Response Fields

| Field | Description |
|---------|-------------|
| `paymentId` | Atlas Commerce identifier for the payment. |
| `merchantReference` | Your original merchant reference. |
| `status` | Current payment status. |
| `amount` | Authorized payment amount. |
| `authorizationCode` | Approval code returned by the payment processor. |
| `created` | Time the payment was created. |

Most integrations store the `paymentId` because it is used by later operations such as captures, refunds, reporting, and customer support.

---

# What Just Happened?

Congratulations—you've completed your first payment.

Behind the scenes, Atlas Commerce has:

- Authenticated your application.
- Validated your request.
- Routed the authorization to the payment processor.
- Received the processor's response.
- Normalized that response into a consistent API model.
- Returned the result to your application.

Although payment processors differ significantly in how they communicate, Atlas Commerce presents a consistent API regardless of the underlying payment infrastructure.

This abstraction allows developers to build against a single API while Atlas Commerce manages processor-specific differences internally.

---

# Payment Status

The payment response includes a status describing the current state of the transaction.

For this tutorial, the expected status is:

```text
authorized
```

As you explore additional payment capabilities, you'll encounter other payment states.

```mermaid
stateDiagram-v2

[*] --> Created
Created --> Authorized
Authorized --> Captured
Captured --> Refunded
Authorized --> Declined
```

Not every payment follows every state, but understanding the lifecycle helps explain how refunds, captures, and reporting relate to one another.

---

# Verify Your Results

Before continuing, confirm that your payment completed successfully.

You should have:

- ✅ Received an HTTP 201 response.
- ✅ Received a payment identifier.
- ✅ Received an authorization code.
- ✅ Received a payment status of `authorized`.
- ✅ Confirmed that the merchant reference matches your request.

If all of these conditions are true, you've successfully processed your first payment using Atlas Commerce.

In the next section, you'll learn how to interpret payment responses, diagnose common failures, and understand what to do when a payment is declined or rejected.

# Step 3: Understand the Payment Response

Receiving a successful response is only the beginning of the payment workflow.

Your application must interpret the response correctly in order to determine what happened, what should happen next, and how the transaction should be recorded within your own systems.

Understanding the payment response is just as important as constructing the request.

---

# Payment Outcomes

Every payment submitted to Atlas Commerce results in one of three broad outcomes.

| Outcome | Meaning | Next Step |
|---------|---------|-----------|
| **Approved** | The issuer approved the transaction. | Continue your business workflow. |
| **Declined** | The issuer rejected the transaction. | Inform the customer and request another payment method if appropriate. |
| **Error** | Atlas Commerce could not process the request. | Correct the error and retry if appropriate. |

Understanding which category a response falls into simplifies error handling and creates a more predictable integration.

---

# Interpreting the Response

A successful authorization response contains more than an approval status.

Each field provides information that your application can use throughout the remainder of the payment lifecycle.

```json
{
  "paymentId": "pay_123456789",
  "merchantReference": "ORDER-100045",
  "status": "authorized",
  "amount": {
    "currency": "USD",
    "value": 42.50
  },
  "authorizationCode": "A34781",
  "created": "2026-06-23T15:42:18Z"
}
```

---

# paymentId

```text
pay_123456789
```

The `paymentId` uniquely identifies the payment within Atlas Commerce.

Your application should store this value because it is used by future operations such as:

- Captures
- Refunds
- Payment inquiries
- Reporting
- Customer support

Think of the payment identifier as the primary key for the remainder of the payment lifecycle.

---

# merchantReference

```text
ORDER-100045
```

The merchant reference is your identifier.

Atlas Commerce simply echoes the value supplied in the request.

Using meaningful merchant references makes reconciliation and customer support significantly easier.

---

# status

The payment status describes the current state of the transaction.

For this tutorial the expected value is:

```text
authorized
```

An authorized payment has been approved by the issuer.

Depending on your payment workflow, additional steps—such as capture or settlement—may still occur later.

---

# authorizationCode

```text
A34781
```

The authorization code is returned by the payment processor after a successful authorization.

Many merchants retain this value for customer service, reporting, and processor reconciliation.

---

# created

The timestamp records when Atlas Commerce created the payment.

Applications frequently use this value for:

- Reporting
- Auditing
- Transaction history
- Customer receipts

---

# Payment Lifecycle

A payment is not a single event.

It progresses through a lifecycle.

```mermaid
stateDiagram-v2

[*] --> Created

Created --> Authorized

Authorized --> Captured

Captured --> Settled

Settled --> Refunded

Authorized --> Voided

Authorized --> Declined
```

Not every payment visits every state.

For example:

- An authorization may be voided before settlement.
- A captured payment may later be refunded.
- A declined payment exits the workflow immediately.

Understanding this lifecycle makes later payment guides much easier to follow.

---

# Approved vs. Declined vs. Error

These outcomes are frequently confused.

They represent different types of events.

## Approved

The payment processor approved the transaction.

Business workflow can continue.

Example:

- Complete checkout
- Create the order
- Generate the receipt

---

## Declined

The request reached the payment processor, but the issuer declined the transaction.

This is **not** a technical failure.

Possible reasons include:

- Insufficient funds
- Expired card
- Card restrictions
- Issuer fraud checks

Most declined payments should prompt the customer to provide another payment method rather than automatically retrying the request.

---

## Error

Errors occur before or during payment processing.

Examples include:

- Invalid request payload
- Authentication failure
- Missing required fields
- Network interruption
- Unsupported currency

Unlike declines, many errors can be corrected and safely retried.

---

# Common Response Handling Mistakes

When implementing payment workflows, avoid these common mistakes.

- Assuming every HTTP 200 or 201 response means the payment was approved.
- Ignoring the payment status.
- Failing to store the `paymentId`.
- Treating declines as system errors.
- Automatically retrying declined transactions.
- Discarding the authorization code.

Proper response handling is just as important as constructing a valid request.

---

# What Should Your Application Do Next?

A successful authorization typically leads to one or more downstream business operations.

For example:

```text
Payment Authorized
        │
        ▼
Create Order
        │
        ▼
Reserve Inventory
        │
        ▼
Send Confirmation Email
        │
        ▼
Fulfill Order
```

The exact workflow depends on your business, but understanding where payment authorization fits within your application architecture is essential when designing reliable integrations.

---

# Verify Your Integration

At this point your integration should be able to:

- ✅ Authenticate successfully.
- ✅ Submit a payment request.
- ✅ Receive a successful authorization.
- ✅ Interpret the payment response.
- ✅ Store the payment identifier.
- ✅ Continue the appropriate business workflow.

Congratulations!

You've completed your first end-to-end payment using Atlas Commerce.

The following guides build upon the workflow introduced here to add additional capabilities such as refunds, payment methods, recurring billing, and webhook notifications.
