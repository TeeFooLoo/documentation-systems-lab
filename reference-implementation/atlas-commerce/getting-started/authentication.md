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
