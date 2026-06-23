# Authentication

**Page Type:** Capability

Authentication is the first step in every Atlas Commerce integration.

Before your application can create payments, retrieve customer information, issue refunds, or receive webhook events, it must prove its identity to the Atlas Commerce platform. Authentication establishes trust between your application and Atlas Commerce, allowing the platform to determine who is making a request and what that application is permitted to do.

Atlas Commerce uses the industry-standard **OAuth 2.1 Client Credentials** authentication flow for server-to-server integrations. Your application exchanges a client ID and client secret for a short-lived access token, which is then included with each API request using the HTTP `Authorization` header.

This authentication model is designed to provide strong security while remaining straightforward to implement. By issuing short-lived access tokens rather than transmitting long-lived credentials with every request, Atlas Commerce minimizes credential exposure and simplifies credential rotation.

This guide explains how authentication works, how to obtain credentials, how to request access tokens, and how to securely authenticate every request to the platform.

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

1. Register your application and receive a client ID and client secret.
2. Exchange those credentials for an OAuth access token.
3. Include the access token in the `Authorization` header of each API request.
4. Request a new access token when the existing token expires.

The following sequence diagram illustrates the complete authentication flow.

```mermaid
sequenceDiagram
    participant App as Your Application
    participant Auth as Atlas Authorization Server
    participant API as Atlas Commerce API

    App->>Auth: Request access token<br/>(client ID + client secret)
    Auth-->>App: Access token (Bearer)

    App->>API: API request<br/>Authorization: Bearer &lt;access_token&gt;
    API-->>App: Requested resource

    Note over App: Reuse the access token until it expires

    App->>Auth: Request new access token
    Auth-->>App: New access token
```

Notice that the client secret is never sent to the business APIs.

Only the authorization server receives your client credentials.

All business APIs—including Payments, Customers, Orders, Refunds, and Reporting—accept only bearer access tokens.

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

This separation provides several important benefits.

The authorization server is responsible only for verifying application identity and issuing access tokens. Individual APIs are responsible only for validating those tokens and determining whether the requested operation is permitted.

This architecture simplifies security, improves scalability, and ensures that every Atlas Commerce service uses a consistent authentication model.

---

# Sandbox and Production

Atlas Commerce maintains separate environments for development and production.

Each environment has its own credentials, authorization server, and API endpoints.

| Environment | Purpose |
|------------|---------|
| Sandbox | Build and test integrations without processing live transactions. |
| Production | Process live customer transactions. |

Sandbox credentials cannot be used against production APIs, and production credentials cannot be used against sandbox APIs.

This separation helps prevent accidental use of live systems during development and allows integrations to be thoroughly tested before deployment.

Throughout this guide, examples use sandbox endpoints and sample credentials.

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

Atlas Commerce uses two long-lived credentials to identify your application.

| Credential | Purpose |
|------------|---------|
| **Client ID** | Public identifier for your application. |
| **Client Secret** | Private credential used to request OAuth access tokens. |

The Client Secret functions much like a password.

Unlike an access token, it should never be transmitted when calling business APIs and should never be embedded in client-side applications, mobile applications, browser-based JavaScript, or source code repositories.

Instead, store the Client Secret in a secure secrets management system such as a cloud secrets manager, encrypted configuration store, or environment variable managed by your deployment platform.

---

# Environment Credentials

Atlas Commerce issues separate credentials for each environment.

| Environment | Authorization Server | Intended Use |
|-------------|---------------------|--------------|
| Sandbox | `https://auth.sandbox.atlas-commerce.example` | Development and testing |
| Production | `https://auth.atlas-commerce.example` | Live production traffic |

Sandbox credentials cannot authenticate against production APIs.

Likewise, production credentials cannot be used within the sandbox environment.

Using separate credentials helps isolate development from production and prevents accidental processing of live customer transactions during testing.

---

# Request an Access Token

Atlas Commerce uses the OAuth 2.1 Client Credentials flow for server-to-server integrations.

Your application exchanges its Client ID and Client Secret for a short-lived access token by sending a request to the authorization server.

The access token is then included in the `Authorization` header of subsequent API requests.

The Client Secret is used only during this exchange.

---

## Token Request

```http
POST /oauth/token HTTP/1.1
Host: auth.sandbox.atlas-commerce.example
Content-Type: application/json
Accept: application/json
```

```json
{
  "clientId": "atlas_demo_application",
  "clientSecret": "atlas_demo_secret",
  "grantType": "client_credentials",
  "scope": "payments:read payments:write customers:read"
}
```

### Request Fields

| Field | Required | Description |
|--------|:--------:|-------------|
| `clientId` | Yes | The Client ID assigned during application registration. |
| `clientSecret` | Yes | The Client Secret associated with the registered application. |
| `grantType` | Yes | OAuth grant type. Atlas Commerce currently supports `client_credentials` for server-to-server integrations. |
| `scope` | No* | Optional list of requested scopes. If omitted, the authorization server issues the default scopes assigned to the application. |

---

# Successful Response

If the request succeeds, the authorization server returns a bearer access token.

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

### Response Fields

| Field | Description |
|--------|-------------|
| `accessToken` | OAuth bearer token used to authenticate API requests. |
| `tokenType` | Authentication scheme. Always `Bearer`. |
| `expiresIn` | Number of seconds until the token expires. |
| `scope` | Scopes granted to the issued token. |

Applications should cache the access token and reuse it until it expires rather than requesting a new token before every API request.

---

# Access Token Lifecycle

Access tokens are intentionally short-lived.

Short-lived tokens reduce security risk by limiting the amount of time a compromised token remains valid.

The recommended lifecycle is:

1. Request an access token.
2. Cache the token securely.
3. Use the token for subsequent API requests.
4. Request a new token when the existing token expires.

Applications should avoid requesting a new token before every API call unless specifically required by their architecture.

Excessive token requests increase unnecessary network traffic and may be subject to rate limiting.

---

# Understanding Scopes

Scopes define which Atlas Commerce capabilities an application is permitted to access.

When an access token is issued, it contains one or more scopes assigned to the application during registration.

Typical scopes include:

| Scope | Description |
|---------|-------------|
| `payments:read` | Retrieve payment information. |
| `payments:write` | Create or modify payment transactions. |
| `customers:read` | Retrieve customer records. |
| `customers:write` | Create or update customer records. |
| `refunds:write` | Issue refunds for eligible transactions. |
| `webhooks:read` | Retrieve webhook endpoint configuration. |

Applications should request only the scopes necessary for their intended functionality.

Following the principle of least privilege limits the potential impact of compromised credentials while simplifying operational security.

---

# What's Next?

At this point your application has successfully authenticated with the Atlas Commerce authorization server and obtained an OAuth access token.

In the next section you'll use that access token to make your first authenticated API request and learn how Atlas Commerce secures every request using the standard HTTP `Authorization` header.

# Make an Authenticated Request

Once your application has obtained an access token, it can begin calling Atlas Commerce APIs.

Every protected API request must include the access token in the HTTP `Authorization` header using the Bearer authentication scheme.

Atlas Commerce validates the token before processing the request. If the token is valid and includes the required scopes, the request proceeds normally. Otherwise, the request is rejected with an authentication or authorization error.

---

## Authorization Header

Include the access token in the following format:

```http
Authorization: Bearer <access_token>
```

For example:

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.example-token
```

The `Bearer` keyword is case-sensitive and must be included exactly as shown.

---

# Example Request

The following example retrieves a payment using an authenticated request.

```bash
curl --request GET \
  --url https://api.sandbox.atlas-commerce.example/v1/payments/pay_123456789 \
  --header "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.example-token" \
  --header "Accept: application/json"
```

---

# Example Response

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

This example demonstrates a successful authenticated request. The authorization server is no longer involved once the access token has been issued. Instead, the Payments API validates the bearer token before processing the request.

---

# Authentication vs. Authorization

Authentication and authorization are closely related, but they solve different problems.

Authentication answers the question:

> **Who is making this request?**

Authorization answers the question:

> **Is this application allowed to perform this action?**

Atlas Commerce performs both checks before processing every protected request.

For example:

- A valid access token successfully authenticates the application.
- The token's scopes determine which API operations are permitted.
- If the token lacks the required scope, the request is rejected even though authentication succeeded.

Understanding this distinction helps diagnose common integration issues and prevents unnecessary troubleshooting.

---

# Common Authentication Errors

Authentication failures generally occur before business logic is executed.

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

Verify that every protected request includes the `Authorization` header using the Bearer format.

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

Request a new access token from the authorization server and retry the request.

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

Ensure that:

- The token was issued by the correct Atlas Commerce environment.
- The token has not expired.
- The Authorization header is correctly formatted.
- The token has not been modified.

---

## Insufficient Scope

```http
HTTP/1.1 403 Forbidden
```

```json
{
  "error": {
    "code": "insufficient_scope",
    "message": "The access token does not include the required scope: payments:write."
  }
}
```

Authentication succeeded, but the application is not authorized to perform the requested operation.

Review the application's assigned scopes and request additional permissions if necessary.

---

# Troubleshooting Authentication

Most authentication issues can be resolved by verifying a few common conditions.

| Problem | Possible Cause | Recommended Action |
|----------|----------------|--------------------|
| 401 Unauthorized | Missing bearer token | Verify the Authorization header. |
| 401 Unauthorized | Expired access token | Request a new access token. |
| 401 Unauthorized | Invalid token | Confirm the token was issued by the correct environment. |
| 403 Forbidden | Missing scope | Review the application's assigned scopes. |
| Token request fails | Incorrect Client Secret | Verify your application credentials. |
| Authentication succeeds but API calls fail | Sandbox/Production mismatch | Ensure credentials and API endpoints belong to the same environment. |

When troubleshooting authentication, begin by confirming that the correct credentials, authorization server, and API endpoint all belong to the same environment. Environment mismatches are among the most common causes of authentication failures during initial integration.

---

# Security Best Practices

Authentication credentials are among the most sensitive assets in an API integration. Protecting them should be considered part of the application's security architecture.

Atlas Commerce recommends the following practices:

- Store Client Secrets in a secure secrets manager.
- Never embed Client Secrets in browser or mobile applications.
- Never commit credentials to source control.
- Use HTTPS for every request.
- Rotate credentials periodically.
- Request only the scopes your application requires.
- Cache access tokens until expiration instead of requesting a new token for every API call.
- Use separate credentials for each environment.
- Monitor authentication failures for unusual activity.

Following these recommendations improves operational security while reducing the likelihood of authentication-related outages.

---

# Related Topics

After successfully authenticating with Atlas Commerce, continue your integration with the following guides:

- **Make Your First API Call**
- **Process Your First Payment**
- **Payments**
- **Error Handling**
- **Webhooks**
- **Authorization and Scopes**

These guides build upon the authentication model introduced in this document and demonstrate how authentication integrates with the broader Atlas Commerce platform.

---

# Summary

Authentication establishes the trusted relationship between your application and Atlas Commerce.

Using the OAuth 2.1 Client Credentials flow, your application exchanges its client credentials for a short-lived bearer token, which is then used to authenticate API requests. By separating long-lived credentials from day-to-day API traffic, Atlas Commerce improves security while providing a familiar and standards-based developer experience.

With authentication configured, you're ready to make your first API request and begin integrating with the Atlas Commerce platform.
