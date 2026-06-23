# Authentication

**Page Type:** Capability

Authentication is the first step in every Atlas Commerce integration.

Before your application can create payments, retrieve customer information, issue refunds, or receive webhook events, it must prove its identity to the Atlas Commerce platform. Authentication establishes trust between your application and Atlas Commerce, allowing the platform to determine who is making a request and what that application is permitted to do.

Atlas Commerce uses the industry-standard **OAuth 2.1 Client Credentials** authentication flow for server-to-server integrations. Your application exchanges a client ID and client secret for a short-lived access token, which is then included with each API request using the HTTP `Authorization` header.

This authentication model is designed to provide strong security while remaining straightforward to implement. By issuing short-lived access tokens rather than transmitting long-lived credentials with every request, Atlas Commerce minimizes credential exposure and simplifies credential rotation.

This guide explains how authentication works, how to obtain credentials, how to request access tokens, and how to securely authenticate every request to the platform.

---

# Estimated Time

**10–15 minutes**

---

# Prerequisites

Before beginning this guide, you should have:

- An Atlas Commerce developer account.
- A registered Atlas Commerce application.
- Sandbox API credentials.
- A REST client such as curl, Postman, or Insomnia.
- A basic understanding of HTTP and JSON.

No prior knowledge of Atlas Commerce is required.

---

# When to Use This Guide

Read this guide if you are:

- Building your first Atlas Commerce integration.
- Learning how Atlas Commerce authenticates API requests.
- Configuring a new application.
- Troubleshooting authentication failures.
- Rotating application credentials.

If you're looking for endpoint-specific authentication requirements, refer to the API Reference documentation.

---

# Learning Objectives

After completing this guide, you will be able to:

- Understand the Atlas Commerce authentication model.
- Distinguish between client credentials and access tokens.
- Configure sandbox and production credentials.
- Request an OAuth access token.
- Authenticate API requests using bearer tokens.
- Understand token expiration and lifecycle.
- Diagnose common authentication failures.
- Apply recommended security practices for protecting credentials.

---

# Authentication at a Glance

Atlas Commerce uses the following authentication model for machine-to-machine API integrations.

| Component | Purpose |
|-----------|---------|
| Client ID | Identifies your registered application. |
| Client Secret | Authenticates your application when requesting an access token. |
| Access Token | Short-lived bearer token used to authenticate API requests. |
| OAuth 2.1 Client Credentials Flow | Exchanges client credentials for an access token. |
| Scopes | Define which APIs your application may access. |

Client credentials are used only when requesting an access token. Once an access token has been obtained, it becomes the credential used for all subsequent API requests until it expires.

Applications should never transmit client secrets when calling business APIs such as Payments, Customers, or Refunds.

---

# Why OAuth?

Atlas Commerce uses OAuth because it separates long-lived application credentials from day-to-day API traffic.

Instead of sending a client secret with every request, your application authenticates once with the authorization server to obtain a temporary access token. That token is then presented when calling Atlas Commerce APIs.

This approach provides several advantages:

- Client secrets remain protected and are transmitted only to the authorization server.
- Access tokens have limited lifetimes, reducing the impact of accidental exposure.
- Permissions can be managed through scopes without changing application credentials.
- Credentials can be rotated without requiring changes to application logic.
- Authentication follows widely adopted industry standards, making integration familiar to developers already working with modern APIs.

For most integrations, authentication becomes a small part of the overall implementation. Your application periodically requests a new access token and then reuses that token until it expires.

---

# Choosing the Right Authentication Flow

OAuth defines several authentication flows, each designed for different types of applications.

Atlas Commerce uses the **OAuth 2.1 Client Credentials** flow because it is designed specifically for trusted server-to-server communication. In this model, your backend application authenticates directly with the Atlas Commerce authorization server using credentials issued during application registration.

This approach is appropriate for services that securely store application credentials and communicate with Atlas Commerce without direct user interaction.

Examples include:

- Backend web applications
- Payment gateways
- Server-side integrations
- Enterprise middleware
- Scheduled jobs
- Internal business services

Client Credentials is **not** appropriate for applications that execute entirely on a user's device, such as browser-based JavaScript applications, native mobile applications, or desktop clients.

These application types cannot adequately protect a Client Secret because users can potentially inspect the application or its network traffic. Instead, applications that authenticate individual users should use an interactive OAuth flow, such as the Authorization Code flow with Proof Key for Code Exchange (PKCE).

Atlas Commerce intentionally separates application authentication from user authentication.

Applications authenticate using the Client Credentials flow to obtain access tokens for server-to-server communication. User identity, if required by a business workflow, is handled independently of the application's authentication to the platform.

This distinction simplifies integrations while ensuring that long-lived application credentials remain protected within trusted server environments.

---

# How Authentication Works

At a high level, authentication follows a simple four-step process.

1. Register your application and receive a Client ID and Client Secret.
2. Exchange those credentials for an OAuth access token.
3. Include the access token in the `Authorization` header of each API request.
4. Request a new access token when the existing token expires.

The following sequence diagram illustrates the complete authentication flow.

```mermaid
sequenceDiagram
    participant App as Your Application
    participant Auth as Atlas Authorization Server
    participant API as Atlas Commerce API

    App->>Auth: Request access token<br/>(Client ID + Client Secret)
    Auth-->>App: Bearer access token

    App->>API: API request<br/>Authorization: Bearer <access_token>
    API-->>App: Requested resource

    Note over App: Reuse the access token until it expires

    App->>Auth: Request new access token
    Auth-->>App: New bearer access token
```

Notice that the Client Secret is never sent to the business APIs.

Only the authorization server receives your application credentials.

All Atlas Commerce APIs—including Payments, Customers, Orders, Refunds, and Reporting—accept only bearer access tokens.

---

# Authentication Architecture

Atlas Commerce separates authentication from business operations.

```text
                     Client Credentials
                (Client ID + Client Secret)
                           │
                           ▼
                Atlas Authorization Server
                           │
                   Issues Access Token
                           │
                           ▼
                    Bearer Access Token
                           │
        ┌──────────────────┼──────────────────┐
        ▼                  ▼                  ▼
   Payments API      Customers API      Refunds API
```

The authorization server is responsible only for verifying application identity and issuing access tokens.

Individual APIs are responsible only for validating bearer tokens and determining whether the requested operation is permitted.

This separation simplifies security, improves scalability, and ensures that every Atlas Commerce service uses a consistent authentication model.

---

# Sandbox and Production

Atlas Commerce maintains separate environments for development and production.

Each environment has its own credentials, authorization server, and API endpoints.

| Environment | Purpose |
|------------|---------|
| Sandbox | Build and test integrations without processing live transactions. |
| Production | Process live customer transactions. |

Sandbox credentials cannot be used against production APIs, and production credentials cannot be used within the production environment.

This separation helps prevent accidental use of live systems during development while allowing integrations to be fully validated before deployment.

Throughout this guide, all examples use sandbox credentials and sandbox endpoints.

# Register Your Application

Before your application can authenticate with Atlas Commerce, it must be registered with the platform.

Application registration establishes a trusted identity for your integration and generates the credentials required to request OAuth access tokens.

Each registered application receives:

- A Client ID
- A Client Secret
- One or more authorized scopes
- Environment-specific credentials
- Access to the Atlas Commerce Developer Portal

Applications should use separate credentials for each environment. Sandbox credentials are intended exclusively for development and testing, while production credentials are reserved for live transactions.

> **Best Practice**
>
> Register separate applications for development, staging, and production environments. Using independent credentials simplifies troubleshooting, supports credential rotation, and reduces the risk of accidental production access during development.

---

# Application Credentials

Atlas Commerce issues two credentials that uniquely identify your application.

| Credential | Purpose | Secret? |
|------------|---------|:-------:|
| **Client ID** | Identifies your application to the authorization server. | No |
| **Client Secret** | Proves the identity of your application when requesting access tokens. | Yes |

The Client Secret should be treated with the same care as any production password.

Never expose it to end users, include it in browser applications, or commit it to source control.

Instead, store Client Secrets in a secure secrets management solution such as:

- AWS Secrets Manager
- Azure Key Vault
- Google Secret Manager
- HashiCorp Vault
- Encrypted environment variables

Atlas Commerce never requires your Client Secret after an access token has been issued.

---

# Development and Production Environments

Atlas Commerce maintains completely separate environments for testing and production workloads.

Each environment has its own:

- Authorization server
- API endpoints
- Client credentials
- Access tokens
- Data

Credentials issued for one environment are never valid in another.

| Environment | Authorization Server | API Base URL |
|-------------|----------------------|--------------|
| Sandbox | `https://auth.sandbox.atlas-commerce.example` | `https://api.sandbox.atlas-commerce.example` |
| Production | `https://auth.atlas-commerce.example` | `https://api.atlas-commerce.example` |

Using isolated environments allows integrations to be fully validated before processing live customer transactions.

---

# Request an Access Token

Atlas Commerce uses the OAuth 2.1 Client Credentials flow.

Your application authenticates with the authorization server by exchanging its Client ID and Client Secret for a short-lived bearer token.

That access token is then used to authenticate every subsequent API request.

The Client Secret is used only during this exchange.

---

## Token Endpoint

```http
POST https://auth.sandbox.atlas-commerce.example/oauth/token
```

---

## Request Headers

```http
Content-Type: application/json
Accept: application/json
```

---

## Example Request

```json
{
  "clientId": "atlas_demo_application",
  "clientSecret": "atlas_demo_secret",
  "grantType": "client_credentials",
  "scope": "payments:read payments:write customers:read"
}
```

---

## Request Fields

| Field | Required | Description |
|--------|:--------:|-------------|
| `clientId` | ✓ | The Client ID assigned during application registration. |
| `clientSecret` | ✓ | The Client Secret associated with your application. |
| `grantType` | ✓ | OAuth grant type. Atlas Commerce currently supports `client_credentials`. |
| `scope` | Optional | Space-delimited list of requested scopes. If omitted, Atlas Commerce issues the application's default scopes. |

---

## Example cURL Request

```bash
curl --request POST \
  --url https://auth.sandbox.atlas-commerce.example/oauth/token \
  --header "Content-Type: application/json" \
  --header "Accept: application/json" \
  --data '{
    "clientId":"atlas_demo_application",
    "clientSecret":"atlas_demo_secret",
    "grantType":"client_credentials",
    "scope":"payments:read payments:write customers:read"
}'
```

---

# Successful Response

If authentication succeeds, Atlas Commerce returns an OAuth bearer token.

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.example-token",
  "tokenType": "Bearer",
  "expiresIn": 3600,
  "scope": "payments:read payments:write customers:read"
}
```

---

## Response Fields

| Field | Description |
|--------|-------------|
| `accessToken` | OAuth bearer token used to authenticate subsequent API requests. |
| `tokenType` | Authentication scheme. Always `Bearer`. |
| `expiresIn` | Lifetime of the access token, in seconds. |
| `scope` | Scopes granted to the issued access token. |

Applications should cache the access token until it expires rather than requesting a new token for every API request.

---

# Understanding Access Tokens

Access tokens are temporary credentials that represent your application's authenticated identity.

Unlike Client Secrets, access tokens are designed to be transmitted with API requests. Their limited lifetime significantly reduces the security impact of accidental exposure.

Atlas Commerce bearer tokens are:

- Short-lived
- Environment-specific
- Scope-limited
- Cryptographically signed
- Valid only for the issuing application

Applications should never modify, decode, or attempt to generate access tokens themselves.

---

# Token Lifecycle

Authentication is not a one-time event.

Applications should expect access tokens to expire as part of normal operation.

The recommended lifecycle is:

```text
Request Access Token
          │
          ▼
Cache Token
          │
          ▼
Call APIs
          │
          ▼
Token Expires
          │
          ▼
Request New Token
```

Applications should not request a new token before every API call.

Instead, cache the issued token until it expires, then request a replacement.

This approach minimizes network traffic, improves performance, and reduces unnecessary load on the authorization server.

---

# Understanding Scopes

Scopes determine what your application is allowed to do after authentication succeeds.

Authentication answers the question:

> **Who are you?**

Scopes answer the question:

> **What are you allowed to do?**

Typical Atlas Commerce scopes include:

| Scope | Description |
|---------|-------------|
| `payments:read` | Retrieve payment information. |
| `payments:write` | Create or modify payment transactions. |
| `customers:read` | Retrieve customer information. |
| `customers:write` | Create and update customer records. |
| `refunds:write` | Issue refunds. |
| `webhooks:read` | Retrieve webhook configuration. |

Atlas Commerce follows the principle of least privilege.

Applications should request only the permissions necessary to perform their intended functions.

Doing so limits the impact of compromised credentials while simplifying security reviews and operational governance.

---

# Authentication Checklist

Before continuing, verify that you have completed each of the following steps.

- ✓ Registered an application
- ✓ Obtained a Client ID
- ✓ Stored the Client Secret securely
- ✓ Requested an access token
- ✓ Received a successful OAuth response
- ✓ Cached the access token
- ✓ Confirmed the granted scopes

Once these steps are complete, you're ready to authenticate your first Atlas Commerce API request.

Continue to **Make an Authenticated Request** to learn how bearer tokens are used with Atlas Commerce APIs.

# Make an Authenticated Request

Your application is now ready to communicate with Atlas Commerce.

Once you've obtained an access token, every request to a protected Atlas Commerce API must include that token in the HTTP `Authorization` header using the Bearer authentication scheme.

Before processing the request, Atlas Commerce validates the access token to ensure that:

- The token was issued by the Atlas Commerce authorization server.
- The token has not expired.
- The token was issued for the current environment.
- The token contains the required scopes.
- The token has not been revoked.

Only after these checks succeed does the requested API operation begin.

---

# Authorization Header

Every authenticated request includes an HTTP `Authorization` header.

```http
Authorization: Bearer <access_token>
```

For example:

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.example-token
```

The `Bearer` authentication scheme is defined by OAuth and must be included exactly as shown.

Do not include your Client Secret when calling Atlas Commerce business APIs.

---

# Authentication Request Flow

The following diagram illustrates how Atlas Commerce authenticates a protected API request.

```mermaid
sequenceDiagram
    participant App as Your Application
    participant API as Payments API
    participant Auth as Token Validation

    App->>API: GET /payments/{paymentId}<br/>Authorization: Bearer token

    API->>Auth: Validate access token

    Auth-->>API: Token valid<br/>Scopes confirmed

    API-->>App: HTTP 200 OK
```

Notice that the authorization server is **not** contacted during every API request.

Instead, Atlas Commerce validates the bearer token locally before executing the requested operation.

This design minimizes latency while maintaining a consistent security model across every API.

---

# Example Request

The following request retrieves a payment using a bearer access token.

```bash
curl --request GET \
  --url https://api.sandbox.atlas-commerce.example/v1/payments/pay_123456789 \
  --header "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.example-token" \
  --header "Accept: application/json"
```

This request demonstrates the minimum authentication required to call a protected Atlas Commerce API.

---

# Successful Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "paymentId": "pay_123456789",
  "status": "authorized",
  "amount": {
    "currency": "USD",
    "value": 42.50
  },
  "created": "2026-06-23T14:05:17Z"
}
```

Receiving a successful response confirms that:

- Authentication succeeded.
- The access token is valid.
- The token contains the required permissions.
- The requested resource exists.
- Your application is communicating successfully with Atlas Commerce.

At this point your integration has completed its first authenticated API request.

---

# Verifying Authentication

Authentication succeeds before business logic executes.

The HTTP status code returned by Atlas Commerce helps determine where a request failed.

| Status | Meaning |
|---------|----------|
| **200 OK** | Authentication succeeded and the request completed successfully. |
| **201 Created** | Authentication succeeded and a resource was created. |
| **401 Unauthorized** | Authentication failed. |
| **403 Forbidden** | Authentication succeeded, but the application lacks the required permissions. |

Understanding this distinction simplifies troubleshooting because it separates authentication problems from authorization problems.

---

# Authentication vs. Authorization

Although the terms are often used interchangeably, authentication and authorization represent different stages of request processing.

Authentication answers the question:

> **Who is making this request?**

Authorization answers the question:

> **Is this application allowed to perform this operation?**

For example:

| Scenario | Authentication | Authorization |
|----------|---------------|---------------|
| Valid token with required scope | ✓ | ✓ |
| Expired token | ✗ | — |
| Invalid token | ✗ | — |
| Missing `payments:write` scope | ✓ | ✗ |

Understanding the distinction makes API errors significantly easier to diagnose.

---

# Common Authentication Errors

Authentication failures occur before Atlas Commerce executes business logic.

## Missing Authorization Header

```http
HTTP/1.1 401 Unauthorized
```

```json
{
  "error": {
    "code": "missing_authorization",
    "message": "Authorization header is required."
  }
}
```

Verify that the request includes an `Authorization` header using the Bearer authentication scheme.

---

## Expired Access Token

```http
HTTP/1.1 401 Unauthorized
```

```json
{
  "error": {
    "code": "token_expired",
    "message": "The supplied access token has expired."
  }
}
```

Request a new access token and retry the request.

---

## Invalid Access Token

```http
HTTP/1.1 401 Unauthorized
```

```json
{
  "error": {
    "code": "invalid_token",
    "message": "The supplied access token is invalid."
  }
}
```

Confirm that:

- The token was issued by Atlas Commerce.
- The token belongs to the current environment.
- The token has not expired.
- The Authorization header includes the `Bearer` prefix.

---

## Insufficient Scope

```http
HTTP/1.1 403 Forbidden
```

```json
{
  "error": {
    "code": "insufficient_scope",
    "message": "The supplied token does not include the required scope: payments:write."
  }
}
```

Authentication succeeded, but the application is not authorized to perform the requested operation.

Review the application's assigned scopes before retrying the request.

---

# Common Mistakes

The following mistakes account for most authentication-related support requests.

- Using Sandbox credentials against Production APIs.
- Using Production credentials against Sandbox APIs.
- Forgetting the `Bearer` prefix.
- Requesting a new access token before every API request.
- Including the Client Secret in business API requests.
- Embedding Client Secrets in browser or mobile applications.
- Assuming authentication and authorization are the same process.

Avoiding these common mistakes significantly reduces integration issues during development.

---

# Troubleshooting Authentication

If authentication fails, work through the following checklist.

| Problem | Possible Cause | Recommended Action |
|----------|----------------|--------------------|
| 401 Unauthorized | Missing Authorization header | Verify the Bearer token is included. |
| 401 Unauthorized | Expired token | Request a new access token. |
| 401 Unauthorized | Invalid token | Confirm the correct environment was used. |
| 403 Forbidden | Missing scope | Review the application's assigned scopes. |
| OAuth request fails | Incorrect Client Secret | Verify your application credentials. |
| Authentication succeeds but requests fail | Environment mismatch | Confirm credentials and API endpoints belong to the same environment. |

When troubleshooting, always verify that your credentials, authorization server, access token, and API endpoint all belong to the same Atlas Commerce environment.

Environment mismatches are among the most common causes of initial integration failures.

# Security Best Practices

Authentication credentials represent one of the most sensitive assets within an API integration. Protecting those credentials should be considered part of your application's overall security architecture rather than an implementation detail.

Atlas Commerce recommends the following practices for all integrations.

## Protect Client Secrets

Treat Client Secrets with the same level of protection as production passwords.

Never:

- Commit Client Secrets to source control.
- Embed Client Secrets in browser applications.
- Embed Client Secrets in mobile applications.
- Include Client Secrets in log files.
- Share Client Secrets through email or messaging platforms.

Instead, store Client Secrets in a secure secrets management system appropriate for your infrastructure.

---

## Use HTTPS for Every Request

Atlas Commerce requires HTTPS for all API communication.

TLS encryption protects:

- Client credentials
- Access tokens
- Request payloads
- Response payloads

Applications should never disable certificate validation or transmit authentication credentials over unsecured connections.

---

## Request Only Required Scopes

Follow the Principle of Least Privilege.

Applications should request only the scopes necessary to perform their intended business functions.

Limiting permissions reduces operational risk while simplifying security reviews and access management.

---

## Cache Access Tokens

Access tokens are intended to be reused until expiration.

Avoid requesting a new token before every API request unless your architecture specifically requires it.

Proper token caching:

- Reduces network traffic
- Improves application performance
- Minimizes unnecessary authentication requests
- Reduces load on the authorization service

---

## Rotate Credentials Regularly

Credential rotation should be part of your organization's routine operational procedures.

Regular rotation helps reduce long-term exposure and supports compliance with many organizational security policies.

When rotating credentials:

1. Generate a new Client Secret.
2. Update your application configuration.
3. Verify successful authentication.
4. Revoke the previous secret.

---

## Monitor Authentication Activity

Authentication failures often provide the earliest indication of configuration errors or suspicious activity.

Monitor for:

- Repeated authentication failures
- Unexpected token requests
- Requests from unfamiliar environments
- Excessive authorization failures

Investigating these events early can help identify operational issues before they affect production workloads.

---

# Frequently Asked Questions

## How often should I request a new access token?

Request an access token once and reuse it until it expires.

Most applications should not request a new token before every API request.

---

## Should I cache access tokens?

Yes.

Applications should securely cache access tokens until they expire.

Caching improves performance while reducing unnecessary traffic to the authorization server.

---

## Can multiple services share one access token?

They can, provided they belong to the same trusted application and security boundary.

However, Atlas Commerce recommends registering separate applications whenever practical to simplify credential management, auditing, and permission assignment.

---

## Can I use Sandbox credentials in Production?

No.

Sandbox and Production environments are completely isolated.

Each environment has its own:

- Client credentials
- Authorization server
- Access tokens
- API endpoints

Credentials issued for one environment are never valid in another.

---

## Can I include my Client Secret in browser or mobile applications?

No.

Client Secrets are intended only for trusted server environments.

Public applications should use an OAuth flow designed for public clients, such as Authorization Code with PKCE.

---

## What happens when my access token expires?

Atlas Commerce returns an HTTP `401 Unauthorized` response indicating that the supplied token is no longer valid.

Your application should request a new access token before retrying the request.

Token expiration is a normal part of the authentication lifecycle and should be expected.

---

## Why doesn't Atlas Commerce require the Client Secret on every API request?

The Client Secret establishes the identity of your application.

Once that identity has been verified, Atlas Commerce issues a temporary bearer token.

Business APIs accept only bearer tokens, reducing the exposure of long-lived credentials while maintaining a consistent security model.

---

# Related Topics

Authentication is the foundation for every Atlas Commerce integration.

Continue with the following guides to build your integration.

| Guide | Description |
|---------|-------------|
| **Make Your First API Call** | Learn how to send your first authenticated request. |
| **Process Your First Payment** | Build your first complete payment workflow. |
| **Payments** | Understand the Atlas Commerce payment lifecycle. |
| **Webhooks** | Receive asynchronous notifications from Atlas Commerce. |
| **Error Handling** | Learn how to diagnose and recover from API errors. |
| **Authorization and Scopes** | Explore application permissions in greater detail. |

---

# Authentication Checklist

Before continuing, verify that your integration can successfully complete each of the following tasks.

- ✅ Registered an Atlas Commerce application.
- ✅ Obtained Sandbox credentials.
- ✅ Stored the Client Secret securely.
- ✅ Requested an OAuth access token.
- ✅ Successfully authenticated an API request.
- ✅ Verified granted scopes.
- ✅ Implemented token caching.
- ✅ Tested token renewal after expiration.

Completing these tasks confirms that your application is ready to interact securely with Atlas Commerce APIs.

---

# Summary

Authentication establishes the trusted relationship between your application and the Atlas Commerce platform.

Using the OAuth 2.1 Client Credentials flow, your application exchanges its Client ID and Client Secret for a short-lived bearer access token. That token is then presented with each API request, allowing Atlas Commerce to verify your application's identity and determine which operations it is authorized to perform.

By separating long-lived application credentials from day-to-day API traffic, Atlas Commerce reduces credential exposure while providing a familiar, standards-based authentication model that aligns with modern API security practices.

Throughout this guide you learned how to:

- Register an application.
- Protect Client Secrets.
- Request OAuth access tokens.
- Authenticate API requests.
- Understand scopes and permissions.
- Diagnose authentication failures.
- Apply recommended security practices.

Your application is now ready to communicate securely with Atlas Commerce.

Continue to **Make Your First API Call** to begin building your first end-to-end Atlas Commerce integration.
