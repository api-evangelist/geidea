# Geidea (geidea)

Geidea is a Saudi-headquartered fintech and payments platform serving merchants across the MENA region - Saudi Arabia, Egypt, and the UAE. Its Payment Gateway lets merchants accept card and wallet payments through a hosted **Geidea Checkout** (HPP) page or a server-to-server **Direct API** for PCI-DSS-compliant merchants, covering the full transaction lifecycle: create session, 3-D Secure authentication, pay, capture, void, refund, cancel, tokenization, Pay by Link, and Pay by Invoice - with support for mada, Visa, Mastercard, Apple Pay, Google Pay, Meeza QR, and BNPL.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/geidea/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/geidea/refs/heads/main/apis.yml)

## Access model (read this first)

Geidea's API is **not self-serve**. You do not sign up for an API key on a developer page and start calling. Access works like a payments acquirer:

1. **Merchant onboarding.** You apply and are underwritten as a merchant. On approval, Geidea (via the enablement team or the merchant portal under *Payment Gateway -> Gateway Settings*) issues your gateway credentials: a **Merchant Public Key** and an **API Password**.
2. **Authentication = HTTP Basic + HMAC signature.** Requests use HTTP Basic auth with the **Public Key as the username** and the **API Password as the password**. In addition, sensitive requests carry an **HMAC-SHA256 `signature`** in the JSON body, Base64-encoded and keyed by your API Password (for Create Session it is computed over the public key, amount, currency, merchant reference id, and timestamp; for merchant-initiated transactions over the public key and session id).
3. **Server-to-server only.** The API Password is a secret. It must **never** be exposed in a front-end - all Direct API calls must be made from your backend.
4. **PCI-DSS.** The hosted **Geidea Checkout** page is for merchants who are not PCI-DSS compliant. The **Direct API** (where you handle raw card data) requires the merchant to be PCI-DSS compliant.

**Region-specific base hosts:**

| Region | Base host |
| --- | --- |
| Saudi Arabia (KSA) | `https://api.ksamerchant.geidea.net` |
| Egypt | `https://api.merchant.geidea.net` |
| UAE | `https://api.geidea.ae` |

Pricing is a negotiated **per-transaction Merchant Discount Rate (MDR)** set during onboarding - there is no metered API fee and no public tier ladder.

## Tags

- Payments
- Payment Gateway
- Saudi Arabia
- Egypt
- MENA
- mada
- Cards
- POS
- Checkout
- Fintech

## Timestamps

- **Created:** 2026-07-12
- **Modified:** 2026-07-12

## APIs

### Geidea Checkout API

Create a payment session with `POST /payment-intent/api/v2/direct/session` and render the pre-built, hosted Geidea Checkout (HPP) page. The session is signed with an HMAC signature so merchants that are not PCI-DSS compliant can collect card and wallet payments without handling raw card data.

- **Human URL:** [https://docs.geidea.net/docs/geidea-checkout-v2](https://docs.geidea.net/docs/geidea-checkout-v2)
- **Base URL:** `https://api.merchant.geidea.net`

#### Properties

- [Documentation](https://docs.geidea.net/docs/geidea-checkout-v2)
- [API Reference](https://docs.geidea.net/reference/create-session-v2-1)
- [OpenAPI](openapi/geidea-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/geidea.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/geidea.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Geidea Direct API

Server-to-server card acceptance for PCI-DSS-compliant merchants: Initiate Authentication (`POST /pgw/api/v6/direct/authenticate/initiate`), Authenticate Payer (`POST /pgw/api/v6/direct/authenticate/payer`) for 3-D Secure, then Pay (`POST /pgw/api/v2/direct/pay`).

- **Human URL:** [https://docs.geidea.net/docs/pg-direct-api](https://docs.geidea.net/docs/pg-direct-api)
- **Base URL:** `https://api.merchant.geidea.net`

#### Properties

- [Documentation](https://docs.geidea.net/docs/pg-direct-api)
- [API Reference](https://docs.geidea.net/reference/pay-v-2)
- [OpenAPI](openapi/geidea-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/geidea.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/geidea.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Geidea Tokenization API

Save cards on file and reuse them for recurring and merchant-initiated transactions. Retrieve Token (`GET /pgw/api/v1/direct/token/{tokenId}`) returns a stored instrument token.

- **Human URL:** [https://docs.geidea.net/docs/tokenization](https://docs.geidea.net/docs/tokenization)
- **Base URL:** `https://api.merchant.geidea.net`

#### Properties

- [Documentation](https://docs.geidea.net/docs/tokenization)
- [API Reference](https://docs.geidea.net/reference/retrieve-token-1)
- [OpenAPI](openapi/geidea-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)

### Geidea Transaction Management API

Manage the lifecycle of an authorized transaction: Capture (`POST /pgw/api/v1/direct/capture`), Void (`POST /pgw/api/v3/direct/void`), Refund full or partial (`POST /pgw/api/v2/direct/refund`), Cancel Order (`POST /pgw/api/v1/direct/cancel`), and Fetch Transaction or Order (`GET /pgw/api/v1/direct/order/{orderId}`).

- **Human URL:** [https://docs.geidea.net/docs/overview-1](https://docs.geidea.net/docs/overview-1)
- **Base URL:** `https://api.merchant.geidea.net`

#### Properties

- [Documentation](https://docs.geidea.net/docs/overview-1)
- [API Reference](https://docs.geidea.net/reference/refund-1-1)
- [OpenAPI](openapi/geidea-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/geidea.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/geidea.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Geidea Pay by Link API

Programmatically create, update, fetch, delete, and send (by email or SMS) shareable payment links (Egypt and UAE). Exact request paths are in the Geidea API reference; endpoint schemas are not modeled in this repo's OpenAPI.

- **Human URL:** [https://docs.geidea.net/docs/pay-by-link-apis](https://docs.geidea.net/docs/pay-by-link-apis)
- **Base URL:** `https://api.merchant.geidea.net`

#### Properties

- [Documentation](https://docs.geidea.net/docs/pay-by-link-apis)
- [API Reference](https://docs.geidea.net/reference/create-payment-link)

### Geidea Pay by Invoice API

Create, update, fetch, delete, and send payment invoices (KSA) that customers settle through a Geidea-hosted page. Exact request paths are in the Geidea API reference; endpoint schemas are not modeled in this repo's OpenAPI.

- **Human URL:** [https://docs.geidea.net/docs/pay-by-invoice-apis](https://docs.geidea.net/docs/pay-by-invoice-apis)
- **Base URL:** `https://api.ksamerchant.geidea.net`

#### Properties

- [Documentation](https://docs.geidea.net/docs/pay-by-invoice-apis)
- [API Reference](https://docs.geidea.net/reference/create-payment-invoice)

## Common Properties

- [Domain Security](security/geidea-domain-security.yml)
- [Authentication](authentication/geidea-authentication.yml)
- [GitHub Organization](https://github.com/GeideaSolutions)
- [LinkedIn](https://www.linkedin.com/company/geidea)
- [Website](https://www.geidea.net)
- [Documentation](https://docs.geidea.net)
- [Plans](plans/geidea-plans-pricing.yml)
- [Rate Limits](rate-limits/geidea-rate-limits.yml)
- [Fin Ops](finops/geidea-finops.yml)
- [Blog / News Room](https://geidea.net/merchants/en/about-us/news-room.html)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
