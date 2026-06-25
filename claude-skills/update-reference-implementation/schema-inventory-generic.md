# Schema Inventory

> Working draft generated from two legacy OAS files plus canonical JSON payload examples. This is a documentation reference inventory, not an official architecture-owned API specification.

Generated: 2026-06-18 13:44 UTC

## Source Files Analyzed

- `reference-oas-b.yaml`
- `reference-oas-a.yaml`
- `canonical-json-payloads-core(1).md`
- `canonical-json-payloads-tokenization-variants(1).md`
- `canonical-json-payloads-wallets(1).md`
- `canonical-json-payloads-3ds(1).md`
- `canonical-json-payloads-lifecycle(1).md`
- `canonical-json-payloads-optional-other(1).md`

## Important Caveats

- The uploaded Reference OAS A references external domain schemas that were not included, including External payment and Workflow workflow schemas. Those unresolved references mean many `payment.*` and `workflow.*` fields are only available from canonical payloads.
- The inventory tracks observed fields, inferred types, source provenance, and confidence. It should be reviewed by Product/Engineering before being treated as a formal schema source.
- `JSON-only` does not mean invalid. It means the field appears in the canonical payload corpus but was not found in the parsed legacy OAS files.
- `OAS-only` does not mean current. It means the field appears in one of the legacy OAS files but was not observed in the canonical payload corpus.

## Endpoint Inventory from Reference OAS A

| Method | Path | Summary | Source |
|---|---|---|---|
| POST | `/config` | Return configuration data including Payment Options enabled for the Enterprise | `reference-oas-a.yaml` |
| POST | `/key` | Return the RSA public key to use to secure payment data if encryption is done in merchant code | `reference-oas-a.yaml` |
| POST | `/payment` | Process a payment through the Vendor platform | `reference-oas-a.yaml` |
| POST | `/payment/reverse` | Reverse a payment through the Vendor platform | `reference-oas-a.yaml` |
| POST | `/payment/introspect` | Get the status of a recent payment using a valid context id | `reference-oas-a.yaml` |
| POST | `/3ds/progress` | Support for a Secure Customer Authentication workflow | `reference-oas-a.yaml` |

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
| eCommerce.threeDS vs eCommerce.threeDSOptions | Canonical payloads use eCommerce.threeDS.* for enabled/channel/result/required, while the eCommerce Domain OAS exposes eCommerce.threeDSOptions.*. Treat as a probable schema/model drift area. |
| eCommerce.returnUrl / cancelUrl / redirectUrl | Canonical APM and browser 3DS examples use eCommerce.returnUrl, eCommerce.cancelUrl, and eCommerce.redirectUrl; these are not directly present in the parsed legacy OAS field inventory. |
| payment.* model mostly unresolved in OAS | The Reference OAS A references an external ExternalDomain payment schema that is not present in the uploaded files, so most payment.* fields are inferred from canonical payloads. |
| workflow.* model mostly unresolved in OAS | The Reference OAS A references an external WorkflowDomain workflow schema that is not present in the uploaded files, so workflow.* fields are mostly inferred from canonical payloads. |
| consumer identity naming | Canonical payloads use consumerId and merchantCustomerId, while the OAS uses merchantConsumerId and several deprecated consumer profile fields. This needs human review. |

## Field Inventory by Container

### `workflow`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `workflow` | object | Canonical JSON, Reference OAS A | High |  | Observed in: 1. Sale — New Card with RSA > Example Request; 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Request |
| `workflow.contextId` | string | Canonical JSON | Medium — observed in canonical payloads only | <contextId> | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `workflow.decision` | string | Canonical JSON | Medium — observed in canonical payloads only | REJECT; INTERRUPT; ERROR | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `workflow.nonAcceptedServices` | array | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `workflow.nonAcceptedServices[]` | array | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `workflow.posSyncId` | string | Canonical JSON | Medium — observed in canonical payloads only | <posSyncId> | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `workflow.reasonCode` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `workflow.reasonCode.code` | string | Canonical JSON | Medium — observed in canonical payloads only | <reasonCode>; 900 | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `workflow.reasonCode.description` | string | Canonical JSON | Medium — observed in canonical payloads only | Additional customer action required; Challenge cancelled by customer; Additional customer authentication required | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |

### `credentials`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `credentials` | object | Canonical JSON, Reference OAS A | High |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `credentials.clientId` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The Vendor-assigned value that uniquely identifies the merchant organization. |
| `credentials.merchantPropertyCode` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The merchant value that represents a StoreId and TerminalId on the Vendor side. Not required if passing StoreId and TerminalId. If merchantPropertyCode is provided, those values will be resolved automatically. |
| `credentials.storeId` | string | Canonical JSON, Reference OAS A | High | <storeId> | The Vendor public StoreId assigned to the merchant. Not required if passing merchantPropertyCode |
| `credentials.terminalId` | string | Canonical JSON, Reference OAS A | High | <terminalId> | The Vendor public TerminalId assigned to the merchant. Not required if passing merchantPropertyCode |

### `consumer`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `consumer` | object | Canonical JSON, Reference OAS A | High |  | Observed in: 3. Returning User with PMT from Vendor CDS > Example Request; 4. Returning User with Merchant-Managed PMT > Example Request; Optional Features > 2. AVS / CVV Sale Request |
| `consumer.avsMatchThreshold` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'consumer.profile.billingInformation.avsMatchThreshold' instead. |
| `consumer.billingAddress` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `consumer.billingAddress.address1` | string | Canonical JSON | Medium — observed in canonical payloads only | <addressLine1> | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `consumer.billingAddress.city` | string | Canonical JSON | Medium — observed in canonical payloads only | <city> | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `consumer.billingAddress.country` | string | Canonical JSON | Medium — observed in canonical payloads only | US | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `consumer.billingAddress.firstName` | string | Canonical JSON | Medium — observed in canonical payloads only | <firstName> | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `consumer.billingAddress.lastName` | string | Canonical JSON | Medium — observed in canonical payloads only | <lastName> | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `consumer.billingAddress.postalCode` | string | Canonical JSON | Medium — observed in canonical payloads only | <postalCode> | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `consumer.billingAddress.state` | string | Canonical JSON | Medium — observed in canonical payloads only | <state> | Observed in: Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request; Optional Features > 4. Fraud Check Request |
| `consumer.consumerId` | string | Canonical JSON | Medium — observed in canonical payloads only | <consumerId> | Observed in: 3. Returning User with PMT from Vendor CDS > Example Request |
| `consumer.createCardOnFileIfNotFound` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'consumer.profile.account.save' instead. |
| `consumer.createConsumer` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | If a Consumer Profile is not found, automatically create a profile. Defaults to false. |
| `consumer.createConsumerProfileIfNotFound` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'consumer.createConsumer' instead. |
| `consumer.email` | string | Canonical JSON | Medium — observed in canonical payloads only | <emailAddress> | Observed in: Optional Features > 4. Fraud Check Request |
| `consumer.merchantConsumerId` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The merchant-facing value used to uniquely identify a Consumer |
| `consumer.merchantCustomerId` | string | Canonical JSON | Medium — observed in canonical payloads only | <merchantCustomerId> | Observed in: 4. Returning User with Merchant-Managed PMT > Example Request |
| `consumer.profile` | object | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `consumer.profile.account` | object | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `consumer.profile.account.return` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Returns the Token from the specified Consumer Profile that matches the CardIssuer passed in. |
| `consumer.profile.account.save` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | If the specified Consumer Profile is found, but an associated card on file is not, the specified token will automatically be added as a card on file. |
| `consumer.profile.account.updateExpirationDate` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | If true, External will use the card expiration dates passed in the request instead of the exp dates stored in CardStor but only if it is newer than CardStor. API will then update the CardStor with these dates. |
| `consumer.profile.account.use` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Use the Token found on the Consumer's Profile that matches the CardIssuer passed into API |
| `consumer.profile.accounts` | array | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `consumer.profile.accounts[].account` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The Token from the Consumer's Profile which matches the CardIssuer from the request. |
| `consumer.profile.accounts[].type` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The account type for this account. Will match card issuer that was passed in. |
| `consumer.profile.billingInformation` | object | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `consumer.profile.billingInformation.avsMatchThreshold` | string | Reference OAS A | Low/Legacy — present in outdated OAS only | enum: Strict, Partial, None | Defines how closely the Billing Address must match an entry within the Address Verification Service. |
| `consumer.profile.billingInformation.stopNotFound` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | If true and a Consumer Profile Billing Address is not found, stop processing and return error, else continue processing with the merchant-provided information or null if it does not exist. |
| `consumer.profile.billingInformation.update` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Update the Billing Address information in the Consumer Data Service. Requires MerchantConsumerId to be passed. |
| `consumer.profile.billingInformation.use` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Use the Billing Address information from the Consumer Data Service if one is found. Requires MerchantConsumerId, Payment Token, and CVV to be passed. |
| `consumer.refreshBillingAddressOnConsumerProfile` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'consumer.profile.billingInformation.update' instead. |
| `consumer.returnCardOnFileToken` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'consumer.profile.account.return' instead. |
| `consumer.stopOnConsumerProfileBillingAddressNotFound` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'consumer.profile.billingInformation.stopNotFound' instead. |
| `consumer.stopOnRefreshBillingAddressOnConsumerProfileFailure` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | This field is deprecated and has no effect. Values provided will be ignored. |
| `consumer.updateCardExpirationDate` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'consumer.profile.card.updateExpirationDate' instead. |
| `consumer.useConsumerProfileBillingAddress` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'consumer.profile.billingInformation.use' instead. |
| `consumer.useConsumerProfileTokenForPayment` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | This field is deprecated. Use 'consumer.profile.account.use' instead. |

### `payment`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `payment` | object | Canonical JSON, Reference OAS A | High |  | Observed in: 1. Sale — New Card with RSA > Example Request; 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Request |
| `payment.autoRefundOnVoidFailure` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Denotes an optional fallback behavior. Perform a Refund when a Void request fails due to batch timing. |
| `payment.cardInformation` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.cardInformation.brandCode` | string | Canonical JSON | Medium — observed in canonical payloads only | VS | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.cardInformation.maskedPan` | string | Canonical JSON | Medium — observed in canonical payloads only | 411111xxxxxx1111 | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.cardInformation.network` | string | Canonical JSON | Medium — observed in canonical payloads only | FREEWAY; VISA | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.cardInformation.type` | string | Canonical JSON | Medium — observed in canonical payloads only | credit | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.clientMetadata` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.clientMetadata.sellingMiddlewareName` | string | Canonical JSON | Medium — observed in canonical payloads only | <middlewareName> | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.clientMetadata.sellingMiddlewareVersion` | string | Canonical JSON | Medium — observed in canonical payloads only | <middlewareVersion> | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.clientMetadata.sellingSystemName` | string | Canonical JSON | Medium — observed in canonical payloads only | <systemName> | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.clientMetadata.sellingSystemVersion` | string | Canonical JSON | Medium — observed in canonical payloads only | <systemVersion> | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.fallbackAction` | string | Canonical JSON | Medium — observed in canonical payloads only | refund | Observed in: Other Transaction Operations > 9. Refund Fallback from Failed Void Response |
| `payment.invoice` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.invoice.invoiceHeader` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.invoice.invoiceHeader.invoiceNumber` | string | Canonical JSON | Medium — observed in canonical payloads only | <invoiceNumber> | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.invoice.purchaseTotals` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.invoice.purchaseTotals.chargeAmount` | number | Canonical JSON | Medium — observed in canonical payloads only | 6.0; 5.01; 0.0 | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.invoice.purchaseTotals.currency` | string | Canonical JSON | Medium — observed in canonical payloads only | USD | Observed in: Optional Features > 1. DCC Request |
| `payment.invoice.purchaseTotals.taxTotal` | number | Canonical JSON | Medium — observed in canonical payloads only | 0.41 | Observed in: 6. Refund — via Original Request ID > Example Request |
| `payment.merchantReferenceCode` | string | Canonical JSON | Medium — observed in canonical payloads only | <merchantReferenceCode> | Observed in: 1. Sale — New Card with RSA > Example Request; 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Request |
| `payment.networkData` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 6. Refund — via Original Request ID > Example Response; 7. Refund — via Payment Token > Example Response; 1. Apple Pay Sale > Example Response |
| `payment.networkData.network` | string | Canonical JSON | Medium — observed in canonical payloads only | FREEWAY | Observed in: 6. Refund — via Original Request ID > Example Response; 7. Refund — via Payment Token > Example Response; 1. Apple Pay Sale > Example Response |
| `payment.orderRequestId` | string | Canonical JSON | Medium — observed in canonical payloads only | <originalRequestId>; <authorizationRequestId> | Observed in: 4. Capture — Prior Authorization > Example Request; 5. Void — Prior Transaction > Example Request; 6. Refund — via Original Request ID > Example Request |
| `payment.originalAction` | string | Canonical JSON | Medium — observed in canonical payloads only | void | Observed in: Other Transaction Operations > 9. Refund Fallback from Failed Void Response |
| `payment.response` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.response.decision` | string | Canonical JSON | Medium — observed in canonical payloads only | REJECT; ACCEPT | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.response.reasonCode` | string | Canonical JSON | Medium — observed in canonical payloads only | 100; <reasonCode> | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.response.requestId` | string | Canonical JSON | Medium — observed in canonical payloads only | <voidRequestId>; <captureRequestId>; <refundRequestId> | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.serviceReplies` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.serviceReplies.dccReply` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: Optional Features > DCC Response |
| `payment.serviceReplies.dccReply.accepted` | boolean | Canonical JSON | Medium — observed in canonical payloads only | True | Observed in: Optional Features > DCC Response |
| `payment.serviceReplies.dccReply.exchangeRate` | number | Canonical JSON | Medium — observed in canonical payloads only | 0.925 | Observed in: Optional Features > DCC Response |
| `payment.serviceReplies.dccReply.exchangeRateSource` | string | Canonical JSON | Medium — observed in canonical payloads only | <exchangeRateSource> | Observed in: Optional Features > DCC Response |
| `payment.serviceReplies.dccReply.foreignAmount` | number | Canonical JSON | Medium — observed in canonical payloads only | 9.25 | Observed in: Optional Features > DCC Response |
| `payment.serviceReplies.dccReply.foreignCurrency` | string | Canonical JSON | Medium — observed in canonical payloads only | EUR | Observed in: Optional Features > DCC Response |
| `payment.serviceReplies.dccReply.reasonCode` | string | Canonical JSON | Medium — observed in canonical payloads only | 100 | Observed in: Optional Features > DCC Response |
| `payment.serviceReplies.fraudCheckReply` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: Optional Features > Fraud Check Response |
| `payment.serviceReplies.fraudCheckReply.decision` | string | Canonical JSON | Medium — observed in canonical payloads only | ACCEPT | Observed in: Optional Features > Fraud Check Response |
| `payment.serviceReplies.fraudCheckReply.processorResponseCode` | string | Canonical JSON | Medium — observed in canonical payloads only | A | Observed in: Optional Features > Fraud Check Response |
| `payment.serviceReplies.fraudCheckReply.reasonCode` | string | Canonical JSON | Medium — observed in canonical payloads only | 100 | Observed in: Optional Features > Fraud Check Response |
| `payment.serviceReplies.tokenCreateReply` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response; 1. Sale with Token Creation > Example Response; 2. Token-Only Verification without Sale > Example Response |
| `payment.serviceReplies.tokenCreateReply.paymentToken` | string | Canonical JSON | Medium — observed in canonical payloads only | <paymentToken> | Observed in: 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response; 1. Sale with Token Creation > Example Response; 2. Token-Only Verification without Sale > Example Response |
| `payment.serviceReplies.tokenCreateReply.reasonCode` | string | Canonical JSON | Medium — observed in canonical payloads only | 100 | Observed in: 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response; 1. Sale with Token Creation > Example Response; 2. Token-Only Verification without Sale > Example Response |
| `payment.serviceReplies.transactionReply` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.serviceReplies.transactionReply.accountBalance2Currency` | string | Canonical JSON | Medium — observed in canonical payloads only | USD | Observed in: 7. Refund — via Payment Token > Example Response |
| `payment.serviceReplies.transactionReply.amount` | number | Canonical JSON | Medium — observed in canonical payloads only | 6.0; 5.01; 0.0 | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.serviceReplies.transactionReply.authorizationCode` | string | Canonical JSON | Medium — observed in canonical payloads only | <authorizationCode> | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.serviceReplies.transactionReply.avsCode` | string | Canonical JSON | Medium — observed in canonical payloads only | M | Observed in: Optional Features > AVS / CVV Sale Response; Optional Features > Address Verification / ZVAV with AVS Fields Response |
| `payment.serviceReplies.transactionReply.cvCode` | string | Canonical JSON | Medium — observed in canonical payloads only | M | Observed in: Optional Features > AVS / CVV Sale Response; Optional Features > Address Verification / ZVAV with AVS Fields Response |
| `payment.serviceReplies.transactionReply.partialAmount` | boolean | Canonical JSON | Medium — observed in canonical payloads only | False | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.serviceReplies.transactionReply.processorResponseCode` | string | Canonical JSON | Medium — observed in canonical payloads only | 100 | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.serviceReplies.transactionReply.processorResponseMessage` | string | Canonical JSON | Medium — observed in canonical payloads only | APPROVED | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.serviceReplies.transactionReply.processorTransactionId` | string | Canonical JSON | Medium — observed in canonical payloads only | <processorTransactionId> | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.serviceReplies.transactionReply.purchasingLevel3Enabled` | boolean | Canonical JSON | Medium — observed in canonical payloads only | False | Observed in: 6. Refund — via Original Request ID > Example Response; 7. Refund — via Payment Token > Example Response |
| `payment.serviceReplies.transactionReply.reasonCode` | string | Canonical JSON | Medium — observed in canonical payloads only | 100 | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.serviceReplies.transactionReply.reconciliationId` | string | Canonical JSON | Medium — observed in canonical payloads only | <reconciliationId> | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.serviceReplies.transactionReply.requestDateTime` | string | Canonical JSON | Medium — observed in canonical payloads only | <requestDateTime> | Observed in: 1. Sale — New Card with RSA > Example Response; 2. Sale — Existing Payment Token > Example Response; 3. Authorization — New Card with RSA > Example Response |
| `payment.services` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.services.dccService` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: Optional Features > 1. DCC Request |
| `payment.services.dccService.run` | boolean | Canonical JSON | Medium — observed in canonical payloads only | True | Observed in: Optional Features > 1. DCC Request |
| `payment.services.fraudCheckService` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: Optional Features > 4. Fraud Check Request |
| `payment.services.fraudCheckService.run` | boolean | Canonical JSON | Medium — observed in canonical payloads only | True | Observed in: Optional Features > 4. Fraud Check Request |
| `payment.services.tokenCreateService` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request; 1. Sale with Token Creation > Example Request; 2. Token-Only Verification without Sale > Example Request |
| `payment.services.tokenCreateService.run` | boolean | Canonical JSON | Medium — observed in canonical payloads only | True | Observed in: 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request; 1. Sale with Token Creation > Example Request; 2. Token-Only Verification without Sale > Example Request |
| `payment.services.transactionService` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.services.transactionService.commerceIndicator` | string | Canonical JSON | Medium — observed in canonical payloads only | internet | Observed in: 6. Refund — via Original Request ID > Example Request; Optional Features > 2. AVS / CVV Sale Request; Optional Features > 3. Address Verification / ZVAV with AVS Fields Request |
| `payment.services.transactionService.transactionType` | string | Canonical JSON | Medium — observed in canonical payloads only | capture; authorization; void | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.tenders` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.tenders.alternativePayment` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 3. PayPal / Redirect APM Sale > Example Request; 4. ATH Móvil Sale > Example Request; 1. APM Sale Request |
| `payment.tenders.alternativePayment.paymentMethod` | string | Canonical JSON | Medium — observed in canonical payloads only | paypal; athmovil; <paymentMethod> | Observed in: 3. PayPal / Redirect APM Sale > Example Request; 4. ATH Móvil Sale > Example Request; 1. APM Sale Request |
| `payment.tenders.card` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 2. Sale — Existing Payment Token > Example Request; 3. Authorization — New Card with RSA > Example Request |
| `payment.tenders.card.accountNumber` | string | Canonical JSON | Medium — observed in canonical payloads only | <dpan>; <paymentToken>; <mpan> | Observed in: 2. Sale — Existing Payment Token > Example Request; 7. Refund — via Payment Token > Example Request; 3. Returning User with PMT from Vendor CDS > Example Request |
| `payment.tenders.card.cardType` | string | Canonical JSON | Medium — observed in canonical payloads only | token | Observed in: 2. Sale — Existing Payment Token > Example Request; 7. Refund — via Payment Token > Example Request; 3. Returning User with PMT from Vendor CDS > Example Request |
| `payment.tenders.card.encryptedData` | string | Canonical JSON | Medium — observed in canonical payloads only | <applePayPaymentToken>; <googlePayPaymentToken>; <encryptedCardData> | Observed in: 1. Sale — New Card with RSA > Example Request; 3. Authorization — New Card with RSA > Example Request; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request |
| `payment.tenders.pos` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Sale — New Card with RSA > Example Request; 3. Authorization — New Card with RSA > Example Request; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request |
| `payment.tenders.pos.encMode` | string | Canonical JSON | Medium — observed in canonical payloads only | googlewallet; rsa; applewallet | Observed in: 1. Sale — New Card with RSA > Example Request; 3. Authorization — New Card with RSA > Example Request; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request |
| `payment.tenders.pos.entryMode` | string | Canonical JSON | Medium — observed in canonical payloads only | keyed; inapp | Observed in: 1. Sale — New Card with RSA > Example Request; 3. Authorization — New Card with RSA > Example Request; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request |
| `payment.tenders.pos.msrType` | string | Canonical JSON | Medium — observed in canonical payloads only | applepay; none; googlepay | Observed in: 1. Sale — New Card with RSA > Example Request; 3. Authorization — New Card with RSA > Example Request; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request |
| `payment.tenders.pos.trackKsn` | string | Canonical JSON | Medium — observed in canonical payloads only | <trackKsn> | Observed in: 1. Sale — New Card with RSA > Example Request; 3. Authorization — New Card with RSA > Example Request; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Request |
| `payment.tokenInformation` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 2. Sale — Existing Payment Token > Example Response; 7. Refund — via Payment Token > Example Response; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response |
| `payment.tokenInformation.accountNumberMasked` | string | Canonical JSON | Medium — observed in canonical payloads only | 411111xxxxxx1111 | Observed in: 2. Sale — Existing Payment Token > Example Response; 7. Refund — via Payment Token > Example Response; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response |
| `payment.tokenInformation.brand` | string | Canonical JSON | Medium — observed in canonical payloads only | VS | Observed in: 2. Sale — Existing Payment Token > Example Response; 7. Refund — via Payment Token > Example Response; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response |
| `payment.tokenInformation.cardExpirationMonth` | integer | Canonical JSON | Medium — observed in canonical payloads only | 11 | Observed in: 2. Sale — Existing Payment Token > Example Response; 7. Refund — via Payment Token > Example Response; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response |
| `payment.tokenInformation.cardExpirationYear` | integer | Canonical JSON | Medium — observed in canonical payloads only | 29 | Observed in: 2. Sale — Existing Payment Token > Example Response; 7. Refund — via Payment Token > Example Response; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response |
| `payment.tokenInformation.cardType` | string | Canonical JSON | Medium — observed in canonical payloads only | credit | Observed in: 2. Sale — Existing Payment Token > Example Response; 7. Refund — via Payment Token > Example Response; 8. ZVAV — Zero Value Authorization Verification / Token Creation > Example Response |

### `eCommerce`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `eCommerce` | object | Canonical JSON, Reference OAS A | High |  | Observed in: 6. Refund — via Original Request ID > Example Response; 7. Refund — via Payment Token > Example Response; 3. PayPal / Redirect APM Sale > Example Request |
| `eCommerce.apmOptions` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.apmOptions.qrCodeHeight` | integer | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Height of the QRCode image |
| `eCommerce.apmOptions.qrCodeWidth` | integer | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Width of the QRCode image |
| `eCommerce.apmOptions.returnQrCodeImage` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Apm Url is always returned when applicable. If a scannable QRCode is desired, pass true for this. |
| `eCommerce.apmOptions.thirdPartyIdentifier` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | A unique identifier required by some third parties to uniquely identify a payment session |
| `eCommerce.apmQrCode` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Base64 representation of the QrCode in png format |
| `eCommerce.apmUrl` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Url to the Apm payment system |
| `eCommerce.cancelUrl` | string | Canonical JSON | Medium — observed in canonical payloads only | <cancelUrl> | Observed in: 3. PayPal / Redirect APM Sale > Example Request; 4. ATH Móvil Sale > Example Request; 5. Browser 3DS — Initial Sale Request > Example Request |
| `eCommerce.cardIssuer` | string | Reference OAS B | Low/Legacy — present in outdated OAS only | enum: alipay, athmovilecom, athmovilinstore, citidloc, citimil, cnh, klarna, openbanking, paypal, venmo | Identifies the desired payment method when it cannot be derived by bin check. Applies primarily to Alternate Payment Methods and some private label accounts. |
| `eCommerce.clientPaymentKey` | string | Canonical JSON | Medium — observed in canonical payloads only | <clientPaymentKey> | Observed in: 6. Refund — via Original Request ID > Example Response; 7. Refund — via Payment Token > Example Response; 1. Mobile 3DS — Initial Sale Request > Example Response — 3DS Required |
| `eCommerce.reasonCode` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Provides additional information for the eCommerce portion of the operation. Values in the range of 900-999. |
| `eCommerce.reasonCode.code` | integer | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Numerical representation of the ReasonCode |
| `eCommerce.reasonCode.description` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Textual representation of the ReasonCode |
| `eCommerce.redirectUrl` | string | Canonical JSON | Medium — observed in canonical payloads only | <redirectUrl> | Observed in: 3. PayPal / Redirect APM Sale > Example Response — Redirect Required; 4. ATH Móvil Sale > Example Response — Redirect Required; 5. Browser 3DS — Initial Sale Request > Example Response — Browser Challenge Required |
| `eCommerce.returnStatus` | string | Canonical JSON | Medium — observed in canonical payloads only | cancelled; expired | Observed in: 4. APM Response — Customer Cancelled; 5. APM Response — Payment Expired; 7. APM Full Webhook Callback — Cancelled Payment |
| `eCommerce.returnUrl` | string | Canonical JSON | Medium — observed in canonical payloads only | <returnUrl> | Observed in: 3. PayPal / Redirect APM Sale > Example Request; 4. ATH Móvil Sale > Example Request; 5. Browser 3DS — Initial Sale Request > Example Request |
| `eCommerce.serviceProviderCodes` | array | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.serviceProviderCodes[].serviceProviderCode` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Code returned by the third party |
| `eCommerce.serviceProviderCodes[].serviceProviderMessage` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | The message provided by the third party |
| `eCommerce.serviceProviderCodes[].serviceProviderName` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | The Third Party Provider |
| `eCommerce.threeDS` | object | Canonical JSON | Medium — observed in canonical payloads only |  | Observed in: 1. Mobile 3DS — Initial Sale Request > Example Request; 1. Mobile 3DS — Initial Sale Request > Example Response — 3DS Required; 2. Mobile 3DS — Finalization Request After Successful Challenge > Example Request |
| `eCommerce.threeDS.channel` | string | Canonical JSON | Medium — observed in canonical payloads only | mobile; browser | Observed in: 1. Mobile 3DS — Initial Sale Request > Example Request; 1. Mobile 3DS — Initial Sale Request > Example Response — 3DS Required; 5. Browser 3DS — Initial Sale Request > Example Request |
| `eCommerce.threeDS.enabled` | boolean | Canonical JSON | Medium — observed in canonical payloads only | True | Observed in: 1. Mobile 3DS — Initial Sale Request > Example Request; 5. Browser 3DS — Initial Sale Request > Example Request |
| `eCommerce.threeDS.required` | boolean | Canonical JSON | Medium — observed in canonical payloads only | True; False | Observed in: 1. Mobile 3DS — Initial Sale Request > Example Response — 3DS Required; 4. Mobile 3DS — Frictionless / Challenge Not Needed > Example Response; 5. Browser 3DS — Initial Sale Request > Example Response — Browser Challenge Requir… |
| `eCommerce.threeDS.result` | string | Canonical JSON | Medium — observed in canonical payloads only | challengeSuccessful; errorAtThreeDSProvider; challengeCancelledByCustomer | Observed in: 2. Mobile 3DS — Finalization Request After Successful Challenge > Example Request; 3. Mobile 3DS — Finalization Request After Challenge Failed > Example Request; 4. Mobile 3DS — Frictionless / Challenge Not Needed > Example Res… |
| `eCommerce.threeDSOptions` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSOptions.account` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSOptions.account.ageIndicator` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Length of time that the cardholder had an account with the 3DS requestor. Options 01, 02, 03, 04 and 05. |
| `eCommerce.threeDSOptions.account.changeIndicator` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Length of time since the cardholder's account information with the 3DS Requestor was last changed, including Billing or Shipping address, new payment account, or new user added. Options 01, 02, 03 and 04. |
| `eCommerce.threeDSOptions.account.changedDate` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | The last changed date of the cardholder's account with the 3DS Requestor, including Billing or Shipping address, new payment account, or new user added. Format YYYYMMDD. |
| `eCommerce.threeDSOptions.account.dateOpened` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Date that the cardholder opened the account with the 3DS Requestor. Format YYYYMMDD |
| `eCommerce.threeDSOptions.account.identifier` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Additional information about the account that is optionally provided by the 3DS Requestor. |
| `eCommerce.threeDSOptions.account.passwordChangeIndicator` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Length of time since the cardholder's account with the 3DS Requestor had a password change or account reset. Options 01,02,03, 04 and 05. |
| `eCommerce.threeDSOptions.account.passwordChangedDate` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Date that cardholder's account with the 3DS Requestor had a password change or account reset. Format YYYYMMDD. |
| `eCommerce.threeDSOptions.account.paymentAccountAgeDate` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Date on which the payment account was enrolled in the cardholder's account with the 3DS Requestor. Format YYYYMMDD. |
| `eCommerce.threeDSOptions.account.paymentAccountIndicator` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Indicates the length of time that the payment account was enrolled in the cardholder's account with the 3DS Requestor. Options 01, 02, 03, 04 and 05. |
| `eCommerce.threeDSOptions.account.provisionAttemptsDay` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Number of Add Card attempts in the last 24 hours. |
| `eCommerce.threeDSOptions.account.purchasesInLastSixMonths` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Number of purchases with the cardholder's account during the previous six months. |
| `eCommerce.threeDSOptions.account.shippingAddressUsageDate` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Indicates the date when the shipping address was first used with the 3DS Requestor for the transaction. Format YYYYMMDD. |
| `eCommerce.threeDSOptions.account.shippingAddressUsageIndicator` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Indicates when the shipping address used for this transaction was first used with the 3DS Requestor. Options 01, 02, 03 and 04. |
| `eCommerce.threeDSOptions.account.shippingNameIndicator` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Indicates if the cardholder's name on the account is identical to the shipping name used for this transaction. Options 01=Same, 02=Different. |
| `eCommerce.threeDSOptions.account.suspiciousAccountActivityIndicator` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Suspicious activity (including previous fraud occurrence) indicator on the cardholder account. Options 01=No Suspicious Activity, 02=Suspicious Activity. |
| `eCommerce.threeDSOptions.account.txnActivityDay` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Number of transactions for the cardholder's account with the 3DS Requestor across all payment accounts in the previous 24 hours. |
| `eCommerce.threeDSOptions.account.txnActivityYear` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Number of transactions for the cardholder's account with the 3DS Requestor across all payment accounts in the previous year. |
| `eCommerce.threeDSOptions.account.type` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Indicates the type of account. For example, for a multi-account card product. |
| `eCommerce.threeDSOptions.addressesMustMatch` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Indicates that the Bill-To and Ship-To addresses are identical |
| `eCommerce.threeDSOptions.authenticationIndicator` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Indicates the type of Authentication request. This data element provides additional information to the ACS to determine the best approach for handling an authentication request. |
| `eCommerce.threeDSOptions.challengeIndicator` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Indicates whether a challenge is requested for this transaction. For example, for 01-PA, a 3DS Requestor may have concerns about the transaction, and request a challenge. For 02-NPA, a challenge may be necessary when adding a new card to a … |
| `eCommerce.threeDSOptions.challengeWindowSize` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Required if generateCReq is present |
| `eCommerce.threeDSOptions.clientType` | string | Reference OAS B | Low/Legacy — present in outdated OAS only | enum: browserJs, mobileSdk |  |
| `eCommerce.threeDSOptions.merchantDomainName` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | The domain name of the merchant website, this will help to handle the CORS issue in three DS flow |
| `eCommerce.threeDSOptions.mobileSdkRenderOptions` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSOptions.mobileSdkRenderOptions.interface` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Lists all the SDK Interface types that the device supports for displaying specific challenge user interfaces within the SDK. Values accepted are 01=Native, 02=HTML, 03=Both |
| `eCommerce.threeDSOptions.mobileSdkRenderOptions.uiType` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Lists all the UI types that the device supports for displaying specific challenge user interfaces within the SDK. Values accepted are Native UI=01-04; HTML UI=01-05. |
| `eCommerce.threeDSOptions.purchaseInstallData` | integer | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Indicates the maximum number of authorizations permitted for installment payments. |
| `eCommerce.threeDSOptions.recurrence` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSOptions.recurrence.expirationDate` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Date after which no further authorizations will be performed. Format YYYYMMDD. |
| `eCommerce.threeDSOptions.recurrence.frequency` | integer | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Indicates the minimum number of days between authorizations. |
| `eCommerce.threeDSOptions.resultDrivenProcessing` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  | This will instruct the API what actions needed to be taken once the 3ds outcome has received |
| `eCommerce.threeDSOptions.resultDrivenProcessing.enabled` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Default true, If false, this will instruct the system that 3ds outcome will have no impact on the transaction processing |
| `eCommerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  | This will instruct how to process specific 3ds outcome |
| `eCommerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSAuthenticationAttempted` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | ??? |
| `eCommerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSAuthenticationFailed` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Default false, if true, the payment processing will continue even when the 3ds authentication faild. |
| `eCommerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSAuthenticationSuccessful` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Default true, If true, this will process payment if 3ds authentication was successful |
| `eCommerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSCardNotSupported` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Default false, if true, the payment processing will continue even when the card doesn't support 3ds. |
| `eCommerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSRejected` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Default false, if true, the payment processing will continue even when the 3ds is rejected. |
| `eCommerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSUnavailable` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Default false, if true, the payment processing will continue even when the 3ds authentication was unavailable. |
| `eCommerce.threeDSOptions.runThreeDsAuthentication` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Default true, this indicates if the consumer wants to run the 3DS authentication or not. This will override the store config value |
| `eCommerce.threeDSOptions.transactionType` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Identifies the type of transaction being authenticated. This field is required for VISA transactions and accepted values are 01, 03, 10, 11, and 28. |
| `eCommerce.threeDSProviderResponse` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.acs` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.acs.challengeMandated` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Indication of whether a challenge is required for the transaction to be authorised due to local or regional mandates or other variable. |
| `eCommerce.threeDSProviderResponse.acs.operatorIdentifer` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | DS-assigned ACS identifier. Each DS can provide a unique ID to each ACS. |
| `eCommerce.threeDSProviderResponse.acs.referenceNumber` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Unique identifier assigned by the EMVCo Secretariat upon testing and approval. |
| `eCommerce.threeDSProviderResponse.acs.renderingType` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.acs.renderingType.acsInterface` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Identifies the ACS interface that presents the challenge to the cardholder. Options are 01, 02. |
| `eCommerce.threeDSProviderResponse.acs.renderingType.acsUiTemplate` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Identifies the UI Template format that the ACS presents to the consumer first. Options are 01, 02, 03, 04, 05. |
| `eCommerce.threeDSProviderResponse.acs.transactionId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Universally Unique transaction identifier assigned by the ACS to identify a single transaction. Format uuid. |
| `eCommerce.threeDSProviderResponse.acs.url` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Fully-qualified URL of the ACS to be used for the challenge. |
| `eCommerce.threeDSProviderResponse.authenticationValue` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.cardHolderInformation` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.challengeCancelledIndicator` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.dsTransactionId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.eci` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.error` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.error.code` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.error.component` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.error.description` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.interactionCounter` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.message` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.message.version` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.sdk` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.sdk.transactionId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.threeDsResult` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.threeDsServerTransactionId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.transaction` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.transaction.status` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.transaction.statusReason` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.trustList` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.trustList.status` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `eCommerce.threeDSProviderResponse.trustList.statusSource` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |

### `instore`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `instore` | object | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |

### `device`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `device` | object | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `device.applicationName` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `device.id` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `device.proceedWithAuth` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `device.registrationId` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `device.requests` | array | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `device.responses` | array | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `device.type` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `device.version` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |

### `threeDSProgressRequest`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `threeDSProgressRequest.browserCharacteristics` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.browserCharacteristics.acceptHeader` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.browserCharacteristics.colorDepth` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.browserCharacteristics.ipAddress` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.browserCharacteristics.javaEnabled` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.browserCharacteristics.javascriptEnabled` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.browserCharacteristics.language` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.browserCharacteristics.screenHeight` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.browserCharacteristics.screenWidth` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.browserCharacteristics.timeZoneOffset` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.browserCharacteristics.userAgent` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.clientErrors` | array | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.clientErrors[].message` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.contextId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Uniquely identifies a unit of work. Provide when performing an asynchronous workflow that spans multiple calls. |
| `threeDSProgressRequest.mobileSdkData` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.mobileSdkData.applicationId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.mobileSdkData.encData` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.mobileSdkData.maxTimeout` | integer | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Time in minutes for the entire 3DS workflow including Auth/Challenge/etc. Minimum of 5 minutes, maximum of 60. |
| `threeDSProgressRequest.mobileSdkData.publicKey` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.mobileSdkData.publicKey.crv` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.mobileSdkData.publicKey.keyType` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.mobileSdkData.publicKey.x` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.mobileSdkData.publicKey.y` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.mobileSdkData.referenceNumber` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Identifies the vendor and version for the 3DS SDK that is integrated in a 3DS Requestor App, assigned by EMVCo when the 3DS SDK is approved. |
| `threeDSProgressRequest.mobileSdkData.transactionId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Universally unique transaction identifier assigned by the 3DS SDK to identify a single transaction. |
| `threeDSProgressRequest.trackKsn` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressRequest.tracke` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |

### `threeDSProgressResponse`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `threeDSProgressResponse.challengeAcsUrl` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | The Url for the customer challenge if 3dsOptions.clientType is 'browserJs' |
| `threeDSProgressResponse.clientPollInterval` | integer | Reference OAS B | Low/Legacy — present in outdated OAS only |  | How many seconds between poll attempts |
| `threeDSProgressResponse.clientPollWindow` | integer | Reference OAS B | Low/Legacy — present in outdated OAS only |  | The maximum seconds to poll |
| `threeDSProgressResponse.merchantNotifiedOfTransactionResult` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Denotes that Vendor API invoked the Merchant callback endpoint |
| `threeDSProgressResponse.methodUrl` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | The Url for the device data collection if the 3dsOptions.ClientType is 'browserJs' |
| `threeDSProgressResponse.mobileSdkData` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.mobileSdkData.dsCaCertificate` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey.e` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey.kty` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey.n` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKeyId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.status` | string | Reference OAS B | Low/Legacy — present in outdated OAS only | enum: CardDataMissing, NotApplicable, DeviceDataRequired, ChallengeRequired, ChallengeNotNeeded, ErrorAt3dsProvider, ChallengeSuccessful, ChallengeFailed, ChallengeCancelledByCustomer | The status of the 3DS workflow at the time it was polled |
| `threeDSProgressResponse.threeDSEncodedChallengeRequestData` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Base64 URL encoded CReq message, only returned for challenge transactions during browser flow |
| `threeDSProgressResponse.threeDSProvider` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.acs` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.acs.challengeMandated` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Indication of whether a challenge is required for the transaction to be authorised due to local or regional mandates or other variable. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.operatorIdentifer` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | DS-assigned ACS identifier. Each DS can provide a unique ID to each ACS. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.referenceNumber` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Unique identifier assigned by the EMVCo Secretariat upon testing and approval. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.renderingType` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.acs.renderingType.acsInterface` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Identifies the ACS interface that presents the challenge to the cardholder. Options are 01, 02. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.renderingType.acsUiTemplate` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Identifies the UI Template format that the ACS presents to the consumer first. Options are 01, 02, 03, 04, 05. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.transactionId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Universally Unique transaction identifier assigned by the ACS to identify a single transaction. Format uuid. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.url` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Fully-qualified URL of the ACS to be used for the challenge. |
| `threeDSProgressResponse.threeDSProviderResponse.authenticationValue` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.cardHolderInformation` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.challengeCancelledIndicator` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.dsTransactionId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.eci` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.error` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.error.code` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.error.component` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.error.description` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.interactionCounter` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.message` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.message.version` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.sdk` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.sdk.transactionId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.threeDsResult` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.threeDsServerTransactionId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.transaction` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.transaction.status` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.transaction.statusReason` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.trustList` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.trustList.status` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderResponse.trustList.statusSource` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProgressResponse.threeDSProviderTransactionId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | The provider specific transactionId that is the unique identifer for the 3ds session. |
| `threeDSProgressResponse.transactionComplete` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | The transaction has been processed and is complete. Full information available on the introspection endpoint |

### `threeDSProviderResponse`

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `threeDSProviderResponse.acs` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.acs.challengeMandated` | boolean | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Indication of whether a challenge is required for the transaction to be authorised due to local or regional mandates or other variable. |
| `threeDSProviderResponse.acs.operatorIdentifer` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | DS-assigned ACS identifier. Each DS can provide a unique ID to each ACS. |
| `threeDSProviderResponse.acs.referenceNumber` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Unique identifier assigned by the EMVCo Secretariat upon testing and approval. |
| `threeDSProviderResponse.acs.renderingType` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.acs.renderingType.acsInterface` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Identifies the ACS interface that presents the challenge to the cardholder. Options are 01, 02. |
| `threeDSProviderResponse.acs.renderingType.acsUiTemplate` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Identifies the UI Template format that the ACS presents to the consumer first. Options are 01, 02, 03, 04, 05. |
| `threeDSProviderResponse.acs.transactionId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Universally Unique transaction identifier assigned by the ACS to identify a single transaction. Format uuid. |
| `threeDSProviderResponse.acs.url` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  | Fully-qualified URL of the ACS to be used for the challenge. |
| `threeDSProviderResponse.authenticationValue` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.cardHolderInformation` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.challengeCancelledIndicator` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.dsTransactionId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.eci` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.error` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.error.code` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.error.component` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.error.description` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.interactionCounter` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.message` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.message.version` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.sdk` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.sdk.transactionId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.threeDsResult` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.threeDsServerTransactionId` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.transaction` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.transaction.status` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.transaction.statusReason` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.trustList` | object | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.trustList.status` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |
| `threeDSProviderResponse.trustList.statusSource` | string | Reference OAS B | Low/Legacy — present in outdated OAS only |  |  |

### Other / Endpoint-Specific Fields

| Field Path | Type(s) | Source | Confidence | Examples / Enum | Notes |
|---|---|---|---|---|---|
| `b64Endianness` | string | Reference OAS A | Low/Legacy — present in outdated OAS only | enum: be, le | Options for b64 endianness if format is modexp |
| `b64Signedness` | string | Reference OAS A | Low/Legacy — present in outdated OAS only | enum: signed, unsigned | Options for b64 signedness if format is modexp |
| `certificate` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | x509 Certificate containing the public key if the format specified is x509 |
| `consumerRequestAccountOptions.return` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Returns the Token from the specified Consumer Profile that matches the CardIssuer passed in. |
| `consumerRequestAccountOptions.save` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | If the specified Consumer Profile is found, but an associated card on file is not, the specified token will automatically be added as a card on file. |
| `consumerRequestAccountOptions.updateExpirationDate` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | If true, External will use the card expiration dates passed in the request instead of the exp dates stored in CardStor but only if it is newer than CardStor. API will then update the CardStor with these dates. |
| `consumerRequestAccountOptions.use` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Use the Token found on the Consumer's Profile that matches the CardIssuer passed into API |
| `consumerRequestBillingInformationOptions.avsMatchThreshold` | string | Reference OAS A | Low/Legacy — present in outdated OAS only | enum: Strict, Partial, None | Defines how closely the Billing Address must match an entry within the Address Verification Service. |
| `consumerRequestBillingInformationOptions.stopNotFound` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | If true and a Consumer Profile Billing Address is not found, stop processing and return error, else continue processing with the merchant-provided information or null if it does not exist. |
| `consumerRequestBillingInformationOptions.update` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Update the Billing Address information in the Consumer Data Service. Requires MerchantConsumerId to be passed. |
| `consumerRequestBillingInformationOptions.use` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Use the Billing Address information from the Consumer Data Service if one is found. Requires MerchantConsumerId, Payment Token, and CVV to be passed. |
| `consumerRequestProfileOptions.account` | object | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `consumerRequestProfileOptions.account.return` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Returns the Token from the specified Consumer Profile that matches the CardIssuer passed in. |
| `consumerRequestProfileOptions.account.save` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | If the specified Consumer Profile is found, but an associated card on file is not, the specified token will automatically be added as a card on file. |
| `consumerRequestProfileOptions.account.updateExpirationDate` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | If true, External will use the card expiration dates passed in the request instead of the exp dates stored in CardStor but only if it is newer than CardStor. API will then update the CardStor with these dates. |
| `consumerRequestProfileOptions.account.use` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Use the Token found on the Consumer's Profile that matches the CardIssuer passed into API |
| `consumerRequestProfileOptions.billingInformation` | object | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `consumerRequestProfileOptions.billingInformation.avsMatchThreshold` | string | Reference OAS A | Low/Legacy — present in outdated OAS only | enum: Strict, Partial, None | Defines how closely the Billing Address must match an entry within the Address Verification Service. |
| `consumerRequestProfileOptions.billingInformation.stopNotFound` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | If true and a Consumer Profile Billing Address is not found, stop processing and return error, else continue processing with the merchant-provided information or null if it does not exist. |
| `consumerRequestProfileOptions.billingInformation.update` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Update the Billing Address information in the Consumer Data Service. Requires MerchantConsumerId to be passed. |
| `consumerRequestProfileOptions.billingInformation.use` | boolean | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Use the Billing Address information from the Consumer Data Service if one is found. Requires MerchantConsumerId, Payment Token, and CVV to be passed. |
| `consumerResponse.profile` | object | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `consumerResponse.profile.accounts` | array | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `consumerResponse.profile.accounts[].account` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The Token from the Consumer's Profile which matches the CardIssuer from the request. |
| `consumerResponse.profile.accounts[].type` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The account type for this account. Will match card issuer that was passed in. |
| `consumerResponseProfile.accounts` | array | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `consumerResponseProfile.accounts[].account` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The Token from the Consumer's Profile which matches the CardIssuer from the request. |
| `consumerResponseProfile.accounts[].type` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The account type for this account. Will match card issuer that was passed in. |
| `consumerResponseProfileAccount.account` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The Token from the Consumer's Profile which matches the CardIssuer from the request. |
| `consumerResponseProfileAccount.type` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The account type for this account. Will match card issuer that was passed in. |
| `contextId` | string | Canonical JSON, Reference OAS A | High | <contextId> | Uniquely identifies a unit of work. Provide when performing an asynchronous workflow that spans multiple calls. This value is provided by Vendor in all responses. |
| `data` | object | Reference OAS A | Low/Legacy — present in outdated OAS only |  |  |
| `enterpriseCode` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The unique Vendor-supplied value that represents the enterprise |
| `expSubformat` | string | Reference OAS A | Low/Legacy — present in outdated OAS only | enum: b64, hex, dec | Specifies the subformat for exponent if format is modexp |
| `expirationDatetime` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Not-valid-after datetime for the key |
| `exponent` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Exponent e for RSA Encryption |
| `format` | string | Reference OAS A | Low/Legacy — present in outdated OAS only | enum: x509, spki, rpk, modexp | The format of the public key being returned. |
| `keySlotId` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The Key Slot Id set created within Vendor's Enterprise Portal. |
| `merchantPropertyCode` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Merchant property code, required if storeID not provided |
| `modSubformat` | string | Reference OAS A | Low/Legacy — present in outdated OAS only | enum: b64, hex, dec | Specifies the subformat for modulus if format is modexp |
| `modulus` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Modulus n for RSA Encryption |
| `publicKey` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The RSA public key in the specified format |
| `storeId` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | The unique Vendor-supplied value that represents a Store, required only if merchantPropertyCode is not provided |
| `subformat` | string | Reference OAS A | Low/Legacy — present in outdated OAS only | enum: b64der, pem | Specifies the subformat for x509, spki, and rpk |
| `trackKsn` | string | Reference OAS A | Low/Legacy — present in outdated OAS only |  | Value to be passed in the trackKsn field in the Payment Request to identify the correct key |

## JSON-Only Fields Requiring Schema Review

These fields are observed in canonical payloads but were not found in the parsed legacy OAS files. Many are likely legitimate current API fields, especially under `payment.*` and `workflow.*`, because the legacy OAS references external schemas that were not included.

| Field Path | Type(s) | Observed Count | Source Files | Example Values |
|---|---|---:|---|---|
| `consumer.billingAddress` | object | 3 | canonical-json-payloads-optional-other.md |  |
| `consumer.billingAddress.address1` | string | 3 | canonical-json-payloads-optional-other.md | <addressLine1> |
| `consumer.billingAddress.city` | string | 3 | canonical-json-payloads-optional-other.md | <city> |
| `consumer.billingAddress.country` | string | 3 | canonical-json-payloads-optional-other.md | US |
| `consumer.billingAddress.firstName` | string | 3 | canonical-json-payloads-optional-other.md | <firstName> |
| `consumer.billingAddress.lastName` | string | 3 | canonical-json-payloads-optional-other.md | <lastName> |
| `consumer.billingAddress.postalCode` | string | 3 | canonical-json-payloads-optional-other.md | <postalCode> |
| `consumer.billingAddress.state` | string | 3 | canonical-json-payloads-optional-other.md | <state> |
| `consumer.consumerId` | string | 1 | canonical-json-payloads-tokenization-variants.md | <consumerId> |
| `consumer.email` | string | 1 | canonical-json-payloads-optional-other.md | <emailAddress> |
| `consumer.merchantCustomerId` | string | 1 | canonical-json-payloads-tokenization-variants.md | <merchantCustomerId> |
| `eCommerce.cancelUrl` | string | 4 | canonical-json-payloads-3ds.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-wallets.md | <cancelUrl> |
| `eCommerce.clientPaymentKey` | string | 7 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md | <clientPaymentKey> |
| `eCommerce.redirectUrl` | string | 4 | canonical-json-payloads-3ds.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-wallets.md | <redirectUrl> |
| `eCommerce.returnStatus` | string | 6 | canonical-json-payloads-lifecycle.md | cancelled; expired |
| `eCommerce.returnUrl` | string | 4 | canonical-json-payloads-3ds.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-wallets.md | <returnUrl> |
| `eCommerce.threeDS` | object | 10 | canonical-json-payloads-3ds.md |  |
| `eCommerce.threeDS.channel` | string | 4 | canonical-json-payloads-3ds.md | mobile; browser |
| `eCommerce.threeDS.enabled` | boolean | 2 | canonical-json-payloads-3ds.md | True |
| `eCommerce.threeDS.required` | boolean | 3 | canonical-json-payloads-3ds.md | True; False |
| `eCommerce.threeDS.result` | string | 6 | canonical-json-payloads-3ds.md | challengeSuccessful; errorAtThreeDSProvider; challengeCancelledByCustomer |
| `payment.cardInformation` | object | 20 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `payment.cardInformation.brandCode` | string | 20 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | VS |
| `payment.cardInformation.maskedPan` | string | 20 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | 411111xxxxxx1111 |
| `payment.cardInformation.network` | string | 20 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | FREEWAY; VISA |
| `payment.cardInformation.type` | string | 20 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | credit |
| `payment.clientMetadata` | object | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `payment.clientMetadata.sellingMiddlewareName` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <middlewareName> |
| `payment.clientMetadata.sellingMiddlewareVersion` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <middlewareVersion> |
| `payment.clientMetadata.sellingSystemName` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <systemName> |
| `payment.clientMetadata.sellingSystemVersion` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <systemVersion> |
| `payment.fallbackAction` | string | 1 | canonical-json-payloads-optional-other.md | refund |
| `payment.invoice` | object | 30 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `payment.invoice.invoiceHeader` | object | 30 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `payment.invoice.invoiceHeader.invoiceNumber` | string | 30 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <invoiceNumber> |
| `payment.invoice.purchaseTotals` | object | 29 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `payment.invoice.purchaseTotals.chargeAmount` | number | 29 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | 6.0; 5.01; 0.0 |
| `payment.invoice.purchaseTotals.currency` | string | 1 | canonical-json-payloads-optional-other.md | USD |
| `payment.invoice.purchaseTotals.taxTotal` | number | 1 | canonical-json-payloads-core.md | 0.41 |
| `payment.merchantReferenceCode` | string | 85 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <merchantReferenceCode> |
| `payment.networkData` | object | 8 | canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-wallets.md |  |
| `payment.networkData.network` | string | 8 | canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-wallets.md | FREEWAY |
| `payment.orderRequestId` | string | 8 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md | <originalRequestId>; <authorizationRequestId> |
| `payment.originalAction` | string | 1 | canonical-json-payloads-optional-other.md | void |
| `payment.response` | object | 37 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `payment.response.decision` | string | 37 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | REJECT; ACCEPT |
| `payment.response.reasonCode` | string | 37 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | 100; <reasonCode> |
| `payment.response.requestId` | string | 37 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <voidRequestId>; <captureRequestId>; <refundRequestId> |
| `payment.serviceReplies` | object | 34 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `payment.serviceReplies.dccReply` | object | 1 | canonical-json-payloads-optional-other.md |  |
| `payment.serviceReplies.dccReply.accepted` | boolean | 1 | canonical-json-payloads-optional-other.md | True |
| `payment.serviceReplies.dccReply.exchangeRate` | number | 1 | canonical-json-payloads-optional-other.md | 0.925 |
| `payment.serviceReplies.dccReply.exchangeRateSource` | string | 1 | canonical-json-payloads-optional-other.md | <exchangeRateSource> |
| `payment.serviceReplies.dccReply.foreignAmount` | number | 1 | canonical-json-payloads-optional-other.md | 9.25 |
| `payment.serviceReplies.dccReply.foreignCurrency` | string | 1 | canonical-json-payloads-optional-other.md | EUR |
| `payment.serviceReplies.dccReply.reasonCode` | string | 1 | canonical-json-payloads-optional-other.md | 100 |
| `payment.serviceReplies.fraudCheckReply` | object | 1 | canonical-json-payloads-optional-other.md |  |
| `payment.serviceReplies.fraudCheckReply.decision` | string | 1 | canonical-json-payloads-optional-other.md | ACCEPT |
| `payment.serviceReplies.fraudCheckReply.processorResponseCode` | string | 1 | canonical-json-payloads-optional-other.md | A |
| `payment.serviceReplies.fraudCheckReply.reasonCode` | string | 1 | canonical-json-payloads-optional-other.md | 100 |
| `payment.serviceReplies.tokenCreateReply` | object | 4 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md |  |
| `payment.serviceReplies.tokenCreateReply.paymentToken` | string | 4 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | <paymentToken> |
| `payment.serviceReplies.tokenCreateReply.reasonCode` | string | 4 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | 100 |
| `payment.serviceReplies.transactionReply` | object | 34 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `payment.serviceReplies.transactionReply.accountBalance2Currency` | string | 1 | canonical-json-payloads-core.md | USD |
| `payment.serviceReplies.transactionReply.amount` | number | 33 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | 6.0; 5.01; 0.0 |
| `payment.serviceReplies.transactionReply.authorizationCode` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <authorizationCode> |
| `payment.serviceReplies.transactionReply.avsCode` | string | 2 | canonical-json-payloads-optional-other.md | M |
| `payment.serviceReplies.transactionReply.cvCode` | string | 2 | canonical-json-payloads-optional-other.md | M |
| `payment.serviceReplies.transactionReply.partialAmount` | boolean | 27 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | False |
| `payment.serviceReplies.transactionReply.processorResponseCode` | string | 34 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | 100 |
| `payment.serviceReplies.transactionReply.processorResponseMessage` | string | 34 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | APPROVED |
| `payment.serviceReplies.transactionReply.processorTransactionId` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <processorTransactionId> |
| `payment.serviceReplies.transactionReply.purchasingLevel3Enabled` | boolean | 2 | canonical-json-payloads-core.md | False |
| `payment.serviceReplies.transactionReply.reasonCode` | string | 34 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | 100 |
| `payment.serviceReplies.transactionReply.reconciliationId` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <reconciliationId> |
| `payment.serviceReplies.transactionReply.requestDateTime` | string | 34 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <requestDateTime> |
| `payment.services` | object | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `payment.services.dccService` | object | 1 | canonical-json-payloads-optional-other.md |  |
| `payment.services.dccService.run` | boolean | 1 | canonical-json-payloads-optional-other.md | True |
| `payment.services.fraudCheckService` | object | 1 | canonical-json-payloads-optional-other.md |  |
| `payment.services.fraudCheckService.run` | boolean | 1 | canonical-json-payloads-optional-other.md | True |
| `payment.services.tokenCreateService` | object | 4 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md |  |
| `payment.services.tokenCreateService.run` | boolean | 4 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | True |
| `payment.services.transactionService` | object | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `payment.services.transactionService.commerceIndicator` | string | 3 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md | internet |
| `payment.services.transactionService.transactionType` | string | 31 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | capture; authorization; void |
| `payment.tenders` | object | 23 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `payment.tenders.alternativePayment` | object | 3 | canonical-json-payloads-lifecycle.md, canonical-json-payloads-wallets.md |  |
| `payment.tenders.alternativePayment.paymentMethod` | string | 3 | canonical-json-payloads-lifecycle.md, canonical-json-payloads-wallets.md | paypal; athmovil; <paymentMethod> |
| `payment.tenders.card` | object | 20 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `payment.tenders.card.accountNumber` | string | 7 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | <dpan>; <paymentToken>; <mpan> |
| `payment.tenders.card.cardType` | string | 7 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | token |
| `payment.tenders.card.encryptedData` | string | 13 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <applePayPaymentToken>; <googlePayPaymentToken>; <encryptedCardData> |
| `payment.tenders.pos` | object | 13 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `payment.tenders.pos.encMode` | string | 13 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | googlewallet; rsa; applewallet |
| `payment.tenders.pos.entryMode` | string | 13 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | keyed; inapp |
| `payment.tenders.pos.msrType` | string | 13 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | applepay; none; googlepay |
| `payment.tenders.pos.trackKsn` | string | 11 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | <trackKsn> |
| `payment.tokenInformation` | object | 11 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md |  |
| `payment.tokenInformation.accountNumberMasked` | string | 11 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | 411111xxxxxx1111 |
| `payment.tokenInformation.brand` | string | 11 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | VS |
| `payment.tokenInformation.cardExpirationMonth` | integer | 11 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | 11 |
| `payment.tokenInformation.cardExpirationYear` | integer | 11 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | 29 |
| `payment.tokenInformation.cardType` | string | 11 | canonical-json-payloads-core.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md | credit |
| `workflow.contextId` | string | 57 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <contextId> |
| `workflow.decision` | string | 52 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | REJECT; INTERRUPT; ERROR |
| `workflow.nonAcceptedServices` | array | 41 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `workflow.nonAcceptedServices[]` | array | 41 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `workflow.posSyncId` | string | 33 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <posSyncId> |
| `workflow.reasonCode` | object | 52 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md |  |
| `workflow.reasonCode.code` | string | 52 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | <reasonCode>; 900 |
| `workflow.reasonCode.description` | string | 52 | canonical-json-payloads-3ds.md, canonical-json-payloads-core.md, canonical-json-payloads-lifecycle.md, canonical-json-payloads-optional-other.md, canonical-json-payloads-tokenization-variants.md, canonical-json-payloads-wallets.md | Additional customer action required; Challenge cancelled by customer; Additional customer authentication required |

## OAS-Only Fields Requiring Current-State Validation

These fields appear in one or both legacy OAS files but were not observed in the canonical payload corpus.

| Field Path | Type(s) | Source | Enum / Notes |
|---|---|---|---|
| `b64Endianness` | string | reference-oas-a.yaml | enum: be, le |
| `b64Signedness` | string | reference-oas-a.yaml | enum: signed, unsigned |
| `certificate` | string | reference-oas-a.yaml | x509 Certificate containing the public key if the format specified is x509 |
| `consumer.avsMatchThreshold` | string | reference-oas-a.yaml | This field is deprecated. Use 'consumer.profile.billingInformation.avsMatchThreshold' instead. |
| `consumer.createCardOnFileIfNotFound` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'consumer.profile.account.save' instead. |
| `consumer.createConsumer` | boolean | reference-oas-a.yaml | If a Consumer Profile is not found, automatically create a profile. Defaults to false. |
| `consumer.createConsumerProfileIfNotFound` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'consumer.createConsumer' instead. |
| `consumer.merchantConsumerId` | string | reference-oas-a.yaml | The merchant-facing value used to uniquely identify a Consumer |
| `consumer.profile` | object | reference-oas-a.yaml |  |
| `consumer.profile.account` | object | reference-oas-a.yaml |  |
| `consumer.profile.account.return` | boolean | reference-oas-a.yaml | Returns the Token from the specified Consumer Profile that matches the CardIssuer passed in. |
| `consumer.profile.account.save` | boolean | reference-oas-a.yaml | If the specified Consumer Profile is found, but an associated card on file is not, the specified token will automatically be added as a card on file. |
| `consumer.profile.account.updateExpirationDate` | boolean | reference-oas-a.yaml | If true, External will use the card expiration dates passed in the request instead of the exp dates stored in CardStor but only if it is newer than CardStor. API will then update the CardStor with these dates. |
| `consumer.profile.account.use` | boolean | reference-oas-a.yaml | Use the Token found on the Consumer's Profile that matches the CardIssuer passed into API |
| `consumer.profile.accounts` | array | reference-oas-a.yaml |  |
| `consumer.profile.accounts[].account` | string | reference-oas-a.yaml | The Token from the Consumer's Profile which matches the CardIssuer from the request. |
| `consumer.profile.accounts[].type` | string | reference-oas-a.yaml | The account type for this account. Will match card issuer that was passed in. |
| `consumer.profile.billingInformation` | object | reference-oas-a.yaml |  |
| `consumer.profile.billingInformation.avsMatchThreshold` | string | reference-oas-a.yaml | enum: Strict, Partial, None |
| `consumer.profile.billingInformation.stopNotFound` | boolean | reference-oas-a.yaml | If true and a Consumer Profile Billing Address is not found, stop processing and return error, else continue processing with the merchant-provided information or null if it does not exist. |
| `consumer.profile.billingInformation.update` | boolean | reference-oas-a.yaml | Update the Billing Address information in the Consumer Data Service. Requires MerchantConsumerId to be passed. |
| `consumer.profile.billingInformation.use` | boolean | reference-oas-a.yaml | Use the Billing Address information from the Consumer Data Service if one is found. Requires MerchantConsumerId, Payment Token, and CVV to be passed. |
| `consumer.refreshBillingAddressOnConsumerProfile` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'consumer.profile.billingInformation.update' instead. |
| `consumer.returnCardOnFileToken` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'consumer.profile.account.return' instead. |
| `consumer.stopOnConsumerProfileBillingAddressNotFound` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'consumer.profile.billingInformation.stopNotFound' instead. |
| `consumer.stopOnRefreshBillingAddressOnConsumerProfileFailure` | boolean | reference-oas-a.yaml | This field is deprecated and has no effect. Values provided will be ignored. |
| `consumer.updateCardExpirationDate` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'consumer.profile.card.updateExpirationDate' instead. |
| `consumer.useConsumerProfileBillingAddress` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'consumer.profile.billingInformation.use' instead. |
| `consumer.useConsumerProfileTokenForPayment` | boolean | reference-oas-a.yaml | This field is deprecated. Use 'consumer.profile.account.use' instead. |
| `consumerRequestAccountOptions.return` | boolean | reference-oas-a.yaml | Returns the Token from the specified Consumer Profile that matches the CardIssuer passed in. |
| `consumerRequestAccountOptions.save` | boolean | reference-oas-a.yaml | If the specified Consumer Profile is found, but an associated card on file is not, the specified token will automatically be added as a card on file. |
| `consumerRequestAccountOptions.updateExpirationDate` | boolean | reference-oas-a.yaml | If true, External will use the card expiration dates passed in the request instead of the exp dates stored in CardStor but only if it is newer than CardStor. API will then update the CardStor with these dates. |
| `consumerRequestAccountOptions.use` | boolean | reference-oas-a.yaml | Use the Token found on the Consumer's Profile that matches the CardIssuer passed into API |
| `consumerRequestBillingInformationOptions.avsMatchThreshold` | string | reference-oas-a.yaml | enum: Strict, Partial, None |
| `consumerRequestBillingInformationOptions.stopNotFound` | boolean | reference-oas-a.yaml | If true and a Consumer Profile Billing Address is not found, stop processing and return error, else continue processing with the merchant-provided information or null if it does not exist. |
| `consumerRequestBillingInformationOptions.update` | boolean | reference-oas-a.yaml | Update the Billing Address information in the Consumer Data Service. Requires MerchantConsumerId to be passed. |
| `consumerRequestBillingInformationOptions.use` | boolean | reference-oas-a.yaml | Use the Billing Address information from the Consumer Data Service if one is found. Requires MerchantConsumerId, Payment Token, and CVV to be passed. |
| `consumerRequestProfileOptions.account` | object | reference-oas-a.yaml |  |
| `consumerRequestProfileOptions.account.return` | boolean | reference-oas-a.yaml | Returns the Token from the specified Consumer Profile that matches the CardIssuer passed in. |
| `consumerRequestProfileOptions.account.save` | boolean | reference-oas-a.yaml | If the specified Consumer Profile is found, but an associated card on file is not, the specified token will automatically be added as a card on file. |
| `consumerRequestProfileOptions.account.updateExpirationDate` | boolean | reference-oas-a.yaml | If true, External will use the card expiration dates passed in the request instead of the exp dates stored in CardStor but only if it is newer than CardStor. API will then update the CardStor with these dates. |
| `consumerRequestProfileOptions.account.use` | boolean | reference-oas-a.yaml | Use the Token found on the Consumer's Profile that matches the CardIssuer passed into API |
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
| `credentials.clientId` | string | reference-oas-a.yaml | The Vendor-assigned value that uniquely identifies the merchant organization. |
| `credentials.merchantPropertyCode` | string | reference-oas-a.yaml | The merchant value that represents a StoreId and TerminalId on the Vendor side. Not required if passing StoreId and TerminalId. If merchantPropertyCode is provided, those values will be resolved automatically. |
| `data` | object | reference-oas-a.yaml |  |
| `device` | object | reference-oas-a.yaml |  |
| `device.applicationName` | string | reference-oas-a.yaml |  |
| `device.id` | string | reference-oas-a.yaml |  |
| `device.proceedWithAuth` | boolean | reference-oas-a.yaml |  |
| `device.registrationId` | string | reference-oas-a.yaml |  |
| `device.requests` | array | reference-oas-a.yaml |  |
| `device.responses` | array | reference-oas-a.yaml |  |
| `device.type` | string | reference-oas-a.yaml |  |
| `device.version` | string | reference-oas-a.yaml |  |
| `eCommerce.apmOptions` | object | reference-oas-b.yaml |  |
| `eCommerce.apmOptions.qrCodeHeight` | integer | reference-oas-b.yaml | Height of the QRCode image |
| `eCommerce.apmOptions.qrCodeWidth` | integer | reference-oas-b.yaml | Width of the QRCode image |
| `eCommerce.apmOptions.returnQrCodeImage` | boolean | reference-oas-b.yaml | Apm Url is always returned when applicable. If a scannable QRCode is desired, pass true for this. |
| `eCommerce.apmOptions.thirdPartyIdentifier` | string | reference-oas-b.yaml | A unique identifier required by some third parties to uniquely identify a payment session |
| `eCommerce.apmQrCode` | string | reference-oas-b.yaml | Base64 representation of the QrCode in png format |
| `eCommerce.apmUrl` | string | reference-oas-b.yaml | Url to the Apm payment system |
| `eCommerce.cardIssuer` | string | reference-oas-b.yaml | enum: alipay, athmovilecom, athmovilinstore, citidloc, citimil, cnh, klarna, openbanking, paypal, venmo |
| `eCommerce.reasonCode` | object | reference-oas-b.yaml | Provides additional information for the eCommerce portion of the operation. Values in the range of 900-999. |
| `eCommerce.reasonCode.code` | integer | reference-oas-b.yaml | Numerical representation of the ReasonCode |
| `eCommerce.reasonCode.description` | string | reference-oas-b.yaml | Textual representation of the ReasonCode |
| `eCommerce.serviceProviderCodes` | array | reference-oas-b.yaml |  |
| `eCommerce.serviceProviderCodes[].serviceProviderCode` | string | reference-oas-b.yaml | Code returned by the third party |
| `eCommerce.serviceProviderCodes[].serviceProviderMessage` | string | reference-oas-b.yaml | The message provided by the third party |
| `eCommerce.serviceProviderCodes[].serviceProviderName` | string | reference-oas-b.yaml | The Third Party Provider |
| `eCommerce.threeDSOptions` | object | reference-oas-b.yaml |  |
| `eCommerce.threeDSOptions.account` | object | reference-oas-b.yaml |  |
| `eCommerce.threeDSOptions.account.ageIndicator` | string | reference-oas-b.yaml | Length of time that the cardholder had an account with the 3DS requestor. Options 01, 02, 03, 04 and 05. |
| `eCommerce.threeDSOptions.account.changeIndicator` | string | reference-oas-b.yaml | Length of time since the cardholder's account information with the 3DS Requestor was last changed, including Billing or Shipping address, new payment account, or new user added. Options 01, 02, 03 and 04. |
| `eCommerce.threeDSOptions.account.changedDate` | string | reference-oas-b.yaml | The last changed date of the cardholder's account with the 3DS Requestor, including Billing or Shipping address, new payment account, or new user added. Format YYYYMMDD. |
| `eCommerce.threeDSOptions.account.dateOpened` | string | reference-oas-b.yaml | Date that the cardholder opened the account with the 3DS Requestor. Format YYYYMMDD |
| `eCommerce.threeDSOptions.account.identifier` | string | reference-oas-b.yaml | Additional information about the account that is optionally provided by the 3DS Requestor. |
| `eCommerce.threeDSOptions.account.passwordChangeIndicator` | string | reference-oas-b.yaml | Length of time since the cardholder's account with the 3DS Requestor had a password change or account reset. Options 01,02,03, 04 and 05. |
| `eCommerce.threeDSOptions.account.passwordChangedDate` | string | reference-oas-b.yaml | Date that cardholder's account with the 3DS Requestor had a password change or account reset. Format YYYYMMDD. |
| `eCommerce.threeDSOptions.account.paymentAccountAgeDate` | string | reference-oas-b.yaml | Date on which the payment account was enrolled in the cardholder's account with the 3DS Requestor. Format YYYYMMDD. |
| `eCommerce.threeDSOptions.account.paymentAccountIndicator` | string | reference-oas-b.yaml | Indicates the length of time that the payment account was enrolled in the cardholder's account with the 3DS Requestor. Options 01, 02, 03, 04 and 05. |
| `eCommerce.threeDSOptions.account.provisionAttemptsDay` | string | reference-oas-b.yaml | Number of Add Card attempts in the last 24 hours. |
| `eCommerce.threeDSOptions.account.purchasesInLastSixMonths` | string | reference-oas-b.yaml | Number of purchases with the cardholder's account during the previous six months. |
| `eCommerce.threeDSOptions.account.shippingAddressUsageDate` | string | reference-oas-b.yaml | Indicates the date when the shipping address was first used with the 3DS Requestor for the transaction. Format YYYYMMDD. |
| `eCommerce.threeDSOptions.account.shippingAddressUsageIndicator` | string | reference-oas-b.yaml | Indicates when the shipping address used for this transaction was first used with the 3DS Requestor. Options 01, 02, 03 and 04. |
| `eCommerce.threeDSOptions.account.shippingNameIndicator` | string | reference-oas-b.yaml | Indicates if the cardholder's name on the account is identical to the shipping name used for this transaction. Options 01=Same, 02=Different. |
| `eCommerce.threeDSOptions.account.suspiciousAccountActivityIndicator` | string | reference-oas-b.yaml | Suspicious activity (including previous fraud occurrence) indicator on the cardholder account. Options 01=No Suspicious Activity, 02=Suspicious Activity. |
| `eCommerce.threeDSOptions.account.txnActivityDay` | string | reference-oas-b.yaml | Number of transactions for the cardholder's account with the 3DS Requestor across all payment accounts in the previous 24 hours. |
| `eCommerce.threeDSOptions.account.txnActivityYear` | string | reference-oas-b.yaml | Number of transactions for the cardholder's account with the 3DS Requestor across all payment accounts in the previous year. |
| `eCommerce.threeDSOptions.account.type` | string | reference-oas-b.yaml | Indicates the type of account. For example, for a multi-account card product. |
| `eCommerce.threeDSOptions.addressesMustMatch` | boolean | reference-oas-b.yaml | Indicates that the Bill-To and Ship-To addresses are identical |
| `eCommerce.threeDSOptions.authenticationIndicator` | string | reference-oas-b.yaml | Indicates the type of Authentication request. This data element provides additional information to the ACS to determine the best approach for handling an authentication request. |
| `eCommerce.threeDSOptions.challengeIndicator` | string | reference-oas-b.yaml | Indicates whether a challenge is requested for this transaction. For example, for 01-PA, a 3DS Requestor may have concerns about the transaction, and request a challenge. For 02-NPA, a challenge may be necessary when adding a new card to a … |
| `eCommerce.threeDSOptions.challengeWindowSize` | string | reference-oas-b.yaml | Required if generateCReq is present |
| `eCommerce.threeDSOptions.clientType` | string | reference-oas-b.yaml | enum: browserJs, mobileSdk |
| `eCommerce.threeDSOptions.merchantDomainName` | string | reference-oas-b.yaml | The domain name of the merchant website, this will help to handle the CORS issue in three DS flow |
| `eCommerce.threeDSOptions.mobileSdkRenderOptions` | object | reference-oas-b.yaml |  |
| `eCommerce.threeDSOptions.mobileSdkRenderOptions.interface` | string | reference-oas-b.yaml | Lists all the SDK Interface types that the device supports for displaying specific challenge user interfaces within the SDK. Values accepted are 01=Native, 02=HTML, 03=Both |
| `eCommerce.threeDSOptions.mobileSdkRenderOptions.uiType` | string | reference-oas-b.yaml | Lists all the UI types that the device supports for displaying specific challenge user interfaces within the SDK. Values accepted are Native UI=01-04; HTML UI=01-05. |
| `eCommerce.threeDSOptions.purchaseInstallData` | integer | reference-oas-b.yaml | Indicates the maximum number of authorizations permitted for installment payments. |
| `eCommerce.threeDSOptions.recurrence` | object | reference-oas-b.yaml |  |
| `eCommerce.threeDSOptions.recurrence.expirationDate` | string | reference-oas-b.yaml | Date after which no further authorizations will be performed. Format YYYYMMDD. |
| `eCommerce.threeDSOptions.recurrence.frequency` | integer | reference-oas-b.yaml | Indicates the minimum number of days between authorizations. |
| `eCommerce.threeDSOptions.resultDrivenProcessing` | object | reference-oas-b.yaml | This will instruct the API what actions needed to be taken once the 3ds outcome has received |
| `eCommerce.threeDSOptions.resultDrivenProcessing.enabled` | boolean | reference-oas-b.yaml | Default true, If false, this will instruct the system that 3ds outcome will have no impact on the transaction processing |
| `eCommerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus` | object | reference-oas-b.yaml | This will instruct how to process specific 3ds outcome |
| `eCommerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSAuthenticationAttempted` | boolean | reference-oas-b.yaml | ??? |
| `eCommerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSAuthenticationFailed` | boolean | reference-oas-b.yaml | Default false, if true, the payment processing will continue even when the 3ds authentication faild. |
| `eCommerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSAuthenticationSuccessful` | boolean | reference-oas-b.yaml | Default true, If true, this will process payment if 3ds authentication was successful |
| `eCommerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSCardNotSupported` | boolean | reference-oas-b.yaml | Default false, if true, the payment processing will continue even when the card doesn't support 3ds. |
| `eCommerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSRejected` | boolean | reference-oas-b.yaml | Default false, if true, the payment processing will continue even when the 3ds is rejected. |
| `eCommerce.threeDSOptions.resultDrivenProcessing.resumeProcessingOnStatus.threeDSUnavailable` | boolean | reference-oas-b.yaml | Default false, if true, the payment processing will continue even when the 3ds authentication was unavailable. |
| `eCommerce.threeDSOptions.runThreeDsAuthentication` | boolean | reference-oas-b.yaml | Default true, this indicates if the consumer wants to run the 3DS authentication or not. This will override the store config value |
| `eCommerce.threeDSOptions.transactionType` | string | reference-oas-b.yaml | Identifies the type of transaction being authenticated. This field is required for VISA transactions and accepted values are 01, 03, 10, 11, and 28. |
| `eCommerce.threeDSProviderResponse` | object | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.acs` | object | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.acs.challengeMandated` | boolean | reference-oas-b.yaml | Indication of whether a challenge is required for the transaction to be authorised due to local or regional mandates or other variable. |
| `eCommerce.threeDSProviderResponse.acs.operatorIdentifer` | string | reference-oas-b.yaml | DS-assigned ACS identifier. Each DS can provide a unique ID to each ACS. |
| `eCommerce.threeDSProviderResponse.acs.referenceNumber` | string | reference-oas-b.yaml | Unique identifier assigned by the EMVCo Secretariat upon testing and approval. |
| `eCommerce.threeDSProviderResponse.acs.renderingType` | object | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.acs.renderingType.acsInterface` | string | reference-oas-b.yaml | Identifies the ACS interface that presents the challenge to the cardholder. Options are 01, 02. |
| `eCommerce.threeDSProviderResponse.acs.renderingType.acsUiTemplate` | string | reference-oas-b.yaml | Identifies the UI Template format that the ACS presents to the consumer first. Options are 01, 02, 03, 04, 05. |
| `eCommerce.threeDSProviderResponse.acs.transactionId` | string | reference-oas-b.yaml | Universally Unique transaction identifier assigned by the ACS to identify a single transaction. Format uuid. |
| `eCommerce.threeDSProviderResponse.acs.url` | string | reference-oas-b.yaml | Fully-qualified URL of the ACS to be used for the challenge. |
| `eCommerce.threeDSProviderResponse.authenticationValue` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.cardHolderInformation` | object | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.challengeCancelledIndicator` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.dsTransactionId` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.eci` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.error` | object | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.error.code` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.error.component` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.error.description` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.interactionCounter` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.message` | object | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.message.version` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.sdk` | object | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.sdk.transactionId` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.threeDsResult` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.threeDsServerTransactionId` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.transaction` | object | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.transaction.status` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.transaction.statusReason` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.trustList` | object | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.trustList.status` | string | reference-oas-b.yaml |  |
| `eCommerce.threeDSProviderResponse.trustList.statusSource` | string | reference-oas-b.yaml |  |
| `enterpriseCode` | string | reference-oas-a.yaml | The unique Vendor-supplied value that represents the enterprise |
| `expSubformat` | string | reference-oas-a.yaml | enum: b64, hex, dec |
| `expirationDatetime` | string | reference-oas-a.yaml | Not-valid-after datetime for the key |
| `exponent` | string | reference-oas-a.yaml | Exponent e for RSA Encryption |
| `format` | string | reference-oas-a.yaml | enum: x509, spki, rpk, modexp |
| `instore` | object | reference-oas-a.yaml |  |
| `keySlotId` | string | reference-oas-a.yaml | The Key Slot Id set created within Vendor's Enterprise Portal. |
| `merchantPropertyCode` | string | reference-oas-a.yaml | Merchant property code, required if storeID not provided |
| `modSubformat` | string | reference-oas-a.yaml | enum: b64, hex, dec |
| `modulus` | string | reference-oas-a.yaml | Modulus n for RSA Encryption |
| `payment.autoRefundOnVoidFailure` | boolean | reference-oas-a.yaml | Denotes an optional fallback behavior. Perform a Refund when a Void request fails due to batch timing. |
| `publicKey` | string | reference-oas-a.yaml | The RSA public key in the specified format |
| `storeId` | string | reference-oas-a.yaml | The unique Vendor-supplied value that represents a Store, required only if merchantPropertyCode is not provided |
| `subformat` | string | reference-oas-a.yaml | enum: b64der, pem |
| `threeDSProgressRequest.browserCharacteristics` | object | reference-oas-b.yaml |  |
| `threeDSProgressRequest.browserCharacteristics.acceptHeader` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.browserCharacteristics.colorDepth` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.browserCharacteristics.ipAddress` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.browserCharacteristics.javaEnabled` | boolean | reference-oas-b.yaml |  |
| `threeDSProgressRequest.browserCharacteristics.javascriptEnabled` | boolean | reference-oas-b.yaml |  |
| `threeDSProgressRequest.browserCharacteristics.language` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.browserCharacteristics.screenHeight` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.browserCharacteristics.screenWidth` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.browserCharacteristics.timeZoneOffset` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.browserCharacteristics.userAgent` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.clientErrors` | array | reference-oas-b.yaml |  |
| `threeDSProgressRequest.clientErrors[].message` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.contextId` | string | reference-oas-b.yaml | Uniquely identifies a unit of work. Provide when performing an asynchronous workflow that spans multiple calls. |
| `threeDSProgressRequest.mobileSdkData` | object | reference-oas-b.yaml |  |
| `threeDSProgressRequest.mobileSdkData.applicationId` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.mobileSdkData.encData` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.mobileSdkData.maxTimeout` | integer | reference-oas-b.yaml | Time in minutes for the entire 3DS workflow including Auth/Challenge/etc. Minimum of 5 minutes, maximum of 60. |
| `threeDSProgressRequest.mobileSdkData.publicKey` | object | reference-oas-b.yaml |  |
| `threeDSProgressRequest.mobileSdkData.publicKey.crv` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.mobileSdkData.publicKey.keyType` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.mobileSdkData.publicKey.x` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.mobileSdkData.publicKey.y` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.mobileSdkData.referenceNumber` | string | reference-oas-b.yaml | Identifies the vendor and version for the 3DS SDK that is integrated in a 3DS Requestor App, assigned by EMVCo when the 3DS SDK is approved. |
| `threeDSProgressRequest.mobileSdkData.transactionId` | string | reference-oas-b.yaml | Universally unique transaction identifier assigned by the 3DS SDK to identify a single transaction. |
| `threeDSProgressRequest.trackKsn` | string | reference-oas-b.yaml |  |
| `threeDSProgressRequest.tracke` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.challengeAcsUrl` | string | reference-oas-b.yaml | The Url for the customer challenge if 3dsOptions.clientType is 'browserJs' |
| `threeDSProgressResponse.clientPollInterval` | integer | reference-oas-b.yaml | How many seconds between poll attempts |
| `threeDSProgressResponse.clientPollWindow` | integer | reference-oas-b.yaml | The maximum seconds to poll |
| `threeDSProgressResponse.merchantNotifiedOfTransactionResult` | boolean | reference-oas-b.yaml | Denotes that Vendor API invoked the Merchant callback endpoint |
| `threeDSProgressResponse.methodUrl` | string | reference-oas-b.yaml | The Url for the device data collection if the 3dsOptions.ClientType is 'browserJs' |
| `threeDSProgressResponse.mobileSdkData` | object | reference-oas-b.yaml |  |
| `threeDSProgressResponse.mobileSdkData.dsCaCertificate` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey` | object | reference-oas-b.yaml |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey.e` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey.kty` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKey.n` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.mobileSdkData.dsPublicKeyId` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.status` | string | reference-oas-b.yaml | enum: CardDataMissing, NotApplicable, DeviceDataRequired, ChallengeRequired, ChallengeNotNeeded, ErrorAt3dsProvider, ChallengeSuccessful, ChallengeFailed, ChallengeCancelledByCustomer |
| `threeDSProgressResponse.threeDSEncodedChallengeRequestData` | string | reference-oas-b.yaml | Base64 URL encoded CReq message, only returned for challenge transactions during browser flow |
| `threeDSProgressResponse.threeDSProvider` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse` | object | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.acs` | object | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.acs.challengeMandated` | boolean | reference-oas-b.yaml | Indication of whether a challenge is required for the transaction to be authorised due to local or regional mandates or other variable. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.operatorIdentifer` | string | reference-oas-b.yaml | DS-assigned ACS identifier. Each DS can provide a unique ID to each ACS. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.referenceNumber` | string | reference-oas-b.yaml | Unique identifier assigned by the EMVCo Secretariat upon testing and approval. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.renderingType` | object | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.acs.renderingType.acsInterface` | string | reference-oas-b.yaml | Identifies the ACS interface that presents the challenge to the cardholder. Options are 01, 02. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.renderingType.acsUiTemplate` | string | reference-oas-b.yaml | Identifies the UI Template format that the ACS presents to the consumer first. Options are 01, 02, 03, 04, 05. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.transactionId` | string | reference-oas-b.yaml | Universally Unique transaction identifier assigned by the ACS to identify a single transaction. Format uuid. |
| `threeDSProgressResponse.threeDSProviderResponse.acs.url` | string | reference-oas-b.yaml | Fully-qualified URL of the ACS to be used for the challenge. |
| `threeDSProgressResponse.threeDSProviderResponse.authenticationValue` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.cardHolderInformation` | object | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.challengeCancelledIndicator` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.dsTransactionId` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.eci` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.error` | object | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.error.code` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.error.component` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.error.description` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.interactionCounter` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.message` | object | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.message.version` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.sdk` | object | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.sdk.transactionId` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.threeDsResult` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.threeDsServerTransactionId` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.transaction` | object | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.transaction.status` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.transaction.statusReason` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.trustList` | object | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.trustList.status` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderResponse.trustList.statusSource` | string | reference-oas-b.yaml |  |
| `threeDSProgressResponse.threeDSProviderTransactionId` | string | reference-oas-b.yaml | The provider specific transactionId that is the unique identifer for the 3ds session. |
| `threeDSProgressResponse.transactionComplete` | boolean | reference-oas-b.yaml | The transaction has been processed and is complete. Full information available on the introspection endpoint |
| `threeDSProviderResponse.acs` | object | reference-oas-b.yaml |  |
| `threeDSProviderResponse.acs.challengeMandated` | boolean | reference-oas-b.yaml | Indication of whether a challenge is required for the transaction to be authorised due to local or regional mandates or other variable. |
| `threeDSProviderResponse.acs.operatorIdentifer` | string | reference-oas-b.yaml | DS-assigned ACS identifier. Each DS can provide a unique ID to each ACS. |
| `threeDSProviderResponse.acs.referenceNumber` | string | reference-oas-b.yaml | Unique identifier assigned by the EMVCo Secretariat upon testing and approval. |
| `threeDSProviderResponse.acs.renderingType` | object | reference-oas-b.yaml |  |
| `threeDSProviderResponse.acs.renderingType.acsInterface` | string | reference-oas-b.yaml | Identifies the ACS interface that presents the challenge to the cardholder. Options are 01, 02. |
| `threeDSProviderResponse.acs.renderingType.acsUiTemplate` | string | reference-oas-b.yaml | Identifies the UI Template format that the ACS presents to the consumer first. Options are 01, 02, 03, 04, 05. |
| `threeDSProviderResponse.acs.transactionId` | string | reference-oas-b.yaml | Universally Unique transaction identifier assigned by the ACS to identify a single transaction. Format uuid. |
| `threeDSProviderResponse.acs.url` | string | reference-oas-b.yaml | Fully-qualified URL of the ACS to be used for the challenge. |
| `threeDSProviderResponse.authenticationValue` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.cardHolderInformation` | object | reference-oas-b.yaml |  |
| `threeDSProviderResponse.challengeCancelledIndicator` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.dsTransactionId` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.eci` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.error` | object | reference-oas-b.yaml |  |
| `threeDSProviderResponse.error.code` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.error.component` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.error.description` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.interactionCounter` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.message` | object | reference-oas-b.yaml |  |
| `threeDSProviderResponse.message.version` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.sdk` | object | reference-oas-b.yaml |  |
| `threeDSProviderResponse.sdk.transactionId` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.threeDsResult` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.threeDsServerTransactionId` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.transaction` | object | reference-oas-b.yaml |  |
| `threeDSProviderResponse.transaction.status` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.transaction.statusReason` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.trustList` | object | reference-oas-b.yaml |  |
| `threeDSProviderResponse.trustList.status` | string | reference-oas-b.yaml |  |
| `threeDSProviderResponse.trustList.statusSource` | string | reference-oas-b.yaml |  |
| `trackKsn` | string | reference-oas-a.yaml | Value to be passed in the trackKsn field in the Payment Request to identify the correct key |

## Recommended Next Steps

1. Review JSON-only fields first, because they likely represent current documentation behavior missing from the legacy specs.
2. Review OAS-only fields second, because they may represent stale, deprecated, externally referenced, or still-valid-but-unrepresented capabilities.
3. Create a compact `hpx-oas-reference.md` for the `review-hpx-example` skill using high- and medium-confidence fields only.
4. Use this inventory as the input to a reconstructed documentation OAS, preserving source and confidence metadata.