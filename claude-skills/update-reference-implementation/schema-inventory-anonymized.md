# Schema Inventory

> Working draft generated from two legacy OAS files plus canonical JSON payload examples. This is a documentation reference inventory, not an official architecture-owned specification.

Generated: 2026-06-18 13:44 UTC

## Source Files Analyzed

- `-HPXeCommerceDomain-1.1-domain.yaml`
- `reference-oas-a.yaml`
- `canonical-json-payloads-core(1).md`
- `canonical-json-payloads-tokenization-variants(1).md`
- `canonical-json-payloads-wallets(1).md`
- `canonical-json-payloads-3ds(1).md`
- `canonical-json-payloads-lifecycle(1).md`
- `canonical-json-payloads-optional-other(1).md`

## Important Caveats

- The uploaded 1.2.0 OAS references external domain schemas that were not included, including LegacyEngine transaction and WorkflowEngine operation schemas. Those unresolved references mean many `transaction.*` and `operation.*` fields are only available from canonical payloads.
- The inventory tracks observed fields, inferred types, source provenance, and confidence. It should be reviewed by Product/Engineering before being treated as a formal schema source.
- `JSON-only` does not mean invalid. It means the field appears in the canonical payload corpus but was not found in the parsed legacy OAS files.
- `OAS-only` does not mean current. It means the field appears in one of the legacy OAS files but was not observed in the canonical payload corpus.

## Endpoint Inventory from 1.2.0 OAS

| Method | Path | Summary | Source |
|---|---|---|---|
| POST | `/config` | Return configuration data including Payment Options enabled for the Enterprise | `reference-oas-a.yaml` |
| POST | `/key` | Return the RSA public key to use to secure transaction data if encryption is done in merchant code | `reference-oas-a.yaml` |
| POST | `/transaction` | Process a transaction through the  platform | `reference-oas-a.yaml` |
| POST | `/transaction/reverse` | Reverse a transaction through the  platform | `reference-oas-a.yaml` |
| POST | `/transaction/introspect` | Get the status of a recent transaction using a valid context id | `reference-oas-a.yaml` |
| POST | `/3ds/progress` | Support for a Secure Customer Authentication operation | `reference-oas-a.yaml` |

## High-Level Source Coverage

- Total unique canonical JSON paths: **121**
- Total unique parsed OAS paths: **289**
- Paths appearing in both canonical JSON and parsed OAS: **8**
- JSON-only paths: **113**
- OAS-only paths: **281**

The low overlap is expected because the legacy OAS files reference external domain schemas that were not included in the upload.

## Key Model Drift / Review Areas

| Area | Why It Matters |
|---|---|
| commerce.threeDS vs commerce.threeDSOptions | Canonical payloads use commerce.threeDS.* for enabled/channel/result/required, while the commerce Domain OAS exposes commerce.threeDSOptions.*. Treat as a probable schema/model drift area. |
| commerce.returnUrl / cancelUrl / redirectUrl | Canonical APM and browser 3DS examples use commerce.returnUrl, commerce.cancelUrl, and commerce.redirectUrl; these are not directly present in the parsed legacy OAS field inventory. |
| transaction.* model mostly unresolved in OAS | The 1.2.0 OAS references an external LegacyEngineDomain transaction schema that is not present in the uploaded files, so most transaction.* fields are inferred from canonical payloads. |
| operation.* model mostly unresolved in OAS | The 1.2.0 OAS references an external WorkflowEngineDomain operation schema that is not present in the uploaded files, so operation.* fields are mostly inferred from canonical payloads. |
| customer identity naming | Canonical payloads use consumerId and customerReferenceId, while the OAS uses merchantConsumerId and several deprecated customer profile fields. This needs human review. |

## Field Inventory by Container

### `operation`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `operation` | object | Canonical JSON, OAS 1.2.0 | High |  | Observed in: 1. Sale — New Card with RSA > Example Request; 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Request |
| `operation.contextId` | string | Canonical JSON | Medium — observed in canonical payloads only | <contextId> | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `operation.decision` | string | Canonical JSON | Medium — observed in canonical payloads only | REJECT; INTERRUPT; ERROR | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `operation.nonAcceptedServices` | array | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `operation.nonAcceptedServices[]` | array | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `operation.posSyncId` | string | Canonical JSON | Medium — observed in canonical payloads only | <posSyncId> | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `operation.reasonCode` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `operation.reasonCode.code` | string | Canonical JSON | Medium — observed in canonical payloads only | <reasonCode>; 900 | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `operation.reasonCode.description` | string | Canonical JSON | Medium — observed in canonical payloads only | Additional customer action required; Challenge cancelled by customer; Additional customer authentication required | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |

### `authentication`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `authentication` | object | Canonical JSON, OAS 1.2.0 | High |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `authentication.applicationId` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The -assigned value that uniquely identifies the merchant organization. |
| `authentication.locationReference` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The merchant value that represents a StoreId and TerminalId on the  side. Not required if passing StoreId and TerminalId. If locationReference is provided, those values will be resolved automatically. |
| `authentication.locationId` | string | Canonical JSON, OAS 1.2.0 | High | <locationId> | The  public StoreId assigned to the merchant. Not required if passing locationReference |
| `authentication.deviceId` | string | Canonical JSON, OAS 1.2.0 | High | <deviceId> | The  public TerminalId assigned to the merchant. Not required if passing locationReference |

### `customer`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `customer` | object | Canonical JSON, OAS 1.2.0 | High |  | Observed in: 3. Returning User with PMT from  CDS > Example Request; 4. Returning User with Merchant-Managed PMT > Example Request; Optional Features > 2. AVS / CVV Sale Request |
| `customer.avsMatchThreshold` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'customer.profile.billingInformation.avsMatchThreshold' instead. |
| `customer.billingAddress` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `customer.billingAddress.address1` | string | Canonical JSON | Medium — observed in canonical payloads only | <addressLine1> | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `customer.billingAddress.city` | string | Canonical JSON | Medium — observed in canonical payloads only | <city> | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `customer.billingAddress.country` | string | Canonical JSON | Medium — observed in canonical payloads only | US | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `customer.billingAddress.firstName` | string | Canonical JSON | Medium — observed in canonical payloads only | <firstName> | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `customer.billingAddress.lastName` | string | Canonical JSON | Medium — observed in canonical payloads only | <lastName> | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `customer.billingAddress.postalCode` | string | Canonical JSON | Medium — observed in canonical payloads only | <postalCode> | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `customer.billingAddress.state` | string | Canonical JSON | Medium — observed in canonical payloads only | <state> | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `customer.consumerId` | string | Canonical JSON | Medium — observed in canonical payloads only | <consumerId> | Observed in: 3. Returning User with PMT from  CDS > Example Request |
| `customer.createCardOnFileIfNotFound` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'customer.profile.account.save' instead. |
| `customer.createConsumer` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | If a Consumer Profile is not found, automatically create a profile. Defaults to false. |
| `customer.createConsumerProfileIfNotFound` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'customer.createConsumer' instead. |
| `customer.email` | string | Canonical JSON | Medium — observed in canonical payloads only | <emailAddress> | Observed in: Optional Features > 4. Fraud Check Request |
| `customer.merchantConsumerId` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The merchant-facing value used to uniquely identify a Consumer |
| `customer.customerReferenceId` | string | Canonical JSON | Medium — observed in canonical payloads only | <customerReferenceId> | Observed in: 4. Returning User with Merchant-Managed PMT > Example Request |
| `customer.profile` | object | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `customer.profile.account` | object | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `customer.profile.account.return` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Returns the Token from the specified Consumer Profile that matches the CardIssuer passed in. |
| `customer.profile.account.save` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | If the specified Consumer Profile is found, but an associated card on file is not, the specified token will automatically be added as a card on file. |
| `customer.profile.account.updateExpirationDate` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | If true, LegacyEngine will use the card expiration dates passed in the request instead of the exp dates stored in CardStor but only if it is newer than CardStor. will then update the CardStor with these dates. |
| `customer.profile.account.use` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Use the Token found on the Consumer's Profile that matches the CardIssuer passed into |
| `customer.profile.accounts` | array | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `customer.profile.accounts[].account` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The Token from the Consumer's Profile which matches the CardIssuer from the request. |
| `customer.profile.accounts[].type` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The account type for this account. Will match card issuer that was passed in. |
| `customer.profile.billingInformation` | object | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `customer.profile.billingInformation.avsMatchThreshold` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only | enum: Strict, Partial, None | Defines how closely the Billing Address must match an entry within the Address Verification Service. |
| `customer.profile.billingInformation.stopNotFound` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | If true and a Consumer Profile Billing Address is not found, stop processing and return error, else continue processing with the merchant-provided information or null if it does not exist. |
| `customer.profile.billingInformation.update` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Update the Billing Address information in the Consumer Data Service. Requires MerchantConsumerId to be passed. |
| `customer.profile.billingInformation.use` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Use the Billing Address information from the Consumer Data Service if one is found. Requires MerchantConsumerId, Payment Token, and CVV to be passed. |
| `customer.refreshBillingAddressOnConsumerProfile` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'customer.profile.billingInformation.update' instead. |
| `customer.returnCardOnFileToken` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'customer.profile.account.return' instead. |
| `customer.stopOnConsumerProfileBillingAddressNotFound` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'customer.profile.billingInformation.stopNotFound' instead. |
| `customer.stopOnRefreshBillingAddressOnConsumerProfileFailure` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | This field is deprecated and has no effect. Values provided will be ignored. |
| `customer.updateCardExpirationDate` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'customer.profile.card.updateExpirationDate' instead. |
| `customer.useConsumerProfileBillingAddress` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'customer.profile.billingInformation.use' instead. |
| `customer.useConsumerProfileTokenForPayment` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'customer.profile.account.use' instead. |

### `transaction`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `transaction` | object | Canonical JSON, OAS 1.2.0 | High |  | Observed in: 1. Sale — New Card with RSA > Example Request; 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Request |
| `transaction.autoRefundOnVoidFailure` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Denotes an optional fallback behavior. Perform a Refund when a Void request fails due to batch timing. |
| `transaction.cardInformation` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.cardInformation.brandCode` | string | Canonical JSON | Medium — observed in canonical payloads only | VS | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.cardInformation.maskedPan` | string | Canonical JSON | Medium — observed in canonical payloads only | 411111xxxxxx1111 | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.cardInformation.network` | string | Canonical JSON | Medium — observed in canonical payloads only | FREEWAY; VISA | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.cardInformation.type` | string | Canonical JSON | Medium — observed in canonical payloads only | credit | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.clientContext` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.clientContext.sellingMiddlewareName` | string | Canonical JSON | Medium — observed in canonical payloads only | <middlewareName> | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.clientContext.sellingMiddlewareVersion` | string | Canonical JSON | Medium — observed in canonical payloads only | <middlewareVersion> | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.clientContext.sellingSystemName` | string | Canonical JSON | Medium — observed in canonical payloads only | <systemName> | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.clientContext.sellingSystemVersion` | string | Canonical JSON | Medium — observed in canonical payloads only | <systemVersion> | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.fallbackAction` | string | Canonical JSON | Medium — observed in canonical payloads only | refund | Observed in: Other Transaction Operations > 9. Refund Fallback from Failed Void Response |
| `transaction.invoice` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.invoice.invoiceHeader` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.invoice.invoiceHeader.invoiceNumber` | string | Canonical JSON | Medium — observed in canonical payloads only | <invoiceNumber> | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.invoice.purchaseTotals` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.invoice.purchaseTotals.chargeAmount` | number | Canonical JSON | Medium — observed in canonical payloads only | 6.0; 5.01; 0.0 | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.invoice.purchaseTotals.currency` | string | Canonical JSON | Medium — observed in canonical payloads only | USD | Observed in: Optional Features > 1. DCC Request |
| `transaction.invoice.purchaseTotals.taxTotal` | number | Canonical JSON | Medium — observed in canonical payloads only | 0.41 | Observed in: 6. Refund — via Original Request ID > Example Request |
| `transaction.referenceId` | string | Canonical JSON | Medium — observed in canonical payloads only | <referenceId> | Observed in: 1. Sale — New Card with RSA > Example Request; 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Request |
| `transaction.networkData` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 6. Refund — via Original Request ID > Example Response; 7. Refund — via Payment Token > Example Response; 1. Apple Pay Sale > Example Response |
| `transaction.networkData.network` | string | Canonical JSON | Medium — observed in canonical payloads only | FREEWAY | Observed in: 6. Refund — via Original Request ID > Example Response; 7. Refund — via Payment Token > Example Response; 1. Apple Pay Sale > Example Response |
| `transaction.relatedRequestId` | string | Canonical JSON | Medium — observed in canonical payloads only | <originalRequestId>; <authorizationRequestId> | Observed in: 4. Capture — Prior Authorization > Example Request; 5. Void — Prior Transaction > Example Request; 6. Refund — via Original Request ID > Example Request |
| `transaction.originalAction` | string | Canonical JSON | Medium — observed in canonical payloads only | void | Observed in: Other Transaction Operations > 9. Refund Fallback from Failed Void Response |
| `transaction.response` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.response.decision` | string | Canonical JSON | Medium — observed in canonical payloads only | REJECT; ACCEPT | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.response.reasonCode` | string | Canonical JSON | Medium — observed in canonical payloads only | 100; <reasonCode> | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.response.requestId` | string | Canonical JSON | Medium — observed in canonical payloads only | <voidRequestId>; <captureRequestId>; <refundRequestId> | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.serviceReplies` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.serviceReplies.dccReply` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: Optional Features > DCC Response |
| `transaction.serviceReplies.dccReply.accepted` | boolean | Canonical JSON | Medium — observed in canonical payloads only | True | Observed in: Optional Features > DCC Response |
| `transaction.serviceReplies.dccReply.exchangeRate` | number | Canonical JSON | Medium — observed in canonical payloads only | 0.925 | Observed in: Optional Features > DCC Response |
| `transaction.serviceReplies.dccReply.exchangeRateSource` | string | Canonical JSON | Medium — observed in canonical payloads only | <exchangeRateSource> | Observed in: Optional Features > DCC Response |
| `transaction.serviceReplies.dccReply.foreignAmount` | number | Canonical JSON | Medium — observed in canonical payloads only | 9.25 | Observed in: Optional Features > DCC Response |
| `transaction.serviceReplies.dccReply.foreignCurrency` | string | Canonical JSON | Medium — observed in canonical payloads only | EUR | Observed in: Optional Features > DCC Response |
| `transaction.serviceReplies.dccReply.reasonCode` | string | Canonical JSON | Medium — observed in canonical payloads only | 100 | Observed in: Optional Features > DCC Response |
| `transaction.serviceReplies.fraudCheckReply` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: Optional Features > Fraud Check Response |
| `transaction.serviceReplies.fraudCheckReply.decision` | string | Canonical JSON | Medium — observed in canonical payloads only | ACCEPT | Observed in: Optional Features > Fraud Check Response |
| `transaction.serviceReplies.fraudCheckReply.processorResponseCode` | string | Canonical JSON | Medium — observed in canonical payloads only | A | Observed in: Optional Features > Fraud Check Response |
| `transaction.serviceReplies.fraudCheckReply.reasonCode` | string | Canonical JSON | Medium — observed in canonical payloads only | 100 | Observed in: Optional Features > Fraud Check Response |
| `transaction.serviceReplies.tokenCreateReply` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response; 1. Sale with Token Creation > Example Response; 2. Token-Only Verification without Sale > Example Response |
| `transaction.serviceReplies.tokenCreateReply.storedToken` | string | Canonical JSON | Medium — observed in canonical payloads only | <storedToken> | Observed in: 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response; 1. Sale with Token Creation > Example Response; 2. Token-Only Verification without Sale > Example Response |
| `transaction.serviceReplies.tokenCreateReply.reasonCode` | string | Canonical JSON | Medium — observed in canonical payloads only | 100 | Observed in: 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response; 1. Sale with Token Creation > Example Response; 2. Token-Only Verification without Sale > Example Response |
| `transaction.serviceReplies.transactionReply` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.serviceReplies.transactionReply.accountBalance2Currency` | string | Canonical JSON | Medium — observed in canonical payloads only | USD | Observed in: 7. Refund — via Payment Token > Example Response |
| `transaction.serviceReplies.transactionReply.amount` | number | Canonical JSON | Medium — observed in canonical payloads only | 6.0; 5.01; 0.0 | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.serviceReplies.transactionReply.authorizationCode` | string | Canonical JSON | Medium — observed in canonical payloads only | <authorizationCode> | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.serviceReplies.transactionReply.avsCode` | string | Canonical JSON | Medium — observed in canonical payloads only | M | Observed in: Optional Features > AVS / CVV Sale Response; Optional Features > Address Verification / ZVAV with AVS Fields Response |
| `transaction.serviceReplies.transactionReply.cvCode` | string | Canonical JSON | Medium — observed in canonical payloads only | M | Observed in: Optional Features > AVS / CVV Sale Response; Optional Features > Address Verification / ZVAV with AVS Fields Response |
| `transaction.serviceReplies.transactionReply.partialAmount` | boolean | Canonical JSON | Medium — observed in canonical payloads only | False | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.serviceReplies.transactionReply.processorResponseCode` | string | Canonical JSON | Medium — observed in canonical payloads only | 100 | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.serviceReplies.transactionReply.processorResponseMessage` | string | Canonical JSON | Medium — observed in canonical payloads only | APPROVED | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.serviceReplies.transactionReply.processorTransactionId` | string | Canonical JSON | Medium — observed in canonical payloads only | <processorTransactionId> | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.serviceReplies.transactionReply.purchasingLevel3Enabled` | boolean | Canonical JSON | Medium — observed in canonical payloads only | False | Observed in: 6. Refund — via Original Request ID > Example Response; 7. Refund — via Payment Token > Example Response |
| `transaction.serviceReplies.transactionReply.reasonCode` | string | Canonical JSON | Medium — observed in canonical payloads only | 100 | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.serviceReplies.transactionReply.reconciliationId` | string | Canonical JSON | Medium — observed in canonical payloads only | <reconciliationId> | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.serviceReplies.transactionReply.requestDateTime` | string | Canonical JSON | Medium — observed in canonical payloads only | <requestDateTime> | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `transaction.services` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.services.dccService` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: Optional Features > 1. DCC Request |
| `transaction.services.dccService.run` | boolean | Canonical JSON | Medium — observed in canonical payloads only | True | Observed in: Optional Features > 1. DCC Request |
| `transaction.services.fraudCheckService` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: Optional Features > 4. Fraud Check Request |
| `transaction.services.fraudCheckService.run` | boolean | Canonical JSON | Medium — observed in canonical payloads only | True | Observed in: Optional Features > 4. Fraud Check Request |
| `transaction.services.tokenCreateService` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request; 1. Sale with Token Creation > Example Request; 2. Token-Only Verification without Sale > Example Request |
| `transaction.services.tokenCreateService.run` | boolean | Canonical JSON | Medium — observed in canonical payloads only | True | Observed in: 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request; 1. Sale with Token Creation > Example Request; 2. Token-Only Verification without Sale > Example Request |
| `transaction.services.transactionService` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.services.transactionService.commerceIndicator` | string | Canonical JSON | Medium — observed in canonical payloads only | internet | Observed in: 6. Refund — via Original Request ID > Example Request; Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request |
| `transaction.services.transactionService.transactionType` | string | Canonical JSON | Medium — observed in canonical payloads only | capture; authorization; void | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.tenders` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.tenders.alternativePayment` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 3. PayPal / Redirect APM Sale > Example Request; 4. ATH Móvil Sale > Example Request; 1. APM Sale Request |
| `transaction.tenders.alternativePayment.paymentMethod` | string | Canonical JSON | Medium — observed in canonical payloads only | paypal; athmovil; <paymentMethod> | Observed in: 3. PayPal / Redirect APM Sale > Example Request; 4. ATH Móvil Sale > Example Request; 1. APM Sale Request |
| `transaction.tenders.card` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `transaction.tenders.card.accountNumber` | string | Canonical JSON | Medium — observed in canonical payloads only | <dpan>; <storedToken>; <mpan> | Observed in: 2. Sale — Existing Payment Token > Example Request; 7. Refund — via Payment Token > Example Request; 3. Returning User with PMT from  CDS > Example Request |
| `transaction.tenders.card.cardType` | string | Canonical JSON | Medium — observed in canonical payloads only | token | Observed in: 2. Sale — Existing Payment Token > Example Request; 7. Refund — via Payment Token > Example Request; 3. Returning User with PMT from  CDS > Example Request |
| `transaction.tenders.card.encryptedData` | string | Canonical JSON | Medium — observed in canonical payloads only | <applePayPaymentToken>; <googlePayPaymentToken>; <encryptedCardData> | Observed in: 1. Sale — New Card with RSA > Example Request; 3. Authorization — New Card with RSA > Example Request; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request |
| `transaction.tenders.pos` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 3. Authorization — New Card with RSA > Example Request; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request |
| `transaction.tenders.pos.encMode` | string | Canonical JSON | Medium — observed in canonical payloads only | googlewallet; rsa; applewallet | Observed in: 1. Sale — New Card with RSA > Example Request; 3. Authorization — New Card with RSA > Example Request; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request |
| `transaction.tenders.pos.entryMode` | string | Canonical JSON | Medium — observed in canonical payloads only | keyed; inapp | Observed in: 1. Sale — New Card with RSA > Example Request; 3. Authorization — New Card with RSA > Example Request; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request |
| `transaction.tenders.pos.msrType` | string | Canonical JSON | Medium — observed in canonical payloads only | applepay; none; googlepay | Observed in: 1. Sale — New Card with RSA > Example Request; 3. Authorization — New Card with RSA > Example Request; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request |
| `transaction.tenders.pos.trackKsn` | string | Canonical JSON | Medium — observed in canonical payloads only | <trackKsn> | Observed in: 1. Sale — New Card with RSA > Example Request; 3. Authorization — New Card with RSA > Example Request; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request |
| `transaction.tokenInformation` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 2. Sale — Existing Payment Token > Example Response; 7. Refund — via Payment Token > Example Response; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response |
| `transaction.tokenInformation.accountNumberMasked` | string | Canonical JSON | Medium — observed in canonical payloads only | 411111xxxxxx1111 | Observed in: 2. Sale — Existing Payment Token > Example Response; 7. Refund — via Payment Token > Example Response; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response |
| `transaction.tokenInformation.brand` | string | Canonical JSON | Medium — observed in canonical payloads only | VS | Observed in: 2. Sale — Existing Payment Token > Example Response; 7. Refund — via Payment Token > Example Response; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response |
| `transaction.tokenInformation.cardExpirationMonth` | integer | Canonical JSON | Medium — observed in canonical payloads only | 11 | Observed in: 2. Sale — Existing Payment Token > Example Response; 7. Refund — via Payment Token > Example Response; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response |
| `transaction.tokenInformation.cardExpirationYear` | integer | Canonical JSON | Medium — observed in canonical payloads only | 29 | Observed in: 2. Sale — Existing Payment Token > Example Response; 7. Refund — via Payment Token > Example Response; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response |
| `transaction.tokenInformation.cardType` | string | Canonical JSON | Medium — observed in canonical payloads only | credit | Observed in: 2. Sale — Existing Payment Token > Example Response; 7. Refund — via Payment Token > Example Response; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response |

### `commerce`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `commerce` | object | Canonical JSON, OAS 1.2.0 | High |  | Observed in: 6. Refund — via Original Request ID > Example Response; 7. Refund — via Payment Token > Example Response; 3. PayPal / Redirect APM Sale > Example Request |
| `commerce.apmOptions` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.apmOptions.qrCodeHeight` | integer | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Height of the QRCode image |
| `commerce.apmOptions.qrCodeWidth` | integer | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Width of the QRCode image |
| `commerce.apmOptions.returnQrCodeImage` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Apm Url is always returned when applicable. If a scannable QRCode is desired, pass true for this. |
| `commerce.apmOptions.thirdPartyIdentifier` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | A unique identifier required by some third parties to uniquely identify a transaction session |
| `commerce.apmQrCode` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Base64 representation of the QrCode in png format |
| `commerce.apmUrl` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Url to the Apm transaction system |
| `commerce.cancelUrl` | string | Canonical JSON | Medium — observed in canonical payloads only | <cancelUrl> | Observed in: 3. PayPal / Redirect APM Sale > Example Request; 4. ATH Móvil Sale > Example Request; 5. Browser 3DS — Initial Sale Request > Example Request |
| `commerce.cardIssuer` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only | enum: alipay, athmovilecom, athmovilinstore, citidloc, citimil, cnh, klarna, openbanking, paypal, venmo | Identifies the desired transaction method when it cannot be derived by bin check. Applies primarily to Alternate Payment Methods and some private label accounts. |
| `commerce.clientSessionKey` | string | Canonical JSON | Medium — observed in canonical payloads only | <clientSessionKey> | Observed in: 6. Refund — via Original Request ID > Example Response; 7. Refund — via Payment Token > Example Response; 1. Mobile 3DS — Initial Sale Request > Example Response — 3DS Required |
| `commerce.reasonCode` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Provides additional information for the commerce portion of the operation. Values in the range of 900-999. |
| `commerce.reasonCode.code` | integer | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Numerical representation of the ReasonCode |
| `commerce.reasonCode.description` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Textual representation of the ReasonCode |
| `commerce.redirectUrl` | string | Canonical JSON | Medium — observed in canonical payloads only | <redirectUrl> | Observed in: 3. PayPal / Redirect APM Sale > Example Response — Redirect Required; 4. ATH Móvil Sale > Example Response — Redirect Required; 5. Browser 3DS — Initial Sale Request > Example Response — Browser Challenge Required |
| `commerce.returnStatus` | string | Canonical JSON | Medium — observed in canonical payloads only | cancelled; expired | Observed in: 4. APM Response — Customer Cancelled; 5. APM Response — Payment Expired; 7. APM Full Webhook Callback — Cancelled Payment |
| `commerce.returnUrl` | string | Canonical JSON | Medium — observed in canonical payloads only | <returnUrl> | Observed in: 3. PayPal / Redirect APM Sale > Example Request; 4. ATH Móvil Sale > Example Request; 5. Browser 3DS — Initial Sale Request > Example Request |
| `commerce.serviceProviderCodes` | array | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.serviceProviderCodes[].serviceProviderCode` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Code returned by the third party |
| `commerce.serviceProviderCodes[].serviceProviderMessage` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | The message provided by the third party |
| `commerce.serviceProviderCodes[].serviceProviderName` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | The Third Party Provider |
| `commerce.threeDS` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Mobile 3DS — Initial Sale Request > Example Request; 1. Mobile 3DS — Initial Sale Request > Example Response — 3DS Required; 2. Mobile 3DS — Finalization Request After Successful Challenge > Example Request |
| `commerce.threeDS.channel` | string | Canonical JSON | Medium — observed in canonical payloads only | mobile; browser | Observed in: 1. Mobile 3DS — Initial Sale Request > Example Request; 1. Mobile 3DS — Initial Sale Request > Example Response — 3DS Required; 5. Browser 3DS — Initial Sale Request > Example Request |
| `commerce.threeDS.enabled` | boolean | Canonical JSON | Medium — observed in canonical payloads only | True | Observed in: 1. Mobile 3DS — Initial Sale Request > Example Request; 5. Browser 3DS — Initial Sale Request > Example Request |
| `commerce.threeDS.required` | boolean | Canonical JSON | Medium — observed in canonical payloads only | True; False | Observed in: 1. Mobile 3DS — Initial Sale Request > Example Response — 3DS Required; 4. Mobile 3DS — Frictionless / Challenge Not Needed > Example Response; 5. Browser 3DS — Initial Sale Request > Example Response — Browser Challenge Requir… |
| `commerce.threeDS.result` | string | Canonical JSON | Medium — observed in canonical payloads only | challengeSuccessful; errorAtThreeDSProvider; challengeCancelledByCustomer | Observed in: 2. Mobile 3DS — Finalization Request After Successful Challenge > Example Request; 3. Mobile 3DS — Finalization Request After Challenge Failed > Example Request; 4. Mobile 3DS — Frictionless / Challenge Not Needed > Example Res… |
| `commerce.threeDSOptions` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSOptions.account` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSOptions.account.ageIndicator` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Length of time that the cardholder had an account with the 3DS requestor. Options 01, 02, 03, 04 and 05. |
| `commerce.threeDSOptions.account.changeIndicator` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Length of time since the cardholder's account information with the 3DS Requestor was last changed, including Billing or Shipping address, new transaction account, or new user added. Options 01, 02, 03 and 04. |
| `commerce.threeDSOptions.account.changedDate` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | The last changed date of the cardholder's account with the 3DS Requestor, including Billing or Shipping address, new transaction account, or new user added. Format YYYYMMDD. |
| `commerce.threeDSOptions.account.dateOpened` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Date that the cardholder opened the account with the 3DS Requestor. Format YYYYMMDD |
| `commerce.threeDSOptions.account.identifier` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Additional information about the account that is optionally provided by the 3DS Requestor. |
| `commerce.threeDSOptions.account.passwordChangeIndicator` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Length of time since the cardholder's account with the 3DS Requestor had a password change or account reset. Options 01,02,03, 04 and 05. |
| `commerce.threeDSOptions.account.passwordChangedDate` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Date that cardholder's account with the 3DS Requestor had a password change or account reset. Format YYYYMMDD. |
| `commerce.threeDSOptions.account.paymentAccountAgeDate` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Date on which the transaction account was enrolled in the cardholder's account with the 3DS Requestor. Format YYYYMMDD. |
| `commerce.threeDSOptions.account.paymentAccountIndicator` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Indicates the length of time that the transaction account was enrolled in the cardholder's account with the 3DS Requestor. Options 01, 02, 03, 04 and 05. |
| `commerce.threeDSOptions.account.provisionAttemptsDay` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Number of Add Card attempts in the last 24 hours. |
| `commerce.threeDSOptions.account.purchasesInLastSixMonths` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Number of purchases with the cardholder's account during the previous six months. |
| `commerce.threeDSOptions.account.shippingAddressUsageDate` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Indicates the date when the shipping address was first used with the 3DS Requestor for the transaction. Format YYYYMMDD. |
| `commerce.threeDSOptions.account.shippingAddressUsageIndicator` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Indicates when the shipping address used for this transaction was first used with the 3DS Requestor. Options 01, 02, 03 and 04. |
| `commerce.threeDSOptions.account.shippingNameIndicator` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Indicates if the cardholder's name on the account is identical to the shipping name used for this transaction. Options 01=Same, 02=Different. |
| `commerce.threeDSOptions.account.suspiciousAccountActivityIndicator` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Suspicious activity (including previous fraud occurrence) indicator on the cardholder account. Options 01=No Suspicious Activity, 02=Suspicious Activity. |
| `commerce.threeDSOptions.account.txnActivityDay` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Number of transactions for the cardholder's account with the 3DS Requestor across all transaction accounts in the previous 24 hours. |
| `commerce.threeDSOptions.account.txnActivityYear` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Number of transactions for the cardholder's account with the 3DS Requestor across all transaction accounts in the previous year. |
| `commerce.threeDSOptions.account.type` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Indicates the type of account. For example, for a multi-account card product. |
| `commerce.threeDSOptions.addressesMustMatch` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Indicates that the Bill-To and Ship-To addresses are identical |
| `commerce.threeDSOptions.authenticationIndicator` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Indicates the type of Authentication request. This data element provides additional information to the ACS to determine the best approach for handling an authentication request. |
| `commerce.threeDSOptions.challengeIndicator` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Indicates whether a challenge is requested for this transaction. For example, for 01-PA, a 3DS Requestor may have concerns about the transaction, and request a challenge. For 02-NPA, a challenge may be necessary when adding a new card to a … |
| `commerce.threeDSOptions.challengeWindowSize` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Required if generateCReq is present |
| `commerce.threeDSOptions.clientType` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only | enum: browserJs, mobileSdk |  |
| `commerce.threeDSOptions.merchantDomainName` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | The domain name of the merchant website, this will help to handle the CORS issue in three DS flow |
| `commerce.threeDSOptions.mobileSdkRenderOptions` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSOptions.mobileSdkRenderOptions.interface` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Lists all the SDK Interface types that the clientDevice supports for displaying specific challenge user interfaces within the SDK. Values accepted are 01=Native, 02=HTML, 03=Both |
| `commerce.threeDSOptions.mobileSdkRenderOptions.uiType` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Lists all the UI types that the clientDevice supports for displaying specific challenge user interfaces within the SDK. Values accepted are Native UI=01-04; HTML UI=01-05. |
| `commerce.threeDSOptions.purchaseInstallData` | integer | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Indicates the maximum number of authorizations permitted for installment payments. |
| `commerce.threeDSOptions.recurrence` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSOptions.recurrence.expirationDate` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Date after which no further authorizations will be performed. Format YYYYMMDD. |
| `commerce.threeDSOptions.recurrence.frequency` | integer | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Indicates the minimum number of days between authorizations. |
| `commerce.threeDSOptions.resultDrivenProcessing` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | This will instruct the what actions needed to be taken once the 3ds outcome has received |
| `commerce.threeDSOptions.resultDrivenProcessing.enabled` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Default true, If false, this will instruct the system that 3ds outcome will have no impact on the transaction processing |
| `commerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | This will instruct how to process specific 3ds outcome |
| `commerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSAuthenticationAttempted` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | ??? |
| `commerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSAuthenticationFailed` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Default false, if true, the transaction processing will continue even when the 3ds authentication faild. |
| `commerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSAuthenticationSuccessful` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Default true, If true, this will process transaction if 3ds authentication was successful |
| `commerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSCardNotSupported` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Default false, if true, the transaction processing will continue even when the card doesn't support 3ds. |
| `commerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSRejected` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Default false, if true, the transaction processing will continue even when the 3ds is rejected. |
| `commerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSUnavailable` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Default false, if true, the transaction processing will continue even when the 3ds authentication was unavailable. |
| `commerce.threeDSOptions.runThreeDsAuthentication` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Default true, this indicates if the customer wants to run the 3DS authentication or not. This will override the store config value |
| `commerce.threeDSOptions.transactionType` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Identifies the type of transaction being authenticated. This field is required for VISA transactions and accepted values are 01, 03, 10, 11, and 28. |
| `commerce.threeDSProviderResponse` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.acs` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.acs.challengeMandated` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Indication of whether a challenge is required for the transaction to be authorised due to local or regional mandates or other variable. |
| `commerce.threeDSProviderResponse.acs.operatorIdentifer` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | DS-assigned ACS identifier. Each DS can provide a unique ID to each ACS. |
| `commerce.threeDSProviderResponse.acs.referenceNumber` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Unique identifier assigned by the EMVCo Secretariat upon testing and approval. |
| `commerce.threeDSProviderResponse.acs.renderingType` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.acs.renderingType.acsInterface` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Identifies the ACS interface that presents the challenge to the cardholder. Options are 01, 02. |
| `commerce.threeDSProviderResponse.acs.renderingType.acsUiTemplate` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Identifies the UI Template format that the ACS presents to the customer first. Options are 01, 02, 03, 04, 05. |
| `commerce.threeDSProviderResponse.acs.transactionId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Universally Unique transaction identifier assigned by the ACS to identify a single transaction. Format uuid. |
| `commerce.threeDSProviderResponse.acs.url` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Fully-qualified URL of the ACS to be used for the challenge. |
| `commerce.threeDSProviderResponse.authenticationValue` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.cardHolderInformation` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.challengeCancelledIndicator` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.dsTransactionId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.eci` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.error` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.error.code` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.error.component` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.error.description` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.interactionCounter` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.message` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.message.version` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.sdk` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.sdk.transactionId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.threeDsResult` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.threeDsServerTransactionId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.transaction` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.transaction.status` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.transaction.statusReason` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.trustList` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.trustList.status` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `commerce.threeDSProviderResponse.trustList.statusSource` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |

### `pointOfSale`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `pointOfSale` | object | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |

### `clientDevice`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `clientDevice` | object | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `clientDevice.applicationName` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `clientDevice.id` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `clientDevice.proceedWithAuth` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `clientDevice.registrationId` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `clientDevice.requests` | array | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `clientDevice.responses` | array | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `clientDevice.type` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `clientDevice.version` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |

### `authenticationProgressRequest`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `authenticationProgressRequest.browserCharacteristics` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.browserCharacteristics.acceptHeader` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.browserCharacteristics.colorDepth` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.browserCharacteristics.ipAddress` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.browserCharacteristics.javaEnabled` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.browserCharacteristics.javascriptEnabled` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.browserCharacteristics.language` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.browserCharacteristics.screenHeight` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.browserCharacteristics.screenWidth` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.browserCharacteristics.timeZoneOffset` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.browserCharacteristics.userAgent` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.clientErrors` | array | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.clientErrors[].message` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.contextId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Uniquely identifies a unit of work. Provide when performing an asynchronous operation that spans multiple calls. |
| `authenticationProgressRequest.mobileSdkData` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.mobileSdkData.applicationId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.mobileSdkData.encData` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.mobileSdkData.maxTimeout` | integer | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Time in minutes for the entire 3DS operation including Auth/Challenge/etc. Minimum of 5 minutes, maximum of 60. |
| `authenticationProgressRequest.mobileSdkData.publicKey` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.mobileSdkData.publicKey.crv` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.mobileSdkData.publicKey.keyType` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.mobileSdkData.publicKey.x` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.mobileSdkData.publicKey.y` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.mobileSdkData.referenceNumber` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Identifies the vendor and version for the 3DS SDK that is integrated in a 3DS Requestor App, assigned by EMVCo when the 3DS SDK is approved. |
| `authenticationProgressRequest.mobileSdkData.transactionId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Universally unique transaction identifier assigned by the 3DS SDK to identify a single transaction. |
| `authenticationProgressRequest.trackKsn` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `authenticationProgressRequest.tracke` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |

### `threeDSProgressResponse`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `threeDSProgressResponse.challengeAcsUrl` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | The Url for the customer challenge if 3dsOptions.clientType is 'browserJs' |
| `threeDSProgressResponse.clientPollInterval` | integer | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | How many seconds between poll attempts |
| `threeDSProgressResponse.clientPollWindow` | integer | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | The maximum seconds to poll |
| `threeDSProgressResponse.merchantNotifiedOfTransactionResult` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Denotes that  invoked the Merchant callback endpoint |
| `threeDSProgressResponse.methodUrl` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | The Url for the clientDevice data collection if the 3dsOptions.ClientType is 'browserJs' |
| `threeDSProgressResponse.mobileSdkData` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.mobileSdkData.dsCaCertificate` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey.e` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey.kty` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey.n` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKeyId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.status` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only | enum: CardDataMissing, NotApplicable, DeviceDataRequired, ChallengeRequired, ChallengeNotNeeded, ErrorAt3dsProvider, ChallengeSuccessful, ChallengeFailed, ChallengeCancelledByCustomer | The status of the 3DS operation at the time it was polled |
| `threeDSProgressResponse.threeDSEncodedChallengeRequestData` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Base64 URL encoded CReq message, only returned for challenge transactions during browser flow |
| `threeDSProgressResponse.threeDSProvider` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.acs` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.acs.challengeMandated` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Indication of whether a challenge is required for the transaction to be authorised due to local or regional mandates or other variable. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.operatorIdentifer` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | DS-assigned ACS identifier. Each DS can provide a unique ID to each ACS. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.referenceNumber` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Unique identifier assigned by the EMVCo Secretariat upon testing and approval. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.renderingType` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.acs.renderingType.acsInterface` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Identifies the ACS interface that presents the challenge to the cardholder. Options are 01, 02. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.renderingType.acsUiTemplate` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Identifies the UI Template format that the ACS presents to the customer first. Options are 01, 02, 03, 04, 05. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.transactionId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Universally Unique transaction identifier assigned by the ACS to identify a single transaction. Format uuid. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.url` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Fully-qualified URL of the ACS to be used for the challenge. |
| `threeDSProgressResponse.threeDSProviderResponse.authenticationValue` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.cardHolderInformation` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.challengeCancelledIndicator` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.dsTransactionId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.eci` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.error` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.error.code` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.error.component` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.error.description` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.interactionCounter` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.message` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.message.version` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.sdk` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.sdk.transactionId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.threeDsResult` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.threeDsServerTransactionId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.transaction` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.transaction.status` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.transaction.statusReason` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.trustList` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.trustList.status` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.trustList.statusSource` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderTransactionId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | The provider specific transactionId that is the unique identifer for the 3ds session. |
| `threeDSProgressResponse.transactionComplete` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | The transaction has been processed and is complete. Full information available on the introspection endpoint |

### `threeDSProviderResponse`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `threeDSProviderResponse.acs` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.acs.challengeMandated` | boolean | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Indication of whether a challenge is required for the transaction to be authorised due to local or regional mandates or other variable. |
| `threeDSProviderResponse.acs.operatorIdentifer` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | DS-assigned ACS identifier. Each DS can provide a unique ID to each ACS. |
| `threeDSProviderResponse.acs.referenceNumber` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Unique identifier assigned by the EMVCo Secretariat upon testing and approval. |
| `threeDSProviderResponse.acs.renderingType` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.acs.renderingType.acsInterface` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Identifies the ACS interface that presents the challenge to the cardholder. Options are 01, 02. |
| `threeDSProviderResponse.acs.renderingType.acsUiTemplate` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Identifies the UI Template format that the ACS presents to the customer first. Options are 01, 02, 03, 04, 05. |
| `threeDSProviderResponse.acs.transactionId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Universally Unique transaction identifier assigned by the ACS to identify a single transaction. Format uuid. |
| `threeDSProviderResponse.acs.url` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  | Fully-qualified URL of the ACS to be used for the challenge. |
| `threeDSProviderResponse.authenticationValue` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.cardHolderInformation` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.challengeCancelledIndicator` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.dsTransactionId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.eci` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.error` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.error.code` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.error.component` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.error.description` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.interactionCounter` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.message` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.message.version` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.sdk` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.sdk.transactionId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.threeDsResult` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.threeDsServerTransactionId` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.transaction` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.transaction.status` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.transaction.statusReason` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.trustList` | object | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.trustList.status` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.trustList.statusSource` | string | commerce Domain 1.1 | Low/Legacy — present in outdated OAS only |  |  |

### Other / Endpoint-Specific Fields

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `b64Endianness` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only | enum: be, le | Options for b64 endianness if format is modexp |
| `b64Signedness` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only | enum: signed, unsigned | Options for b64 signedness if format is modexp |
| `certificate` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | x509 Certificate containing the public key if the format specified is x509 |
| `consumerRequestAccountOptions.return` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Returns the Token from the specified Consumer Profile that matches the CardIssuer passed in. |
| `consumerRequestAccountOptions.save` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | If the specified Consumer Profile is found, but an associated card on file is not, the specified token will automatically be added as a card on file. |
| `consumerRequestAccountOptions.updateExpirationDate` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | If true, LegacyEngine will use the card expiration dates passed in the request instead of the exp dates stored in CardStor but only if it is newer than CardStor. will then update the CardStor with these dates. |
| `consumerRequestAccountOptions.use` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Use the Token found on the Consumer's Profile that matches the CardIssuer passed into |
| `consumerRequestBillingInformationOptions.avsMatchThreshold` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only | enum: Strict, Partial, None | Defines how closely the Billing Address must match an entry within the Address Verification Service. |
| `consumerRequestBillingInformationOptions.stopNotFound` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | If true and a Consumer Profile Billing Address is not found, stop processing and return error, else continue processing with the merchant-provided information or null if it does not exist. |
| `consumerRequestBillingInformationOptions.update` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Update the Billing Address information in the Consumer Data Service. Requires MerchantConsumerId to be passed. |
| `consumerRequestBillingInformationOptions.use` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Use the Billing Address information from the Consumer Data Service if one is found. Requires MerchantConsumerId, Payment Token, and CVV to be passed. |
| `consumerRequestProfileOptions.account` | object | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `consumerRequestProfileOptions.account.return` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Returns the Token from the specified Consumer Profile that matches the CardIssuer passed in. |
| `consumerRequestProfileOptions.account.save` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | If the specified Consumer Profile is found, but an associated card on file is not, the specified token will automatically be added as a card on file. |
| `consumerRequestProfileOptions.account.updateExpirationDate` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | If true, LegacyEngine will use the card expiration dates passed in the request instead of the exp dates stored in CardStor but only if it is newer than CardStor. will then update the CardStor with these dates. |
| `consumerRequestProfileOptions.account.use` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Use the Token found on the Consumer's Profile that matches the CardIssuer passed into |
| `consumerRequestProfileOptions.billingInformation` | object | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `consumerRequestProfileOptions.billingInformation.avsMatchThreshold` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only | enum: Strict, Partial, None | Defines how closely the Billing Address must match an entry within the Address Verification Service. |
| `consumerRequestProfileOptions.billingInformation.stopNotFound` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | If true and a Consumer Profile Billing Address is not found, stop processing and return error, else continue processing with the merchant-provided information or null if it does not exist. |
| `consumerRequestProfileOptions.billingInformation.update` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Update the Billing Address information in the Consumer Data Service. Requires MerchantConsumerId to be passed. |
| `consumerRequestProfileOptions.billingInformation.use` | boolean | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Use the Billing Address information from the Consumer Data Service if one is found. Requires MerchantConsumerId, Payment Token, and CVV to be passed. |
| `consumerResponse.profile` | object | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `consumerResponse.profile.accounts` | array | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `consumerResponse.profile.accounts[].account` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The Token from the Consumer's Profile which matches the CardIssuer from the request. |
| `consumerResponse.profile.accounts[].type` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The account type for this account. Will match card issuer that was passed in. |
| `consumerResponseProfile.accounts` | array | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `consumerResponseProfile.accounts[].account` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The Token from the Consumer's Profile which matches the CardIssuer from the request. |
| `consumerResponseProfile.accounts[].type` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The account type for this account. Will match card issuer that was passed in. |
| `consumerResponseProfileAccount.account` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The Token from the Consumer's Profile which matches the CardIssuer from the request. |
| `consumerResponseProfileAccount.type` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The account type for this account. Will match card issuer that was passed in. |
| `contextId` | string | Canonical JSON, OAS 1.2.0 | High | <contextId> | Uniquely identifies a unit of work. Provide when performing an asynchronous operation that spans multiple calls. This value is provided by  in all responses. |
| `data` | object | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  |  |
| `enterpriseCode` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The unique -supplied value that represents the enterprise |
| `expSubformat` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only | enum: b64, hex, dec | Specifies the subformat for exponent if format is modexp |
| `expirationDatetime` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Not-valid-after datetime for the key |
| `exponent` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Exponent e for RSA Encryption |
| `format` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only | enum: x509, spki, rpk, modexp | The format of the public key being returned. |
| `keySlotId` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The Key Slot Id set created within 's Enterprise Portal. |
| `locationReference` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Merchant property code, required if storeID not provided |
| `modSubformat` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only | enum: b64, hex, dec | Specifies the subformat for modulus if format is modexp |
| `modulus` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Modulus n for RSA Encryption |
| `publicKey` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The RSA public key in the specified format |
| `locationId` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | The unique -supplied value that represents a Store, required only if locationReference is not provided |
| `subformat` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only | enum: b64der, pem | Specifies the subformat for x509, spki, and rpk |
| `trackKsn` | string | OAS 1.2.0 | Low/Legacy — present in outdated OAS only |  | Value to be passed in the trackKsn field in the Payment Request to identify the correct key |

## JSON-Only Fields Requiring Schema Review

These fields are observed in canonical payloads but were not found in the parsed legacy OAS files. Many are likely legitimate current fields, especially under `transaction.*` and `operation.*`, because the legacy OAS references external schemas that were not included.

| Field Path | Type(s) | Observed Count | Source Files | Example Values |
|---|---|---:|---|---|
| `customer.billingAddress` | object | 3 | canonical-json-payloads-optional-other.md |  |
| `customer.billingAddress.address1` | string | 3 | canonical-json-payloads-optional-other.md | <addressLine1> |
| `customer.billingAddress.city` | string | 3 | canonical-json-payloads-optional-other.md | <city> |
| `customer.billingAddress.country` | string | 3 | canonical-json-payloads-optional-other.md | US |
| `customer.billingAddress.firstName` | string | 3 | canonical-json-payloads-optional-other.md | <firstName> |
| `customer.billingAddress.lastName` | string | 3 | canonical-json-payloads-optional-other.md | <lastName> |
| `customer.billingAddress.postalCode` | string | 3 | canonical-json-payloads-optional-other.md | <postalCode> |
| `customer.billingAddress.state` | string | 3 | canonical-json-payloads-optional-other.md | <state> |
| `customer.consumerId` | string | 1 | canonical-json-payloads-tokenization-variants.md | <consumerId> |
| `customer.email` | string | 1 | canonical-json-payloads-optional-other.md | <emailAddress> |
| `customer.customerReferenceId` | string | 1 | canonical-json-payloads-tokenization-variants.md | <customerReferenceId> |
| `commerce.cancelUrl` | string | 4 | canonical-json-payloads-3ds.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-wallets.md | <cancelUrl> |
| `commerce.clientSessionKey` | string | 7 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md | <clientSessionKey> |
| `commerce.redirectUrl` | string | 4 | canonical-json-payloads-3ds.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-wallets.md | <redirectUrl> |
| `commerce.returnStatus` | string | 6 | canonical-json-payloads-lifecycle.md | cancelled; expired |
| `commerce.returnUrl` | string | 4 | canonical-json-payloads-3ds.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-wallets.md | <returnUrl> |
| `commerce.threeDS` | object | 10 | canonical-json-payloads-3ds.md |  |
| `commerce.threeDS.channel` | string | 4 | canonical-json-payloads-3ds.md | mobile; browser |
| `commerce.threeDS.enabled` | boolean | 2 | canonical-json-payloads-3ds.md | True |
| `commerce.threeDS.required` | boolean | 3 | canonical-json-payloads-3ds.md | True; False |
| `commerce.threeDS.result` | string | 6 | canonical-json-payloads-3ds.md | challengeSuccessful; errorAtThreeDSProvider; challengeCancelledByCustomer |
| `transaction.cardInformation` | object | 20 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `transaction.cardInformation.brandCode` | string | 20 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | VS |
| `transaction.cardInformation.maskedPan` | string | 20 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | 411111xxxxxx1111 |
| `transaction.cardInformation.network` | string | 20 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | FREEWAY; VISA |
| `transaction.cardInformation.type` | string | 20 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | credit |
| `transaction.clientContext` | object | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `transaction.clientContext.sellingMiddlewareName` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <middlewareName> |
| `transaction.clientContext.sellingMiddlewareVersion` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <middlewareVersion> |
| `transaction.clientContext.sellingSystemName` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <systemName> |
| `transaction.clientContext.sellingSystemVersion` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <systemVersion> |
| `transaction.fallbackAction` | string | 1 | canonical-json-payloads-optional-other.md | refund |
| `transaction.invoice` | object | 30 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `transaction.invoice.invoiceHeader` | object | 30 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `transaction.invoice.invoiceHeader.invoiceNumber` | string | 30 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <invoiceNumber> |
| `transaction.invoice.purchaseTotals` | object | 29 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `transaction.invoice.purchaseTotals.chargeAmount` | number | 29 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | 6.0; 5.01; 0.0 |
| `transaction.invoice.purchaseTotals.currency` | string | 1 | canonical-json-payloads-optional-other.md | USD |
| `transaction.invoice.purchaseTotals.taxTotal` | number | 1 | canonical-json-payloads-core.md | 0.41 |
| `transaction.referenceId` | string | 85 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <referenceId> |
| `transaction.networkData` | object | 8 | canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-wallets.md |  |
| `transaction.networkData.network` | string | 8 | canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-wallets.md | FREEWAY |
| `transaction.relatedRequestId` | string | 8 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md | <originalRequestId>; <authorizationRequestId> |
| `transaction.originalAction` | string | 1 | canonical-json-payloads-optional-other.md | void |
| `transaction.response` | object | 37 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `transaction.response.decision` | string | 37 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | REJECT; ACCEPT |
| `transaction.response.reasonCode` | string | 37 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | 100; <reasonCode> |
| `transaction.response.requestId` | string | 37 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <voidRequestId>; <captureRequestId>; <refundRequestId> |
| `transaction.serviceReplies` | object | 34 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `transaction.serviceReplies.dccReply` | object | 1 | canonical-json-payloads-optional-other.md |  |
| `transaction.serviceReplies.dccReply.accepted` | boolean | 1 | canonical-json-payloads-optional-other.md | True |
| `transaction.serviceReplies.dccReply.exchangeRate` | number | 1 | canonical-json-payloads-optional-other.md | 0.925 |
| `transaction.serviceReplies.dccReply.exchangeRateSource` | string | 1 | canonical-json-payloads-optional-other.md | <exchangeRateSource> |
| `transaction.serviceReplies.dccReply.foreignAmount` | number | 1 | canonical-json-payloads-optional-other.md | 9.25 |
| `transaction.serviceReplies.dccReply.foreignCurrency` | string | 1 | canonical-json-payloads-optional-other.md | EUR |
| `transaction.serviceReplies.dccReply.reasonCode` | string | 1 | canonical-json-payloads-optional-other.md | 100 |
| `transaction.serviceReplies.fraudCheckReply` | object | 1 | canonical-json-payloads-optional-other.md |  |
| `transaction.serviceReplies.fraudCheckReply.decision` | string | 1 | canonical-json-payloads-optional-other.md | ACCEPT |
| `transaction.serviceReplies.fraudCheckReply.processorResponseCode` | string | 1 | canonical-json-payloads-optional-other.md | A |
| `transaction.serviceReplies.fraudCheckReply.reasonCode` | string | 1 | canonical-json-payloads-optional-other.md | 100 |
| `transaction.serviceReplies.tokenCreateReply` | object | 4 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md |  |
| `transaction.serviceReplies.tokenCreateReply.storedToken` | string | 4 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | <storedToken> |
| `transaction.serviceReplies.tokenCreateReply.reasonCode` | string | 4 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | 100 |
| `transaction.serviceReplies.transactionReply` | object | 34 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `transaction.serviceReplies.transactionReply.accountBalance2Currency` | string | 1 | canonical-json-payloads-core.md | USD |
| `transaction.serviceReplies.transactionReply.amount` | number | 33 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | 6.0; 5.01; 0.0 |
| `transaction.serviceReplies.transactionReply.authorizationCode` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <authorizationCode> |
| `transaction.serviceReplies.transactionReply.avsCode` | string | 2 | canonical-json-payloads-optional-other.md | M |
| `transaction.serviceReplies.transactionReply.cvCode` | string | 2 | canonical-json-payloads-optional-other.md | M |
| `transaction.serviceReplies.transactionReply.partialAmount` | boolean | 27 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | False |
| `transaction.serviceReplies.transactionReply.processorResponseCode` | string | 34 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | 100 |
| `transaction.serviceReplies.transactionReply.processorResponseMessage` | string | 34 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | APPROVED |
| `transaction.serviceReplies.transactionReply.processorTransactionId` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <processorTransactionId> |
| `transaction.serviceReplies.transactionReply.purchasingLevel3Enabled` | boolean | 2 | canonical-json-payloads-core.md | False |
| `transaction.serviceReplies.transactionReply.reasonCode` | string | 34 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | 100 |
| `transaction.serviceReplies.transactionReply.reconciliationId` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <reconciliationId> |
| `transaction.serviceReplies.transactionReply.requestDateTime` | string | 34 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <requestDateTime> |
| `transaction.services` | object | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `transaction.services.dccService` | object | 1 | canonical-json-payloads-optional-other.md |  |
| `transaction.services.dccService.run` | boolean | 1 | canonical-json-payloads-optional-other.md | True |
| `transaction.services.fraudCheckService` | object | 1 | canonical-json-payloads-optional-other.md |  |
| `transaction.services.fraudCheckService.run` | boolean | 1 | canonical-json-payloads-optional-other.md | True |
| `transaction.services.tokenCreateService` | object | 4 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md |  |
| `transaction.services.tokenCreateService.run` | boolean | 4 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | True |
| `transaction.services.transactionService` | object | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `transaction.services.transactionService.commerceIndicator` | string | 3 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md | internet |
| `transaction.services.transactionService.transactionType` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | capture; authorization; void |
| `transaction.tenders` | object | 23 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `transaction.tenders.alternativePayment` | object | 3 | canonical-json-payloads-lifecycle.md, canonical-json-payloads-wallets.md |  |
| `transaction.tenders.alternativePayment.paymentMethod` | string | 3 | canonical-json-payloads-lifecycle.md, canonical-json-payloads-wallets.md | paypal; athmovil; <paymentMethod> |
| `transaction.tenders.card` | object | 20 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `transaction.tenders.card.accountNumber` | string | 7 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | <dpan>; <storedToken>; <mpan> |
| `transaction.tenders.card.cardType` | string | 7 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | token |
| `transaction.tenders.card.encryptedData` | string | 13 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <applePayPaymentToken>; <googlePayPaymentToken>; <encryptedCardData> |
| `transaction.tenders.pos` | object | 13 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `transaction.tenders.pos.encMode` | string | 13 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | googlewallet; rsa; applewallet |
| `transaction.tenders.pos.entryMode` | string | 13 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | keyed; inapp |
| `transaction.tenders.pos.msrType` | string | 13 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | applepay; none; googlepay |
| `transaction.tenders.pos.trackKsn` | string | 11 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | <trackKsn> |
| `transaction.tokenInformation` | object | 11 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md |  |
| `transaction.tokenInformation.accountNumberMasked` | string | 11 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | 411111xxxxxx1111 |
| `transaction.tokenInformation.brand` | string | 11 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | VS |
| `transaction.tokenInformation.cardExpirationMonth` | integer | 11 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | 11 |
| `transaction.tokenInformation.cardExpirationYear` | integer | 11 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | 29 |
| `transaction.tokenInformation.cardType` | string | 11 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | credit |
| `operation.contextId` | string | 57 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <contextId> |
| `operation.decision` | string | 52 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | REJECT; INTERRUPT; ERROR |
| `operation.nonAcceptedServices` | array | 41 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `operation.nonAcceptedServices[]` | array | 41 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `operation.posSyncId` | string | 33 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <posSyncId> |
| `operation.reasonCode` | object | 52 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `operation.reasonCode.code` | string | 52 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <reasonCode>; 900 |
| `operation.reasonCode.description` | string | 52 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | Additional customer action required; Challenge cancelled by customer; Additional customer authentication required |

## OAS-Only Fields Requiring Current-State Validation

These fields appear in one or both legacy OAS files but were not observed in the canonical payload corpus.

| Field Path | Type(s) | Source | Enum / Notes |
|---|---|---|---|
| `b64Endianness` | string | reference-oas-a.yaml | enum: be, le |
| `b64Signedness` | string | reference-oas-a.yaml | enum: signed, unsigned |
| `certificate` | string | reference-oas-a.yaml | x509 Certificate containing the public key if the format specified is x509 |
| `customer.avsMatchThreshold` | string | reference-oas-a.yaml | This field is deprecated. Use 'customer.profile.billingInformation.avsMatchThreshold' instead. |
| `customer.createCardOnFileIfNotFound` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'customer.profile.account.save' instead. |
| `customer.createConsumer` | boolean | reference-oas-a.yaml | If a Consumer Profile is not found, automatically create a profile. Defaults to false. |
| `customer.createConsumerProfileIfNotFound` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'customer.createConsumer' instead. |
| `customer.merchantConsumerId` | string | reference-oas-a.yaml | The merchant-facing value used to uniquely identify a Consumer |
| `customer.profile` | object | reference-oas-a.yaml |  |
| `customer.profile.account` | object | reference-oas-a.yaml |  |
| `customer.profile.account.return` | boolean | reference-oas-a.yaml | Returns the Token from the specified Consumer Profile that matches the CardIssuer passed in. |
| `customer.profile.account.save` | boolean | reference-oas-a.yaml | If the specified Consumer Profile is found, but an associated card on file is not, the specified token will automatically be added as a card on file. |
| `customer.profile.account.updateExpirationDate` | boolean | reference-oas-a.yaml | If true, LegacyEngine will use the card expiration dates passed in the request instead of the exp dates stored in CardStor but only if it is newer than CardStor. will then update the CardStor with these dates. |
| `customer.profile.account.use` | boolean | reference-oas-a.yaml | Use the Token found on the Consumer's Profile that matches the CardIssuer passed into |
| `customer.profile.accounts` | array | reference-oas-a.yaml |  |
| `customer.profile.accounts[].account` | string | reference-oas-a.yaml | The Token from the Consumer's Profile which matches the CardIssuer from the request. |
| `customer.profile.accounts[].type` | string | reference-oas-a.yaml | The account type for this account. Will match card issuer that was passed in. |
| `customer.profile.billingInformation` | object | reference-oas-a.yaml |  |
| `customer.profile.billingInformation.avsMatchThreshold` | string | reference-oas-a.yaml | enum: Strict, Partial, None |
| `customer.profile.billingInformation.stopNotFound` | boolean | reference-oas-a.yaml | If true and a Consumer Profile Billing Address is not found, stop processing and return error, else continue processing with the merchant-provided information or null if it does not exist. |
| `customer.profile.billingInformation.update` | boolean | reference-oas-a.yaml | Update the Billing Address information in the Consumer Data Service. Requires MerchantConsumerId to be passed. |
| `customer.profile.billingInformation.use` | boolean | reference-oas-a.yaml | Use the Billing Address information from the Consumer Data Service if one is found. Requires MerchantConsumerId, Payment Token, and CVV to be passed. |
| `customer.refreshBillingAddressOnConsumerProfile` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'customer.profile.billingInformation.update' instead. |
| `customer.returnCardOnFileToken` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'customer.profile.account.return' instead. |
| `customer.stopOnConsumerProfileBillingAddressNotFound` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'customer.profile.billingInformation.stopNotFound' instead. |
| `customer.stopOnRefreshBillingAddressOnConsumerProfileFailure` | boolean | reference-oas-a.yaml | This field is deprecated and has no effect. Values provided will be ignored. |
| `customer.updateCardExpirationDate` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'customer.profile.card.updateExpirationDate' instead. |
| `customer.useConsumerProfileBillingAddress` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'customer.profile.billingInformation.use' instead. |
| `customer.useConsumerProfileTokenForPayment` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'customer.profile.account.use' instead. |
| `consumerRequestAccountOptions.return` | boolean | reference-oas-a.yaml | Returns the Token from the specified Consumer Profile that matches the CardIssuer passed in. |
| `consumerRequestAccountOptions.save` | boolean | reference-oas-a.yaml | If the specified Consumer Profile is found, but an associated card on file is not, the specified token will automatically be added as a card on file. |
| `consumerRequestAccountOptions.updateExpirationDate` | boolean | reference-oas-a.yaml | If true, LegacyEngine will use the card expiration dates passed in the request instead of the exp dates stored in CardStor but only if it is newer than CardStor. will then update the CardStor with these dates. |
| `consumerRequestAccountOptions.use` | boolean | reference-oas-a.yaml | Use the Token found on the Consumer's Profile that matches the CardIssuer passed into |
| `consumerRequestBillingInformationOptions.avsMatchThreshold` | string | reference-oas-a.yaml | enum: Strict, Partial, None |
| `consumerRequestBillingInformationOptions.stopNotFound` | boolean | reference-oas-a.yaml | If true and a Consumer Profile Billing Address is not found, stop processing and return error, else continue processing with the merchant-provided information or null if it does not exist. |
| `consumerRequestBillingInformationOptions.update` | boolean | reference-oas-a.yaml | Update the Billing Address information in the Consumer Data Service. Requires MerchantConsumerId to be passed. |
| `consumerRequestBillingInformationOptions.use` | boolean | reference-oas-a.yaml | Use the Billing Address information from the Consumer Data Service if one is found. Requires MerchantConsumerId, Payment Token, and CVV to be passed. |
| `consumerRequestProfileOptions.account` | object | reference-oas-a.yaml |  |
| `consumerRequestProfileOptions.account.return` | boolean | reference-oas-a.yaml | Returns the Token from the specified Consumer Profile that matches the CardIssuer passed in. |
| `consumerRequestProfileOptions.account.save` | boolean | reference-oas-a.yaml | If the specified Consumer Profile is found, but an associated card on file is not, the specified token will automatically be added as a card on file. |
| `consumerRequestProfileOptions.account.updateExpirationDate` | boolean | reference-oas-a.yaml | If true, LegacyEngine will use the card expiration dates passed in the request instead of the exp dates stored in CardStor but only if it is newer than CardStor. will then update the CardStor with these dates. |
| `consumerRequestProfileOptions.account.use` | boolean | reference-oas-a.yaml | Use the Token found on the Consumer's Profile that matches the CardIssuer passed into |
| `consumerRequestProfileOptions.billingInformation` | object | reference-oas-a.yaml |  |
| `consumerRequestProfileOptions.billingInformation.avsMatchThreshold` | string | reference-oas-a.yaml | enum: Strict, Partial, None |
| `consumerRequestProfileOptions.billingInformation.stopNotFound` | boolean | reference-oas-a.yaml | If true and a Consumer Profile Billing Address is not found, stop processing and return error, else continue processing with the merchant-provided information or null if it does not exist. |
| `consumerRequestProfileOptions.billingInformation.update` | boolean | reference-oas-a.yaml | Update the Billing Address information in the Consumer Data Service. Requires MerchantConsumerId to be passed. |
| `consumerRequestProfileOptions.billingInformation.use` | boolean | reference-oas-a.yaml | Use the Billing Address information from the Consumer Data Service if one is found. Requires MerchantConsumerId, Payment Token, and CVV to be passed. |
| `consumerResponse.profile` | object | reference-oas-a.yaml |  |
| `consumerResponse.profile.accounts` | array | reference-oas-a.yaml |  |
| `consumerResponse.profile.accounts[].account` | string | reference-oas-a.yaml | The Token from the Consumer's Profile which matches the CardIssuer from the request. |
| `consumerResponse.profile.accounts[].type` | string | reference-oas-a.yaml | The account type for this account. Will match card issuer that was passed in. |
| `consumerResponseProfile.accounts` | array | reference-oas-a.yaml |  |
| `consumerResponseProfile.accounts[].account` | string | reference-oas-a.yaml | The Token from the Consumer's Profile which matches the CardIssuer from the request. |
| `consumerResponseProfile.accounts[].type` | string | reference-oas-a.yaml | The account type for this account. Will match card issuer that was passed in. |
| `consumerResponseProfileAccount.account` | string | reference-oas-a.yaml | The Token from the Consumer's Profile which matches the CardIssuer from the request. |
| `consumerResponseProfileAccount.type` | string | reference-oas-a.yaml | The account type for this account. Will match card issuer that was passed in. |
| `authentication.applicationId` | string | reference-oas-a.yaml | The -assigned value that uniquely identifies the merchant organization. |
| `authentication.locationReference` | string | reference-oas-a.yaml | The merchant value that represents a StoreId and TerminalId on the  side. Not required if passing StoreId and TerminalId. If locationReference is provided, those values will be resolved automatically. |
| `data` | object | reference-oas-a.yaml |  |
| `clientDevice` | object | reference-oas-a.yaml |  |
| `clientDevice.applicationName` | string | reference-oas-a.yaml |  |
| `clientDevice.id` | string | reference-oas-a.yaml |  |
| `clientDevice.proceedWithAuth` | boolean | reference-oas-a.yaml |  |
| `clientDevice.registrationId` | string | reference-oas-a.yaml |  |
| `clientDevice.requests` | array | reference-oas-a.yaml |  |
| `clientDevice.responses` | array | reference-oas-a.yaml |  |
| `clientDevice.type` | string | reference-oas-a.yaml |  |
| `clientDevice.version` | string | reference-oas-a.yaml |  |
| `commerce.apmOptions` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.apmOptions.qrCodeHeight` | integer | -HPXeCommerceDomain-1.1-domain.yaml | Height of the QRCode image |
| `commerce.apmOptions.qrCodeWidth` | integer | -HPXeCommerceDomain-1.1-domain.yaml | Width of the QRCode image |
| `commerce.apmOptions.returnQrCodeImage` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | Apm Url is always returned when applicable. If a scannable QRCode is desired, pass true for this. |
| `commerce.apmOptions.thirdPartyIdentifier` | string | -HPXeCommerceDomain-1.1-domain.yaml | A unique identifier required by some third parties to uniquely identify a transaction session |
| `commerce.apmQrCode` | string | -HPXeCommerceDomain-1.1-domain.yaml | Base64 representation of the QrCode in png format |
| `commerce.apmUrl` | string | -HPXeCommerceDomain-1.1-domain.yaml | Url to the Apm transaction system |
| `commerce.cardIssuer` | string | -HPXeCommerceDomain-1.1-domain.yaml | enum: alipay, athmovilecom, athmovilinstore, citidloc, citimil, cnh, klarna, openbanking, paypal, venmo |
| `commerce.reasonCode` | object | -HPXeCommerceDomain-1.1-domain.yaml | Provides additional information for the commerce portion of the operation. Values in the range of 900-999. |
| `commerce.reasonCode.code` | integer | -HPXeCommerceDomain-1.1-domain.yaml | Numerical representation of the ReasonCode |
| `commerce.reasonCode.description` | string | -HPXeCommerceDomain-1.1-domain.yaml | Textual representation of the ReasonCode |
| `commerce.serviceProviderCodes` | array | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.serviceProviderCodes[].serviceProviderCode` | string | -HPXeCommerceDomain-1.1-domain.yaml | Code returned by the third party |
| `commerce.serviceProviderCodes[].serviceProviderMessage` | string | -HPXeCommerceDomain-1.1-domain.yaml | The message provided by the third party |
| `commerce.serviceProviderCodes[].serviceProviderName` | string | -HPXeCommerceDomain-1.1-domain.yaml | The Third Party Provider |
| `commerce.threeDSOptions` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSOptions.account` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSOptions.account.ageIndicator` | string | -HPXeCommerceDomain-1.1-domain.yaml | Length of time that the cardholder had an account with the 3DS requestor. Options 01, 02, 03, 04 and 05. |
| `commerce.threeDSOptions.account.changeIndicator` | string | -HPXeCommerceDomain-1.1-domain.yaml | Length of time since the cardholder's account information with the 3DS Requestor was last changed, including Billing or Shipping address, new transaction account, or new user added. Options 01, 02, 03 and 04. |
| `commerce.threeDSOptions.account.changedDate` | string | -HPXeCommerceDomain-1.1-domain.yaml | The last changed date of the cardholder's account with the 3DS Requestor, including Billing or Shipping address, new transaction account, or new user added. Format YYYYMMDD. |
| `commerce.threeDSOptions.account.dateOpened` | string | -HPXeCommerceDomain-1.1-domain.yaml | Date that the cardholder opened the account with the 3DS Requestor. Format YYYYMMDD |
| `commerce.threeDSOptions.account.identifier` | string | -HPXeCommerceDomain-1.1-domain.yaml | Additional information about the account that is optionally provided by the 3DS Requestor. |
| `commerce.threeDSOptions.account.passwordChangeIndicator` | string | -HPXeCommerceDomain-1.1-domain.yaml | Length of time since the cardholder's account with the 3DS Requestor had a password change or account reset. Options 01,02,03, 04 and 05. |
| `commerce.threeDSOptions.account.passwordChangedDate` | string | -HPXeCommerceDomain-1.1-domain.yaml | Date that cardholder's account with the 3DS Requestor had a password change or account reset. Format YYYYMMDD. |
| `commerce.threeDSOptions.account.paymentAccountAgeDate` | string | -HPXeCommerceDomain-1.1-domain.yaml | Date on which the transaction account was enrolled in the cardholder's account with the 3DS Requestor. Format YYYYMMDD. |
| `commerce.threeDSOptions.account.paymentAccountIndicator` | string | -HPXeCommerceDomain-1.1-domain.yaml | Indicates the length of time that the transaction account was enrolled in the cardholder's account with the 3DS Requestor. Options 01, 02, 03, 04 and 05. |
| `commerce.threeDSOptions.account.provisionAttemptsDay` | string | -HPXeCommerceDomain-1.1-domain.yaml | Number of Add Card attempts in the last 24 hours. |
| `commerce.threeDSOptions.account.purchasesInLastSixMonths` | string | -HPXeCommerceDomain-1.1-domain.yaml | Number of purchases with the cardholder's account during the previous six months. |
| `commerce.threeDSOptions.account.shippingAddressUsageDate` | string | -HPXeCommerceDomain-1.1-domain.yaml | Indicates the date when the shipping address was first used with the 3DS Requestor for the transaction. Format YYYYMMDD. |
| `commerce.threeDSOptions.account.shippingAddressUsageIndicator` | string | -HPXeCommerceDomain-1.1-domain.yaml | Indicates when the shipping address used for this transaction was first used with the 3DS Requestor. Options 01, 02, 03 and 04. |
| `commerce.threeDSOptions.account.shippingNameIndicator` | string | -HPXeCommerceDomain-1.1-domain.yaml | Indicates if the cardholder's name on the account is identical to the shipping name used for this transaction. Options 01=Same, 02=Different. |
| `commerce.threeDSOptions.account.suspiciousAccountActivityIndicator` | string | -HPXeCommerceDomain-1.1-domain.yaml | Suspicious activity (including previous fraud occurrence) indicator on the cardholder account. Options 01=No Suspicious Activity, 02=Suspicious Activity. |
| `commerce.threeDSOptions.account.txnActivityDay` | string | -HPXeCommerceDomain-1.1-domain.yaml | Number of transactions for the cardholder's account with the 3DS Requestor across all transaction accounts in the previous 24 hours. |
| `commerce.threeDSOptions.account.txnActivityYear` | string | -HPXeCommerceDomain-1.1-domain.yaml | Number of transactions for the cardholder's account with the 3DS Requestor across all transaction accounts in the previous year. |
| `commerce.threeDSOptions.account.type` | string | -HPXeCommerceDomain-1.1-domain.yaml | Indicates the type of account. For example, for a multi-account card product. |
| `commerce.threeDSOptions.addressesMustMatch` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | Indicates that the Bill-To and Ship-To addresses are identical |
| `commerce.threeDSOptions.authenticationIndicator` | string | -HPXeCommerceDomain-1.1-domain.yaml | Indicates the type of Authentication request. This data element provides additional information to the ACS to determine the best approach for handling an authentication request. |
| `commerce.threeDSOptions.challengeIndicator` | string | -HPXeCommerceDomain-1.1-domain.yaml | Indicates whether a challenge is requested for this transaction. For example, for 01-PA, a 3DS Requestor may have concerns about the transaction, and request a challenge. For 02-NPA, a challenge may be necessary when adding a new card to a … |
| `commerce.threeDSOptions.challengeWindowSize` | string | -HPXeCommerceDomain-1.1-domain.yaml | Required if generateCReq is present |
| `commerce.threeDSOptions.clientType` | string | -HPXeCommerceDomain-1.1-domain.yaml | enum: browserJs, mobileSdk |
| `commerce.threeDSOptions.merchantDomainName` | string | -HPXeCommerceDomain-1.1-domain.yaml | The domain name of the merchant website, this will help to handle the CORS issue in three DS flow |
| `commerce.threeDSOptions.mobileSdkRenderOptions` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSOptions.mobileSdkRenderOptions.interface` | string | -HPXeCommerceDomain-1.1-domain.yaml | Lists all the SDK Interface types that the clientDevice supports for displaying specific challenge user interfaces within the SDK. Values accepted are 01=Native, 02=HTML, 03=Both |
| `commerce.threeDSOptions.mobileSdkRenderOptions.uiType` | string | -HPXeCommerceDomain-1.1-domain.yaml | Lists all the UI types that the clientDevice supports for displaying specific challenge user interfaces within the SDK. Values accepted are Native UI=01-04; HTML UI=01-05. |
| `commerce.threeDSOptions.purchaseInstallData` | integer | -HPXeCommerceDomain-1.1-domain.yaml | Indicates the maximum number of authorizations permitted for installment payments. |
| `commerce.threeDSOptions.recurrence` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSOptions.recurrence.expirationDate` | string | -HPXeCommerceDomain-1.1-domain.yaml | Date after which no further authorizations will be performed. Format YYYYMMDD. |
| `commerce.threeDSOptions.recurrence.frequency` | integer | -HPXeCommerceDomain-1.1-domain.yaml | Indicates the minimum number of days between authorizations. |
| `commerce.threeDSOptions.resultDrivenProcessing` | object | -HPXeCommerceDomain-1.1-domain.yaml | This will instruct the what actions needed to be taken once the 3ds outcome has received |
| `commerce.threeDSOptions.resultDrivenProcessing.enabled` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | Default true, If false, this will instruct the system that 3ds outcome will have no impact on the transaction processing |
| `commerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus` | object | -HPXeCommerceDomain-1.1-domain.yaml | This will instruct how to process specific 3ds outcome |
| `commerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSAuthenticationAttempted` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | ??? |
| `commerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSAuthenticationFailed` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | Default false, if true, the transaction processing will continue even when the 3ds authentication faild. |
| `commerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSAuthenticationSuccessful` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | Default true, If true, this will process transaction if 3ds authentication was successful |
| `commerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSCardNotSupported` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | Default false, if true, the transaction processing will continue even when the card doesn't support 3ds. |
| `commerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSRejected` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | Default false, if true, the transaction processing will continue even when the 3ds is rejected. |
| `commerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSUnavailable` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | Default false, if true, the transaction processing will continue even when the 3ds authentication was unavailable. |
| `commerce.threeDSOptions.runThreeDsAuthentication` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | Default true, this indicates if the customer wants to run the 3DS authentication or not. This will override the store config value |
| `commerce.threeDSOptions.transactionType` | string | -HPXeCommerceDomain-1.1-domain.yaml | Identifies the type of transaction being authenticated. This field is required for VISA transactions and accepted values are 01, 03, 10, 11, and 28. |
| `commerce.threeDSProviderResponse` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.acs` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.acs.challengeMandated` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | Indication of whether a challenge is required for the transaction to be authorised due to local or regional mandates or other variable. |
| `commerce.threeDSProviderResponse.acs.operatorIdentifer` | string | -HPXeCommerceDomain-1.1-domain.yaml | DS-assigned ACS identifier. Each DS can provide a unique ID to each ACS. |
| `commerce.threeDSProviderResponse.acs.referenceNumber` | string | -HPXeCommerceDomain-1.1-domain.yaml | Unique identifier assigned by the EMVCo Secretariat upon testing and approval. |
| `commerce.threeDSProviderResponse.acs.renderingType` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.acs.renderingType.acsInterface` | string | -HPXeCommerceDomain-1.1-domain.yaml | Identifies the ACS interface that presents the challenge to the cardholder. Options are 01, 02. |
| `commerce.threeDSProviderResponse.acs.renderingType.acsUiTemplate` | string | -HPXeCommerceDomain-1.1-domain.yaml | Identifies the UI Template format that the ACS presents to the customer first. Options are 01, 02, 03, 04, 05. |
| `commerce.threeDSProviderResponse.acs.transactionId` | string | -HPXeCommerceDomain-1.1-domain.yaml | Universally Unique transaction identifier assigned by the ACS to identify a single transaction. Format uuid. |
| `commerce.threeDSProviderResponse.acs.url` | string | -HPXeCommerceDomain-1.1-domain.yaml | Fully-qualified URL of the ACS to be used for the challenge. |
| `commerce.threeDSProviderResponse.authenticationValue` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.cardHolderInformation` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.challengeCancelledIndicator` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.dsTransactionId` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.eci` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.error` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.error.code` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.error.component` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.error.description` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.interactionCounter` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.message` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.message.version` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.sdk` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.sdk.transactionId` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.threeDsResult` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.threeDsServerTransactionId` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.transaction` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.transaction.status` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.transaction.statusReason` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.trustList` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.trustList.status` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `commerce.threeDSProviderResponse.trustList.statusSource` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `enterpriseCode` | string | reference-oas-a.yaml | The unique -supplied value that represents the enterprise |
| `expSubformat` | string | reference-oas-a.yaml | enum: b64, hex, dec |
| `expirationDatetime` | string | reference-oas-a.yaml | Not-valid-after datetime for the key |
| `exponent` | string | reference-oas-a.yaml | Exponent e for RSA Encryption |
| `format` | string | reference-oas-a.yaml | enum: x509, spki, rpk, modexp |
| `pointOfSale` | object | reference-oas-a.yaml |  |
| `keySlotId` | string | reference-oas-a.yaml | The Key Slot Id set created within 's Enterprise Portal. |
| `locationReference` | string | reference-oas-a.yaml | Merchant property code, required if storeID not provided |
| `modSubformat` | string | reference-oas-a.yaml | enum: b64, hex, dec |
| `modulus` | string | reference-oas-a.yaml | Modulus n for RSA Encryption |
| `transaction.autoRefundOnVoidFailure` | boolean | reference-oas-a.yaml | Denotes an optional fallback behavior. Perform a Refund when a Void request fails due to batch timing. |
| `publicKey` | string | reference-oas-a.yaml | The RSA public key in the specified format |
| `locationId` | string | reference-oas-a.yaml | The unique -supplied value that represents a Store, required only if locationReference is not provided |
| `subformat` | string | reference-oas-a.yaml | enum: b64der, pem |
| `authenticationProgressRequest.browserCharacteristics` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.browserCharacteristics.acceptHeader` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.browserCharacteristics.colorDepth` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.browserCharacteristics.ipAddress` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.browserCharacteristics.javaEnabled` | boolean | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.browserCharacteristics.javascriptEnabled` | boolean | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.browserCharacteristics.language` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.browserCharacteristics.screenHeight` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.browserCharacteristics.screenWidth` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.browserCharacteristics.timeZoneOffset` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.browserCharacteristics.userAgent` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.clientErrors` | array | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.clientErrors[].message` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.contextId` | string | -HPXeCommerceDomain-1.1-domain.yaml | Uniquely identifies a unit of work. Provide when performing an asynchronous operation that spans multiple calls. |
| `authenticationProgressRequest.mobileSdkData` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.mobileSdkData.applicationId` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.mobileSdkData.encData` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.mobileSdkData.maxTimeout` | integer | -HPXeCommerceDomain-1.1-domain.yaml | Time in minutes for the entire 3DS operation including Auth/Challenge/etc. Minimum of 5 minutes, maximum of 60. |
| `authenticationProgressRequest.mobileSdkData.publicKey` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.mobileSdkData.publicKey.crv` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.mobileSdkData.publicKey.keyType` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.mobileSdkData.publicKey.x` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.mobileSdkData.publicKey.y` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.mobileSdkData.referenceNumber` | string | -HPXeCommerceDomain-1.1-domain.yaml | Identifies the vendor and version for the 3DS SDK that is integrated in a 3DS Requestor App, assigned by EMVCo when the 3DS SDK is approved. |
| `authenticationProgressRequest.mobileSdkData.transactionId` | string | -HPXeCommerceDomain-1.1-domain.yaml | Universally unique transaction identifier assigned by the 3DS SDK to identify a single transaction. |
| `authenticationProgressRequest.trackKsn` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `authenticationProgressRequest.tracke` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.challengeAcsUrl` | string | -HPXeCommerceDomain-1.1-domain.yaml | The Url for the customer challenge if 3dsOptions.clientType is 'browserJs' |
| `threeDSProgressResponse.clientPollInterval` | integer | -HPXeCommerceDomain-1.1-domain.yaml | How many seconds between poll attempts |
| `threeDSProgressResponse.clientPollWindow` | integer | -HPXeCommerceDomain-1.1-domain.yaml | The maximum seconds to poll |
| `threeDSProgressResponse.merchantNotifiedOfTransactionResult` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | Denotes that  invoked the Merchant callback endpoint |
| `threeDSProgressResponse.methodUrl` | string | -HPXeCommerceDomain-1.1-domain.yaml | The Url for the clientDevice data collection if the 3dsOptions.ClientType is 'browserJs' |
| `threeDSProgressResponse.mobileSdkData` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.mobileSdkData.dsCaCertificate` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey.e` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey.kty` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey.n` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKeyId` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.status` | string | -HPXeCommerceDomain-1.1-domain.yaml | enum: CardDataMissing, NotApplicable, DeviceDataRequired, ChallengeRequired, ChallengeNotNeeded, ErrorAt3dsProvider, ChallengeSuccessful, ChallengeFailed, ChallengeCancelledByCustomer |
| `threeDSProgressResponse.threeDSEncodedChallengeRequestData` | string | -HPXeCommerceDomain-1.1-domain.yaml | Base64 URL encoded CReq message, only returned for challenge transactions during browser flow |
| `threeDSProgressResponse.threeDSProvider` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.acs` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.acs.challengeMandated` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | Indication of whether a challenge is required for the transaction to be authorised due to local or regional mandates or other variable. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.operatorIdentifer` | string | -HPXeCommerceDomain-1.1-domain.yaml | DS-assigned ACS identifier. Each DS can provide a unique ID to each ACS. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.referenceNumber` | string | -HPXeCommerceDomain-1.1-domain.yaml | Unique identifier assigned by the EMVCo Secretariat upon testing and approval. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.renderingType` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.acs.renderingType.acsInterface` | string | -HPXeCommerceDomain-1.1-domain.yaml | Identifies the ACS interface that presents the challenge to the cardholder. Options are 01, 02. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.renderingType.acsUiTemplate` | string | -HPXeCommerceDomain-1.1-domain.yaml | Identifies the UI Template format that the ACS presents to the customer first. Options are 01, 02, 03, 04, 05. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.transactionId` | string | -HPXeCommerceDomain-1.1-domain.yaml | Universally Unique transaction identifier assigned by the ACS to identify a single transaction. Format uuid. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.url` | string | -HPXeCommerceDomain-1.1-domain.yaml | Fully-qualified URL of the ACS to be used for the challenge. |
| `threeDSProgressResponse.threeDSProviderResponse.authenticationValue` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.cardHolderInformation` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.challengeCancelledIndicator` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.dsTransactionId` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.eci` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.error` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.error.code` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.error.component` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.error.description` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.interactionCounter` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.message` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.message.version` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.sdk` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.sdk.transactionId` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.threeDsResult` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.threeDsServerTransactionId` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.transaction` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.transaction.status` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.transaction.statusReason` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.trustList` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.trustList.status` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.trustList.statusSource` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProgressResponse.threeDSProviderTransactionId` | string | -HPXeCommerceDomain-1.1-domain.yaml | The provider specific transactionId that is the unique identifer for the 3ds session. |
| `threeDSProgressResponse.transactionComplete` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | The transaction has been processed and is complete. Full information available on the introspection endpoint |
| `threeDSProviderResponse.acs` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.acs.challengeMandated` | boolean | -HPXeCommerceDomain-1.1-domain.yaml | Indication of whether a challenge is required for the transaction to be authorised due to local or regional mandates or other variable. |
| `threeDSProviderResponse.acs.operatorIdentifer` | string | -HPXeCommerceDomain-1.1-domain.yaml | DS-assigned ACS identifier. Each DS can provide a unique ID to each ACS. |
| `threeDSProviderResponse.acs.referenceNumber` | string | -HPXeCommerceDomain-1.1-domain.yaml | Unique identifier assigned by the EMVCo Secretariat upon testing and approval. |
| `threeDSProviderResponse.acs.renderingType` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.acs.renderingType.acsInterface` | string | -HPXeCommerceDomain-1.1-domain.yaml | Identifies the ACS interface that presents the challenge to the cardholder. Options are 01, 02. |
| `threeDSProviderResponse.acs.renderingType.acsUiTemplate` | string | -HPXeCommerceDomain-1.1-domain.yaml | Identifies the UI Template format that the ACS presents to the customer first. Options are 01, 02, 03, 04, 05. |
| `threeDSProviderResponse.acs.transactionId` | string | -HPXeCommerceDomain-1.1-domain.yaml | Universally Unique transaction identifier assigned by the ACS to identify a single transaction. Format uuid. |
| `threeDSProviderResponse.acs.url` | string | -HPXeCommerceDomain-1.1-domain.yaml | Fully-qualified URL of the ACS to be used for the challenge. |
| `threeDSProviderResponse.authenticationValue` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.cardHolderInformation` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.challengeCancelledIndicator` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.dsTransactionId` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.eci` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.error` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.error.code` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.error.component` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.error.description` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.interactionCounter` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.message` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.message.version` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.sdk` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.sdk.transactionId` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.threeDsResult` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.threeDsServerTransactionId` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.transaction` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.transaction.status` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.transaction.statusReason` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.trustList` | object | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.trustList.status` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `threeDSProviderResponse.trustList.statusSource` | string | -HPXeCommerceDomain-1.1-domain.yaml |  |
| `trackKsn` | string | reference-oas-a.yaml | Value to be passed in the trackKsn field in the Payment Request to identify the correct key |

## Recommended Next Steps

1. Review JSON-only fields first, because they likely represent current documentation behavior missing from the legacy specs.
2. Review OAS-only fields second, because they may represent stale, deprecated, externally referenced, or still-valid-but-unrepresented capabilities.
3. Create a compact `hpx-oas-reference.md` for the `review-hpx-example` skill using high- and medium-confidence fields only.
4. Use this inventory as the input to a reconstructed documentation OAS, preserving source and confidence metadata.