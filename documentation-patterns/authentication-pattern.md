# Authentication Pattern

**Pattern type:** API Documentation  
**Status:** Draft  

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

## Problem Statement

Authentication is frequently documented either too narrowly or too abstractly.

Some documentation only describes the authentication scheme, such as OAuth 2.0, API keys, bearer tokens, mutual TLS, or signed requests. Other documentation explains individual credential fields but does not show how those fields are used in a complete request. In both cases, readers are left to infer the implementation path from fragments.

Authentication also tends to become fragmented across product areas. One API may document access tokens in one section, another may document API keys elsewhere, and a third may explain permissions or scopes in a separate onboarding guide. This forces developers to assemble the full picture themselves, often before they understand the platform well enough to know what they are looking for.

A good authentication page should not merely identify the authentication mechanism. It should explain the authentication model, provide complete examples, describe common failure modes, and link to the relevant authorization and security concepts.

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

## Recommended Structure

An authentication page should include the following sections.

### 1. Overview

Explain the authentication model in plain language.

The overview should state what method the API uses and what developers must provide with each request. Avoid beginning with protocol details before explaining the practical implementation path.

Example:

> Atlas Commerce APIs use bearer token authentication. To call an API, first request an access token using your client credentials, then include the token in the `Authorization` header of each request.

The overview should answer:

- What authentication method is used?
- Is authentication required for all endpoints?
- Are there different methods for different API surfaces?
- What is the shortest path to a successful authenticated request?

### 2. Authentication Model

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

The purpose of this section is conceptual clarity. Readers should understand the model before they copy a request.

For example, if tokens are issued to applications rather than users, say so explicitly. If sandbox and production use separate credentials, explain that before showing examples.

### 3. Prerequisites

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

Keep this section practical. A developer should be able to look at the list and know whether they are ready to proceed.

### 4. Get Credentials

Explain how credentials are obtained.

This section should describe the process at the right level of detail for the audience. If credentials are self-service, provide the steps. If credentials are issued during onboarding, explain what the reader should request and from whom.

Avoid documenting internal provisioning details that do not help the developer act. Instead, describe the external contract: what credentials exist, how they are delivered, how they are scoped, and how they differ by environment.

### 5. Request an Access Token

If the authentication model uses tokens, provide a complete token request.

Canonical examples are essential here. Developers should not have to infer headers, grant types, content types, or request bodies from prose alone.

Example:

```bash
curl -X POST "https://api.sandbox.atlas-commerce.example/oauth/token" \
  -H "Content-Type: application/json" \
  -d '{
    "clientId": "client_sandbox_12345",
    "clientSecret": "secret_sandbox_67890",
    "grantType": "client_credentials",
    "scope": "payments:read payments:write"
  }'
```

Example response:

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.example",
  "tokenType": "Bearer",
  "expiresIn": 3600,
  "scope": "payments:read payments:write"
}
```

The example should be complete enough that a developer can adapt it directly.

### 6. Make an Authenticated Request

Show exactly how to use the credential or token in an API request.

Example:

```bash
curl -X GET "https://api.sandbox.atlas-commerce.example/v1/payments/pay_12345" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.example" \
  -H "Content-Type: application/json"
```

This section is critical because developers often understand how to obtain a token but still fail when applying it to an actual request.

If the API uses API keys, signatures, certificates, or multiple headers, show the exact required format.

### 7. Scopes and Permissions

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
|-------|--------|-------------|
| `payments:read` | Read payment records | Retrieve payment status |
| `payments:write` | Create and update payments | Create a payment or refund |
| `customers:read` | Read customer records | Display customer details |
| `customers:write` | Create and update customers | Create customer profiles |

### 8. Token Expiration and Refresh

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

### 9. Environments

Clearly distinguish sandbox, staging, and production behavior.

Authentication errors frequently occur when developers mix credentials and base URLs. If sandbox credentials cannot be used in production, say so explicitly.

Example table:

| Environment | Base URL | Credentials |
|-------------|----------|-------------|
| Sandbox | `https://api.sandbox.atlas-commerce.example` | Sandbox credentials only |
| Production | `https://api.atlas-commerce.example` | Production credentials only |

Also document any differences in token lifetimes, scopes, approval workflows, test data, or credential provisioning.

### 10. Credential Rotation and Revocation

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

### 11. Error Responses

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

### 12. Security Guidance

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

### 13. Troubleshooting

Include a focused troubleshooting section for common onboarding failures.

Common issues include:

| Symptom | Possible cause | Recommended action |
|---------|----------------|--------------------|
| `401 Unauthorized` | Missing, expired, or malformed token | Request a new token and verify the `Authorization` header format. |
| `403 Forbidden` | Token lacks required scope | Confirm that the application has the required scope. |
| Token request fails | Invalid client credentials | Verify that the client ID and secret match the environment. |
| Sandbox request fails in production | Environment mismatch | Use production credentials with the production base URL. |
| Signature validation fails | Incorrect signing string or timestamp | Recreate the signature using the documented canonical string. |

Troubleshooting sections should be based on real support patterns whenever possible.

### 14. Related Topics

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

Cross-linking helps authentication remain a single authoritative capability while still connecting it to implementation-specific guidance.

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

## Example Pattern Applied: Atlas Commerce

The following outline demonstrates how this pattern might be applied to a fictional platform.

```text
Authentication

Overview
  Atlas Commerce APIs use OAuth 2.0 bearer token authentication.

Authentication model
  Applications authenticate using client credentials.
  Access tokens are issued per environment.
  Scopes determine which API operations the application can perform.

Before you begin
  Create a developer account.
  Register an application.
  Request sandbox credentials.

Get an access token
  POST /oauth/token

Make an authenticated request
  GET /v1/payments/{paymentId}

Scopes
  payments:read
  payments:write
  customers:read
  customers:write

Token expiration
  Access tokens expire after one hour.

Errors
  401 invalid_token
  403 insufficient_scope

Troubleshooting
  Environment mismatch
  Expired token
  Missing scope
  Incorrect Authorization header
```

This structure gives developers both the conceptual model and the practical implementation path.

## Pattern Checklist

Use this checklist when reviewing authentication documentation.

- [ ] The page explains what authentication method is used.
- [ ] The page explains the authentication model in plain language.
- [ ] Prerequisites are listed before implementation steps.
- [ ] Credential acquisition is explained.
- [ ] Complete request and response examples are provided.
- [ ] At least one authenticated API request is shown.
- [ ] Scopes or permissions are explained in task-based language.
- [ ] Environment-specific behavior is documented.
- [ ] Token expiration, refresh, rotation, or revocation is addressed when applicable.
- [ ] Authentication errors include status codes and example payloads.
- [ ] Troubleshooting guidance addresses common onboarding failures.
- [ ] Related topics are linked.
- [ ] Internal-only implementation details are excluded.
- [ ] Authentication content is not duplicated unnecessarily across product pages.

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

## Summary

Authentication documentation should help developers make a successful authenticated request with confidence.

A strong authentication page explains the model, provides complete examples, describes permissions, addresses lifecycle behavior, and helps readers diagnose failures. It should be authoritative, discoverable, and reusable across the documentation system.

Authentication is not merely a header format or token endpoint.

It is one of the first developer experience moments in an API integration.
