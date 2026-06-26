# Authentication Pattern v2

**Pattern type:** API Documentation  
**Status:** Draft  
**Repository:** Documentation Systems Lab  
**Recommended location:** `documentation-patterns/authentication-pattern.md`  
**Version:** 2  
**Primary page type:** Capability guide  

## Overview

Authentication documentation explains how developers prove the identity of an application, user, service, or integration before accessing an API.

Authentication is often one of the first technical barriers developers encounter. If the authentication documentation is unclear, overly abstract, or scattered across multiple pages, developers may be unable to make a successful first request even when the rest of the API documentation is strong.

This pattern defines a reusable approach for documenting authentication as a platform capability. It is intended for API documentation, developer portals, SDK documentation, internal enablement documentation, and AI-assisted documentation systems.

The goal is to help readers answer practical implementation questions quickly:

- What authentication method does this API use?
- How do I get credentials?
- How do I send credentials with a request?
- What scopes or permissions do I need?
- What happens when authentication fails?
- How do I rotate or revoke credentials?
- How do environments, users, roles, and tokens relate?

In v2, this pattern also treats authentication as a first-use guide. A strong authentication page should not only describe the authentication mechanism. It should lead a developer from initial readiness through a successful authenticated request, then provide lifecycle, troubleshooting, and security guidance that supports real implementation.

## Pattern Intent

Use this pattern to create an authentication page that functions as a canonical capability page, onboarding guide, and implementation bridge.

The page should help a developer:

1. Understand the platform authentication model.
2. Confirm they have the required prerequisites.
3. Obtain or identify the correct credentials.
4. Request a token or construct the required authentication material.
5. Send an authenticated API request.
6. Understand permissions, scopes, roles, or claims.
7. Handle expiration, refresh, rotation, and revocation.
8. Diagnose authentication and authorization failures.
9. Apply basic security practices without exposing internal security implementation details.

The page should be written for a developer who may be new to the platform but is capable of using HTTP, JSON, a REST client, and API reference documentation.

## Problem Statement

Authentication is frequently documented either too narrowly or too abstractly. Some documentation only describes the authentication scheme, such as OAuth 2.0, API keys, bearer tokens, mutual TLS, or signed requests. Other documentation explains individual credential fields but does not show how those fields are used in a complete request. In both cases, readers are left to infer the implementation path from fragments.

Authentication also tends to become fragmented across product areas. One API may document access tokens in one section, another may document API keys elsewhere, and a third may explain permissions or scopes in a separate onboarding guide. This forces developers to assemble the full picture themselves, often before they understand the platform well enough to know what they are looking for.

A good authentication page should not merely identify the authentication mechanism. It should explain the authentication model, provide complete examples, describe common failure modes, and link to the relevant authorization and security concepts.

In addition, a good authentication page should reduce cognitive load by presenting the path in the same order the developer experiences it:

1. What this guide is for.
2. What the developer needs before starting.
3. What authentication method the platform uses.
4. How credentials, tokens, environments, and scopes relate.
5. How to request the token or credential artifact.
6. How to use it in a real API request.
7. How to handle errors and lifecycle behavior.

## Design Principles

### Make authentication a capability, not a scattered prerequisite

If authentication applies across multiple APIs or products, document it once as an authoritative capability. Product-specific pages should link to the canonical authentication page and describe only product-specific differences.

### Lead with the practical model

Begin with what the developer needs to do, not with a protocol lecture. Protocol details are useful only after the reader understands the implementation path.

### Show the complete path

A token request without a follow-up authenticated API request is incomplete. A credential table without a full request example is also incomplete. Show both how to obtain authentication material and how to use it.

### Separate authentication from authorization

Authentication answers the question, “Who is making the request?” Authorization answers the question, “What is that identity allowed to do?” The authentication page should explain this distinction, especially where scopes, permissions, or roles affect requests.

### Distinguish environments explicitly

Authentication failures often happen because credentials, tokens, authorization servers, and API base URLs are mixed across environments. Sandbox, staging, and production behavior should be shown clearly.

### Keep internal security details out

Developer-facing authentication documentation should explain the external contract. It should not expose internal infrastructure, private key handling, security operations, internal threat models, or provisioning workflows that do not help integrators authenticate successfully.

### Design for onboarding and support

Authentication documentation should help a new developer succeed and help support teams diagnose common failures. Include expected errors, troubleshooting steps, and common mistakes.

## When to Use This Pattern

Use this pattern when documenting any API or developer platform that requires credentials, tokens, signatures, certificates, or other proof of identity.

This pattern is especially useful when:

- Authentication applies across multiple APIs or products.
- Developers must obtain credentials before making their first request.
- Multiple environments use different credentials.
- Access depends on scopes, permissions, roles, grants, or claims.
- Tokens expire and must be refreshed.
- Credentials can be rotated or revoked.
- Authentication errors are common during onboarding.
- AI-assisted search or documentation review depends on clear, centralized explanations.

Use this pattern for platform-level authentication pages, API-specific authentication guides, SDK authentication examples, and onboarding flows that require a successful first authenticated request.

## When Not to Use This Pattern

Do not use this pattern as a substitute for full security architecture documentation.

Authentication documentation for developers should explain what integrators need to know to authenticate successfully. It should not expose internal security implementation details, infrastructure design, private key handling practices, internal threat models, or sensitive operational procedures.

Do not duplicate a full authentication explanation on every API page. If authentication is a shared platform capability, document it once in an authoritative location and link to it from product-specific or endpoint-specific pages.

Do not mix unrelated authorization policy details into the authentication page unless they directly affect how a developer obtains or uses credentials. Authentication answers the question, “Who or what is making the request?” Authorization answers the question, “What is that identity allowed to do?”

Do not use this pattern to document user login, identity federation, consent screens, or account recovery unless those concepts are part of the developer-facing API authentication contract. For user-facing identity flows, use a dedicated identity or authorization-code-flow pattern.

## Recommended Page Metadata

Authentication pages should include lightweight metadata that helps readers assess whether the guide applies to them.

Recommended metadata:

```markdown
# Authentication

**Page Type:** Capability
**Estimated Time:** 10-15 minutes
**Audience:** Developers building server-to-server integrations
**Applies to:** All protected APIs
```

Use metadata sparingly. The goal is to orient the reader, not to create administrative overhead.

## Recommended Structure

An authentication page should include the following sections. The exact order can vary by product, but the structure should preserve the implementation journey: orient, explain, prepare, authenticate, call, secure, troubleshoot, and continue.

### 1. Title and Page Type

Use a direct title such as `Authentication`. If the documentation system supports page metadata, identify the page as a capability page.

Example:

```markdown
# Authentication

**Page Type:** Capability
```

The page should feel like the authoritative home for authentication, not a narrow reference fragment.

### 2. Overview

Explain the authentication model in plain language. The overview should state what method the API uses and what developers must provide with each request. Avoid beginning with protocol details before explaining the practical implementation path.

Example:

> Atlas Commerce APIs use bearer token authentication. To call an API, first request an access token using your client credentials, then include the token in the `Authorization` header of each request.

The overview should answer:

- What authentication method is used?
- Is authentication required for all endpoints?
- Are there different methods for different API surfaces?
- What is the shortest path to a successful authenticated request?

A strong overview also explains why the model exists in implementation terms. For example, if the platform uses short-lived access tokens, explain that this avoids sending long-lived secrets with every API request.

### 3. Estimated Time

Include an estimated completion time when the page is instructional.

Example:

```markdown
## Estimated Time

**10-15 minutes**
```

This helps readers understand whether the page is a quick setup guide, a deep conceptual article, or a longer implementation walkthrough.

Use estimated time for guide-style pages. It is optional for pure reference pages.

### 4. Prerequisites

List what developers need before they can authenticate.

Prerequisites may include:

- A developer account
- An application registration
- Sandbox credentials
- Production credentials
- Approved scopes
- A callback URL
- A public/private key pair
- Certificate registration
- Environment-specific base URLs
- A REST client such as curl, Postman, or Insomnia
- A basic understanding of HTTP and JSON

Keep this section practical. A developer should be able to look at the list and know whether they are ready to proceed.

Do not bury prerequisites after implementation examples. Authentication pages often fail when readers reach an example and only then discover they are missing credentials, scopes, or environment access.

### 5. When to Use This Guide

Add a short section that explains who should read the guide and when.

Example:

```markdown
## When to Use This Guide

Read this guide if you are:

- Building your first integration.
- Configuring a new application.
- Troubleshooting authentication failures.
- Rotating application credentials.
```

This is especially helpful when the documentation set also contains API reference pages, SDK pages, product-specific pages, or security architecture documentation.

The section should also tell readers where to go for endpoint-specific requirements.

Example:

> If you are looking for endpoint-specific authentication requirements, refer to the API Reference documentation.

### 6. Learning Objectives

For onboarding-oriented authentication pages, include learning objectives. Learning objectives should describe what the reader will be able to do after completing the page.

Example:

```markdown
## Learning Objectives

After completing this guide, you will be able to:

- Understand the authentication model.
- Distinguish between client credentials and access tokens.
- Request an access token.
- Authenticate API requests using bearer tokens.
- Diagnose common authentication failures.
- Apply recommended security practices.
```

Learning objectives are optional, but they help clarify scope and prevent the page from drifting into unrelated security topics.

### 7. Authentication at a Glance

Provide a compact summary of the authentication model before diving into details.

This section should identify the major components and their purposes.

Example table:

| Component | Purpose |
|         --|         |
| Client ID | Identifies the registered application. |
| Client Secret | Authenticates the application when requesting an access token. |
| Access Token | Short-lived bearer token used to authenticate API requests. |
| OAuth Client Credentials Flow | Exchanges client credentials for an access token. |
| Scopes | Define which APIs the application may access. |

The goal is to create a mental model before the reader encounters implementation steps.

This section is especially useful for AI-assisted documentation review because it provides explicit terminology and relationships.

### 8. Authentication Model

Describe the major parts of the authentication system and how they relate.

Depending on the API, this may include:

- Applications
- Client IDs
- Client secrets
- API keys
- Access tokens
- Refresh tokens
- Service accounts
- Users
- Roles
- Scopes
- Environments
- Certificates
- Signatures

The purpose of this section is conceptual clarity. Readers should understand the model before they copy a request. For example, if tokens are issued to applications rather than users, say so explicitly. If sandbox and production use separate credentials, explain that before showing examples.

The model section should also clarify what should not happen. For example:

> Client credentials are used only when requesting an access token. After an access token has been obtained, the access token is the credential used for subsequent API requests. Applications should not send client secrets to business APIs.

### 9. Why This Authentication Method

When a platform uses a specific authentication approach, briefly explain why. This is not a standards essay. It is a practical explanation of the tradeoff the developer needs to understand.

Example:

> The platform uses OAuth client credentials because it separates long-lived application credentials from day-to-day API traffic. Instead of sending a client secret with every request, the application exchanges the client secret for a short-lived access token and sends that token to protected APIs.

Useful points may include:

- Long-lived credentials are not transmitted with every business API request.
- Access tokens have limited lifetimes.
- Scopes can limit what a token can do.
- Credential rotation can happen without changing every API call.
- The method aligns with a familiar industry standard.

This section helps developers understand the model rather than treating authentication as arbitrary syntax.

### 10. Choosing the Right Authentication Flow

If the platform supports or references multiple authentication flows, explain which flow applies and which flows do not.

Example:

> Use the Client Credentials flow for trusted server-to-server integrations. Do not use this flow for browser-only, mobile, or desktop applications that cannot protect a client secret.

Include examples of appropriate application types:

- Backend web applications
- Payment gateways
- Server-side integrations
- Enterprise middleware
- Scheduled jobs
- Internal business services

Include examples of inappropriate application types when relevant:

- Browser-only JavaScript applications
- Native mobile applications
- Desktop applications
- Public clients that cannot protect a secret

If another flow is required for those scenarios, link to the appropriate guide instead of explaining the whole alternate flow in the authentication page.

### 11. How Authentication Works

Describe the end-to-end authentication process as a short sequence.

Example:

1. Register the application and receive credentials.
2. Exchange credentials for an access token.
3. Include the access token in the `Authorization` header of each API request.
4. Request a new access token when the existing token expires.

Add a diagram when the model benefits from visualization.

Example Mermaid sequence diagram:

```mermaid
sequenceDiagram
    participant App as Your Application
    participant Auth as Authorization Server
    participant API as API

    App->>Auth: Request access token<br/>(Client ID + Client Secret)
    Auth-->>App: Bearer access token

    App->>API: API request<br/>Authorization: Bearer <access_token>
    API-->>App: Requested resource

    Note over App: Reuse the access token until it expires

    App->>Auth: Request new access token
    Auth-->>App: New bearer access token
```

Diagrams should reinforce the implementation path. Avoid diagrams that introduce internal architecture the developer does not need.

### 12. Authentication Architecture

When useful, show the separation between the authorization service and business APIs.

Example:

```text
                     Client Credentials
                (Client ID + Client Secret)
                           │
                           ▼
                  Authorization Server
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

This section should explain the external-facing architecture only.

Useful points:

- The authorization server verifies application identity.
- The authorization server issues access tokens.
- Business APIs accept bearer tokens.
- Client secrets are not sent to business APIs.
- APIs validate tokens before executing operations.

Do not expose private infrastructure, internal validation services, network topology, or security-sensitive implementation details.

### 13. Environments

Clearly distinguish sandbox, staging, and production behavior.

Authentication errors frequently occur when developers mix credentials and base URLs. If sandbox credentials cannot be used in production, say so explicitly.

Example table:

| Environment | Authorization Server | API Base URL | Credentials |
|            -|                     -|            --|            -|
| Sandbox | `https://auth.sandbox.example` | `https://api.sandbox.example` | Sandbox credentials only |
| Production | `https://auth.example` | `https://api.example` | Production credentials only |

Also document any differences in token lifetimes, scopes, approval workflows, test data, or credential provisioning. The environment section should make four relationships explicit:

1. Credentials belong to an environment.
2. Tokens are issued for an environment.
3. API base URLs belong to an environment.
4. Data is isolated by environment.

If examples use sandbox endpoints, state that clearly.

### 14. Register the Application

Explain how an application becomes known to the platform. This section should describe the external contract rather than internal provisioning details.

Include:

- Where registration happens.
- What the developer receives.
- Whether credentials differ by environment.
- Whether scopes are assigned during registration.
- Whether production access requires approval.

Example:

```markdown
Each registered application receives:

- A Client ID
- A Client Secret
- One or more authorized scopes
- Environment-specific credentials
```

If application registration is handled by an onboarding team rather than self-service, say what the developer should request and from whom.

### 15. Application Credentials

Explain each credential and whether it is secret.

Example table:

| Credential | Purpose | Secret? |
|            |         |:      -:|
| Client ID | Identifies the application to the authorization server. | No |
| Client Secret | Proves the identity of the application when requesting access tokens. | Yes |

This section should include clear security guidance for secrets:

- Never commit secrets to source control.
- Never embed secrets in browser or mobile applications.
- Never include secrets in logs.
- Store secrets in a secure secrets management system.

This section should also explain where credentials are used:

> The client secret is used only when requesting an access token. It is not sent to business APIs after a token has been issued.

### 16. Get Credentials

Explain how credentials are obtained.

This section should describe the process at the right level of detail for the audience. If credentials are self-service, provide the steps. If credentials are issued during onboarding, explain what the reader should request and from whom.

Avoid documenting internal provisioning details that do not help the developer act. Instead, describe the external contract: what credentials exist, how they are delivered, how they are scoped, and how they differ by environment.

This section may overlap with application registration. If so, combine them under a single heading, but make sure the page still answers:

- How do I get a Client ID, API key, certificate, or other credential?
- How do I know which environment the credential belongs to?
- How do I know which scopes or permissions are assigned?
- What should I do if I need production credentials?

### 17. Request an Access Token

If the authentication model uses tokens, provide a complete token request.

Canonical examples are essential here. Developers should not have to infer headers, grant types, content types, or request bodies from prose alone.

Include:

- Token endpoint
- Required headers
- Example request payload
- Request field table
- Example cURL request
- Successful response
- Response field table

Example token endpoint:

```http
POST https://auth.sandbox.atlas-commerce.example/oauth/token
```

Example request headers:

```http
Content-Type: application/json
Accept: application/json
```

Example request:

```json
{
  "clientId": "atlas_demo_application",
  "clientSecret": "atlas_demo_secret",
  "grantType": "client_credentials",
  "scope": "payments:read payments:write customers:read"
}
```

Example cURL request:

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

Example successful response:

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

The example should be complete enough that a developer can adapt it directly.

### 18. Understand Access Tokens

Explain what access tokens are and how they should be treated.

Useful points:

- Access tokens are temporary credentials.
- Access tokens represent the authenticated application or identity.
- Access tokens are designed to be transmitted with API requests.
- Access tokens are usually environment-specific.
- Access tokens are often scope-limited.
- Developers should not modify, decode, or generate tokens themselves unless the platform specifically instructs them to do so.

Example:

> Access tokens are temporary credentials that represent your application's authenticated identity. Unlike client secrets, access tokens are designed to be sent with API requests. Their limited lifetime reduces the security impact of accidental exposure.

This section helps prevent developers from treating tokens as static configuration values.

### 19. Token Lifecycle

If tokens expire, explain the expiration behavior and the recommended handling strategy.

This section should describe:

- How long access tokens remain valid
- Whether refresh tokens are supported
- Whether developers should cache tokens
- When to request a new token
- What error indicates expiration
- Whether retry behavior is recommended

Example:

> Access tokens expire after one hour. Applications should cache the token until it expires and request a new token before making additional API calls. Do not request a new token for every API request unless instructed to do so.

This type of guidance prevents inefficient integrations and reduces avoidable authentication traffic.

Example lifecycle diagram:

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

Make the recommended behavior explicit. Authentication pages should not assume developers know whether to cache tokens, refresh tokens, or request a new token for every call.

### 20. Make an Authenticated Request

Show exactly how to use the credential or token in an API request.

Example:

```bash
curl -X GET "https://api.sandbox.atlas-commerce.example/v1/payments/pay_12345" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.example" \
  -H "Content-Type: application/json"
```

This section is critical because developers often understand how to obtain a token but still fail when applying it to an actual request.

If the API uses API keys, signatures, certificates, or multiple headers, show the exact required format.

The section should include:

- Required header or credential placement.
- Example request.
- Example successful response.
- Explanation of what success confirms.

Example header:

```http
Authorization: Bearer <access_token>
```

Example successful response:

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

A successful authenticated request should confirm that:

- Authentication succeeded.
- The access token is valid.
- The token contains the required permissions.
- The requested resource exists.
- The application can communicate with the platform API.

### 21. Protected Request Validation

When useful, explain what the API validates before executing business logic.

Example:

Before processing the request, the API validates that:

- The token was issued by the platform authorization server.
- The token has not expired.
- The token belongs to the current environment.
- The token contains the required scopes.
- The token has not been revoked.

This should be written from the developer's perspective. Do not document internal validation mechanisms.

A short sequence diagram can help:

```mermaid
sequenceDiagram
    participant App as Your Application
    participant API as Protected API
    participant Auth as Token Validation

    App->>API: GET /resources/{id}<br/>Authorization: Bearer token
    API->>Auth: Validate access token
    Auth-->>API: Token valid<br/>Scopes confirmed
    API-->>App: HTTP 200 OK
```

If the API validates tokens locally, phrase that carefully and only at the level the developer needs to understand. Avoid exposing internal implementation details.

### 22. Scopes and Permissions

Explain what scopes, permissions, claims, roles, or grants mean in the context of the API.

This section should help developers answer:

- Which scopes do I need?
- Are scopes assigned during onboarding or requested dynamically?
- What happens if I request a scope I do not have?
- Are scopes different between sandbox and production?
- Do read and write operations require different permissions?

Avoid listing internal permission names without explaining when developers need them. A scope reference is useful only when it is connected to real tasks.

Example table:

| Scope | Allows | Typical use |
|      -|      --|            -|
| `payments:read` | Read payment records | Retrieve payment status |
| `payments:write` | Create and update payments | Create a payment or refund |
| `customers:read` | Read customer records | Display customer details |
| `customers:write` | Create and update customers | Create customer profiles |

Explain the relationship between authentication and authorization:

> Authentication answers “Who are you?” Scopes answer “What are you allowed to do?”

This distinction is important enough to include directly in the page, not only in an error-handling topic.

### 23. Authentication vs. Authorization

Include a dedicated section if the distinction is likely to cause confusion.

Example:

| Scenario | Authentication | Authorization |
|         -|               |               |
| Valid token with required scope | ✓ | ✓ |
| Expired token | ✗ | — |
| Invalid token | ✗ | — |
| Missing required scope | ✓ | ✗ |

This section should make it clear why some failures return `401 Unauthorized` and others return `403 Forbidden`.

Useful explanation:

- `401 Unauthorized` usually means the request could not be authenticated.
- `403 Forbidden` usually means the request was authenticated but not permitted.

### 24. Verify Authentication

After the first authenticated request, help readers interpret the result.

Example table:

| Status | Meaning |
|      --|         |
| `200 OK` | Authentication succeeded and the request completed successfully. |
| `201 Created` | Authentication succeeded and a resource was created. |
| `401 Unauthorized` | Authentication failed. |
| `403 Forbidden` | Authentication succeeded, but the application lacks required permissions. |

This section creates a bridge from implementation to troubleshooting.

### 25. Credential Rotation and Revocation

Explain how credentials should be rotated, replaced, or revoked.

This section should describe the recommended operational behavior rather than internal security procedures.

Include guidance such as:

- Rotate credentials periodically.
- Store secrets securely.
- Never commit secrets to source control.
- Revoke credentials that may have been exposed.
- Use separate credentials for each environment.
- Use separate credentials for different applications when possible.

If the platform supports multiple active credentials during rotation, explain the safe rotation sequence.

Example rotation sequence:

1. Generate a new client secret.
2. Update application configuration.
3. Verify successful authentication with the new secret.
4. Revoke the previous secret.

This section should help developers operate safely without revealing internal security procedures.

### 26. Error Responses

Authentication documentation must include common authentication and authorization errors.

Examples should include both the HTTP status code and the response payload.

Example:

```http
HTTP/1.1 401 Unauthorized
```

```json
{
  "error": {
    "code": "invalid_token",
    "message": "The access token is missing, expired, or invalid."
  }
}
```

Example:

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

Explain the difference between authentication failures and authorization failures. Developers need to know whether the issue is identity, permissions, environment mismatch, token expiration, or malformed request syntax.

Recommended error examples:

- Missing authorization header
- Expired access token
- Invalid access token
- Insufficient scope
- Invalid client credentials
- Environment mismatch
- Malformed signature, when applicable
- Certificate validation failure, when applicable

### 27. Common Authentication Errors

For guide-style pages, present the most common errors as named subsections.

Example structure:

```markdown
## Common Authentication Errors

### Missing Authorization Header

### Expired Access Token

### Invalid Access Token

### Insufficient Scope
```

Each error should include:

- HTTP status code
- Example response payload
- Plain-language cause
- Recommended action

Example:

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

Recommended action:

> Verify that the request includes an `Authorization` header using the Bearer authentication scheme.

### 28. Security Guidance

Provide practical security guidance without exposing sensitive internal implementation details.

Recommended guidance may include:

- Use HTTPS for all API requests.
- Store credentials in a secure secrets manager.
- Do not expose secrets in client-side code.
- Do not send credentials in query parameters.
- Do not log tokens or secrets.
- Use least-privilege scopes.
- Rotate credentials when team members or systems change.
- Separate credentials by application and environment.

This section should be brief, actionable, and aligned with the product’s actual security model.

For guide-style pages, consider breaking security guidance into short subsections:

- Protect client secrets
- Use HTTPS for every request
- Request only required scopes
- Cache access tokens appropriately
- Rotate credentials regularly
- Monitor authentication activity

Security guidance should tell developers what to do, not merely warn them that authentication is sensitive.

### 29. Troubleshooting

Include a focused troubleshooting section for common onboarding failures.

Common issues include:

| Symptom | Possible cause | Recommended action |
|         |               -|                  --|
| `401 Unauthorized` | Missing, expired, or malformed token | Request a new token and verify the `Authorization` header format. |
| `403 Forbidden` | Token lacks required scope | Confirm that the application has the required scope. |
| Token request fails | Invalid client credentials | Verify that the client ID and secret match the environment. |
| Sandbox request fails in production | Environment mismatch | Use production credentials with the production base URL. |
| Signature validation fails | Incorrect signing string or timestamp | Recreate the signature using the documented canonical string. |

Troubleshooting sections should be based on real support patterns whenever possible.

Always include an environment-mismatch check when environments are isolated. A useful instruction is:

> Verify that your credentials, authorization server, access token, and API endpoint all belong to the same environment.

### 30. Common Mistakes

Common mistakes can appear before or after troubleshooting. This section should name the most frequent errors in plain language.

Examples:

- Using sandbox credentials against production APIs.
- Using production credentials against sandbox APIs.
- Forgetting the `Bearer` prefix.
- Requesting a new access token before every API request.
- Including the client secret in business API requests.
- Embedding client secrets in browser or mobile applications.
- Assuming authentication and authorization are the same process.

Common mistakes are especially valuable when they reflect actual support tickets or onboarding failures.

### 31. Frequently Asked Questions

Add an FAQ when the authentication page needs to answer repeated implementation questions without interrupting the main flow.

Useful FAQ questions include:

- How often should I request a new access token?
- Should I cache access tokens?
- Can multiple services share one access token?
- Can I use sandbox credentials in production?
- Can I include my client secret in browser or mobile applications?
- What happens when my access token expires?
- Why does the platform not require the client secret on every API request?

FAQs should not replace the main implementation path. They should reinforce decisions that developers commonly question.

### 32. Related Topics

Authentication pages should link to related documentation rather than duplicating it.

Common related topics include:

- Authorization
- Scopes and permissions
- Error handling
- Rate limits
- Webhooks
- Sandbox testing
- Production access
- Security best practices
- SDK authentication
- API reference
- Make your first API call
- Process your first payment or first domain-specific workflow

Cross-linking helps authentication remain a single authoritative capability while still connecting it to implementation-specific guidance.

Example table:

| Guide | Description |
|      -|            -|
| Make Your First API Call | Learn how to send your first authenticated request. |
| Error Handling | Learn how to diagnose and recover from API errors. |
| Authorization and Scopes | Explore application permissions in greater detail. |
| Webhooks | Learn how asynchronous notifications are authenticated or verified. |

### 33. Final Authentication Checklist

End with a checklist that confirms the developer can proceed.

Example:

```markdown
## Authentication Checklist

Before continuing, verify that your integration can successfully complete each of the following tasks.

- [ ] Registered an application.
- [ ] Obtained sandbox credentials.
- [ ] Stored the client secret securely.
- [ ] Requested an access token.
- [ ] Successfully authenticated an API request.
- [ ] Verified granted scopes.
- [ ] Implemented token caching.
- [ ] Tested token renewal after expiration.
```

A checklist gives the reader a concrete sense of completion and creates a bridge to the next implementation guide.

### 34. Summary

Close with a short summary that restates the implementation path and the security model.

The summary should answer:

- What authentication establishes.
- What credentials are exchanged.
- What token or credential is used for API requests.
- What the developer learned.
- What the developer should do next.

Example:

> Authentication establishes the trusted relationship between your application and the platform. Your application exchanges its client credentials for a short-lived access token, then sends that token with each protected API request. By separating long-lived credentials from day-to-day API traffic, the platform reduces credential exposure while providing a familiar standards-based authentication model.

## Canonical Examples

Authentication documentation should include canonical examples for each supported authentication pattern.

Examples may include:

- API key authentication
- Bearer token authentication
- OAuth client credentials flow
- OAuth authorization code flow
- Refresh token flow
- Signed request authentication
- Mutual TLS
- SDK initialization
- Webhook signature verification

Examples should be realistic, complete, and syntactically valid. Avoid placeholder-heavy examples unless placeholders are necessary to prevent misuse of fake credentials.

Good examples show both the authentication step and a subsequent authenticated API call. A token request without a follow-up request leaves the developer with only half the implementation path.

## Example: OAuth Client Credentials Flow

The following example demonstrates the minimum canonical example set for a server-to-server OAuth client credentials page.

### Token Endpoint

```http
POST https://auth.sandbox.atlas-commerce.example/oauth/token
```

### Token Request

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

### Token Response

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

### Authenticated API Request

```bash
curl --request GET \
  --url https://api.sandbox.atlas-commerce.example/v1/payments/pay_123456789 \
  --header "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.example-token" \
  --header "Accept: application/json"
```

### Successful API Response

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

This example set is complete because it shows both sides of authentication: obtaining the token and using the token.

## Example: API Key Authentication

If the platform uses API keys instead of bearer tokens, the page should show where the key goes and what not to do.

Example:

```bash
curl --request GET \
  --url https://api.sandbox.atlas-commerce.example/v1/payments/pay_123456789 \
  --header "X-API-Key: sk_sandbox_example" \
  --header "Accept: application/json"
```

The page should specify:

- Whether the API key belongs in a header.
- Whether query-string authentication is prohibited.
- Whether keys are environment-specific.
- Whether keys expire.
- How keys are rotated or revoked.
- Whether different keys should be used for different applications.

Avoid vague instructions such as “include your API key with the request.”

## Example: Signed Request Authentication

If the platform uses request signing, the page should include a canonical signing example.

The page should explain:

- Which request components are signed.
- How the canonical string is constructed.
- Which hashing or signing algorithm is used.
- How timestamps and nonces are handled.
- What headers are required.
- What error indicates signature validation failure.

Example outline:

```text
1. Build the canonical string.
2. Hash the request body.
3. Sign the canonical string with the private key or shared secret.
4. Send the signature in the required header.
5. Retry only when the request is safe to retry.
```

Request signing examples must be precise. Developers should not have to infer whitespace, newline, header order, or timestamp format.


## Example: Mutual TLS

If the platform uses mutual TLS, the page should explain the developer-facing certificate contract.

Include:

- How the certificate is obtained or registered.
- Which environment the certificate applies to.
- Where the certificate is installed.
- Whether certificate rotation is supported.
- What happens when the certificate expires.
- What errors indicate certificate validation failures.

Avoid exposing internal certificate validation infrastructure or private operational procedures.

## Common Mistakes

### Mistake: Documenting authentication only in the API reference

Authentication is a prerequisite for using the API, not just a reference detail. It deserves a dedicated explanation that developers can find before they attempt their first request.

### Mistake: Scattering authentication details across products

If authentication is platform-wide, it should have a platform-level page. Product-specific pages should link to the authoritative authentication page and only describe product-specific differences.

### Mistake: Listing scopes without explaining tasks

Scope names are not self-explanatory. Connect each scope to the tasks it enables so developers know what to request and why.

### Mistake: Omitting error examples

Authentication failures are common during onboarding. Without error examples, developers cannot distinguish between invalid credentials, expired tokens, insufficient permissions, and environment mismatches.

### Mistake: Using incomplete examples

Developers should not have to guess where a token goes, which header is required, or whether the request body uses JSON or form encoding. Show complete examples whenever practical.

### Mistake: Mixing authentication and authorization

Authentication and authorization are related but distinct. Explain the difference clearly so readers understand whether they are proving identity or requesting permission.

### Mistake: Including internal provisioning details

Do not expose internal implementation, operational workflows, or security-sensitive details that do not help external developers authenticate successfully.

### Mistake: Explaining protocol choice without implementation guidance

A section such as “Why OAuth?” can be useful, but it should not replace the implementation steps. Explain why the model exists, then show how to use it.

### Mistake: Omitting token lifecycle behavior

Developers need to know whether to cache tokens, when tokens expire, and whether to request a token for every call. If the documentation does not say this explicitly, many integrations will make inefficient or incorrect assumptions.

### Mistake: Treating environment separation as obvious

Sandbox and production separation should be repeated in credential, token, and troubleshooting sections. Environment mismatch is common enough to deserve explicit guidance.

### Mistake: Forgetting the first successful API call

The authentication page should help the reader complete a real authenticated request. Without that step, the page stops before the developer has actually proven the integration works.

## AI Review Considerations

AI-assisted documentation review can help validate authentication documentation against this pattern.

An AI review skill should check whether the authentication documentation:

- Identifies the authentication method clearly.
- Explains the authentication model before implementation details.
- Provides complete token or credential examples.
- Shows at least one authenticated API request.
- Distinguishes authentication from authorization.
- Explains scopes or permissions in task-based language.
- Includes environment-specific guidance.
- Documents token expiration or credential lifecycle behavior.
- Includes common error responses.
- Provides troubleshooting guidance.
- Links to related security, error handling, and API reference pages.
- Avoids unnecessary internal implementation details.
- Avoids duplicating platform-wide content across product pages.

AI review should not be treated as final approval. Authentication documentation should also be reviewed by engineering, security, developer experience, and support teams when appropriate.

### Additional v2 AI Review Checks

A v2 review should also check whether the page:

- Identifies the page type or guide purpose.
- States when to use the guide.
- Lists prerequisites before implementation steps.
- Provides learning objectives when the page is instructional.
- Includes an “at a glance” model summary.
- Explains why the authentication method is used, when helpful.
- Identifies when a flow is appropriate and inappropriate.
- Includes a clear environment matrix.
- Shows token endpoint, headers, request body, cURL request, and response.
- Explains whether secrets are sent only to the authorization server.
- Shows exactly how the access token or credential is used in a protected API request.
- Explains what a successful authenticated request confirms.
- Includes named common errors with status codes and payloads.
- Includes common mistakes based on support patterns.
- Includes FAQ entries for repeated implementation questions.
- Ends with a practical authentication checklist.

## Pattern Applied: Atlas Commerce

The following outline demonstrates how this pattern might be applied to a fictional platform.

```text
Authentication

Page type
  Capability

Estimated time
  10-15 minutes

Overview
  Atlas Commerce APIs use OAuth 2.1 bearer token authentication.
  Applications exchange client credentials for short-lived access tokens.

Prerequisites
  Developer account
  Registered application
  Sandbox credentials
  REST client
  Basic HTTP and JSON knowledge

When to use this guide
  First integration
  New application configuration
  Authentication troubleshooting
  Credential rotation

Learning objectives
  Understand the model
  Request an access token
  Authenticate API requests
  Diagnose failures
  Apply security practices

Authentication at a glance
  Client ID
  Client Secret
  Access Token
  Client Credentials Flow
  Scopes

Why OAuth
  Separates long-lived credentials from business API traffic.

Choosing the right flow
  Use Client Credentials for server-to-server integrations.
  Do not use Client Credentials for public clients that cannot protect secrets.

How authentication works
  Register application
  Request token
  Send bearer token
  Renew token after expiration

Architecture
  Authorization server issues tokens.
  Business APIs accept bearer tokens.

Environments
  Sandbox authorization server and API URL
  Production authorization server and API URL
  Separate credentials and tokens

Register application
  Client ID
  Client Secret
  Authorized scopes
  Environment-specific credentials

Request access token
  POST /oauth/token
  Request headers
  Request body
  cURL example
  Successful response
  Response fields

Understand access tokens
  Temporary
  Environment-specific
  Scope-limited
  Signed

Token lifecycle
  Request
  Cache
  Use
  Renew after expiration

Make authenticated request
  Authorization: Bearer <access_token>
  GET /v1/payments/{paymentId}
  Successful response

Scopes
  payments:read
  payments:write
  customers:read
  customers:write

Authentication vs authorization
  401 for authentication failure
  403 for authorization failure

Errors
  401 missing_authorization
  401 token_expired
  401 invalid_token
  403 insufficient_scope

Troubleshooting
  Environment mismatch
  Expired token
  Missing scope
  Incorrect Authorization header

Security best practices
  Protect client secrets
  Use HTTPS
  Request least privilege
  Cache tokens
  Rotate credentials
  Monitor authentication activity

FAQ
  How often should I request a token?
  Can sandbox credentials be used in production?
  Can client secrets be used in browser applications?

Related topics
  Make Your First API Call
  Error Handling
  Authorization and Scopes
  Webhooks

Final checklist
  Registered application
  Obtained credentials
  Requested token
  Made authenticated request
  Verified scopes
  Implemented token caching
```

This structure gives developers both the conceptual model and the practical implementation path.

## Pattern Checklist

Use this checklist when reviewing authentication documentation.

- [ ] The page explains what authentication method is used.
- [ ] The page explains the authentication model in plain language.
- [ ] The page identifies when the guide should be used.
- [ ] The page includes estimated time when it functions as an implementation guide.
- [ ] The page includes learning objectives when it functions as an onboarding guide.
- [ ] The page includes an “at a glance” summary of key authentication components.
- [ ] Prerequisites are listed before implementation steps.
- [ ] Credential acquisition is explained.
- [ ] The page distinguishes credentials, tokens, scopes, and environments.
- [ ] The page explains why the selected authentication method is used, when useful.
- [ ] The page explains which authentication flow is appropriate and which flows are not appropriate.
- [ ] Complete request and response examples are provided.
- [ ] Token endpoint, headers, request fields, cURL request, and successful response are shown when applicable.
- [ ] At least one authenticated API request is shown.
- [ ] The page explains what a successful authenticated request confirms.
- [ ] Scopes or permissions are explained in task-based language.
- [ ] Authentication and authorization are distinguished clearly.
- [ ] Environment-specific behavior is documented.
- [ ] Token expiration, refresh, rotation, or revocation is addressed when applicable.
- [ ] Authentication errors include status codes and example payloads.
- [ ] Troubleshooting guidance addresses common onboarding failures.
- [ ] Common mistakes are identified.
- [ ] Security guidance is practical and does not expose internal implementation details.
- [ ] Related topics are linked.
- [ ] Internal-only implementation details are excluded.
- [ ] Authentication content is not duplicated unnecessarily across product pages.
- [ ] The page ends with a practical completion checklist or next step.

## Recommended Source-to-Page Mapping

Use this table to decide where information belongs.

| Information type | Belongs on authentication capability page? | Notes |
|                  |:                                          :|      -|
| Platform authentication model | Yes | This is core capability content. |
| Credential acquisition | Yes | Explain external process or onboarding request. |
| Token endpoint and token request example | Yes | Required for implementation. |
| Authenticated example API call | Yes | Shows the complete path. |
| Endpoint-specific required scopes | Sometimes | Include common scopes; link to reference for endpoint-specific scope matrix. |
| Full API reference | No | Link to API reference. |
| Internal credential provisioning workflow | No | Exclude unless externally actionable. |
| Security architecture | No | Link to security architecture if appropriate. |
| Credential rotation guidance | Yes | Keep operational and developer-facing. |
| Support escalation procedures | Sometimes | Include only if the developer needs them. |
| Webhook signature verification | Sometimes | Include summary and link to dedicated webhook authentication topic if substantial. |
| SDK-specific initialization | Sometimes | Include minimal examples or link to SDK pages. |

## Page Quality Bar

A page following this pattern is ready for review when a developer can answer the following questions without leaving the page:

- What authentication method does the platform use?
- What credentials do I need?
- How do I get those credentials?
- Which environment do the credentials belong to?
- How do I request a token or construct the authentication material?
- Where exactly does the token, key, signature, or certificate go in the API request?
- How do I know the request succeeded?
- What should I do when I receive a `401`?
- What should I do when I receive a `403`?
- How long does the token last?
- Should I cache the token?
- How do scopes or permissions affect API access?
- How should I protect and rotate credentials?
- Where do I go next?

If the page cannot answer these questions, it is probably incomplete.

## Future Improvements

Future versions of this pattern may include:

- Separate patterns for authorization and permission modeling
- OAuth-specific documentation patterns
- API key documentation patterns
- Webhook signature verification patterns
- Authentication diagrams using Mermaid
- OpenAPI security scheme examples
- SDK-specific authentication examples
- AI validation rules for authentication completeness
- Security review criteria for developer-facing documentation
- Public-client authentication patterns
- Token lifecycle and caching patterns
- Environment modeling patterns
- Credential rotation runbook patterns

## Summary

Authentication documentation should help developers make a successful authenticated request with confidence.

A strong authentication page explains the model, provides complete examples, describes permissions, addresses lifecycle behavior, and helps readers diagnose failures. It should be authoritative, discoverable, and reusable across the documentation system.

Authentication is not merely a header format or token endpoint. It is one of the first developer experience moments in an API integration.

In v2, the authentication pattern explicitly treats the page as both a capability reference and an onboarding guide. It preserves the original goal of centralizing authentication while adding practical guide elements: estimated time, learning objectives, “at a glance” summaries, flow selection, diagrams, token lifecycle, FAQ, common mistakes, and a final readiness checklist.
