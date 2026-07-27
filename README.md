# Chargefox (chargefox)

Chargefox is Australia's largest public electric-vehicle charging network and, since 2023, a charging software platform rather than a hardware owner. Founded in 2017 and headquartered in Melbourne, it was acquired outright in 2022 by Australian Motoring Services, the joint vehicle of six state motoring clubs — NRMA, RACV, RACQ, RAA, RAC and RACT. It sits downstream of the retailer and the meter: it does not hold a retail electricity licence, it operates the charge points other businesses, councils and governments own. That is precisely why the Consumer Data Right does not reach it — Chargefox is absent from the 84 energy data-holder brands on the ACCC CDR Register, even though Arcline by RACV and RAA Energy, owned by two of its own shareholder clubs, are on it. What Chargefox publishes instead is entirely voluntary: a real, anonymously readable developer documentation site carrying an OpenAPI 3.0.1 contract for a four-endpoint Fleets API, and a rate-limits page that enumerates a full Open Charge Point Interface CPO implementation across OCPI 2.1.1, 2.2 and 2.2.1. Every documented endpoint is closed. The documentation is open; the data is partner-only.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/chargefox/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/chargefox/refs/heads/main/apis.yml)

## Tags

- Energy
- Australia
- EV Charging
- Electricity
- Utilities
- OCPI
- Charge Point Operator
- Roaming
- Fleets
- Mobility
- Charging Sessions
- Electrification

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

### Chargefox Fleets API

Chargefox's documented REST API for fleet customers, described by an OpenAPI 3.0.1 contract titled "Fleets API" version 1.0 that the company renders publicly with Redoc. Four read-only GET operations, all tagged Fleets and all secured by the same `bearerAuth` HTTP bearer scheme: `/api/fleets/v1/usage` lists every charge session completed by a fleet's managed vehicles on the Chargefox network with per-charging-period consumption, tariff, GST and AUD amount; `/api/fleets/v1/sessions` returns the session records; `/api/fleets/v1/vehicles` lists the managed vehicles; `/api/fleets/v1/invoices` returns invoices. Each takes optional `date_from`, `date_to` and `page` query parameters and returns 100 results per page. The contract is real and the documentation is anonymous, but every path returned HTTP 401 to an unauthenticated request on 2026-07-27.

- **Human URL:** [https://app.chargefox.com/developers/docs/fleets](https://app.chargefox.com/developers/docs/fleets)
- **Base URL:** `https://app.chargefox.com/api/fleets/v1`

#### Tags

- EV Charging
- Fleets
- Charging Sessions
- Usage
- Billing
- Australia

#### Properties

- [OpenAPI](openapi/chargefox-fleets-api-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://app.chargefox.com/developers/docs/getting_started)
- [API Reference](https://app.chargefox.com/developers/docs/fleets)
- [Rate Limits](https://app.chargefox.com/developers/docs/rate_limits)
- [Support](https://support.chargefox.com/hc/en-au)

### Chargefox OCPI CPO API

Chargefox's Open Charge Point Interface implementation in the Charge Point Operator role, used for roaming so another network's drivers can authorise, charge and be billed on Chargefox infrastructure. No OCPI specification document is published, but the surface is documented in Chargefox's own rate-limits page, which names the endpoints version by version and module by module — locations under 2, 2.1.1, 2.2 and 2.2.1; sessions under 2.1.1, 2.2 and 2.2.1; CDRs and tariffs under 2.2 and 2.2.1; a PUT tokens endpoint under 2.2 and 2.2.1; and a POST commands endpoint under 2.2. Existence was confirmed by anonymous probe: those routes return HTTP 401 with `WWW-Authenticate: Token realm="Application"` while sibling paths return 404. Access follows a commercial roaming agreement, not a signup.

- **Human URL:** [https://app.chargefox.com/developers/docs/rate_limits](https://app.chargefox.com/developers/docs/rate_limits)
- **Base URL:** `https://app.chargefox.com/ocpi/cpo`

#### Tags

- EV Charging
- OCPI
- Roaming
- Charge Point Operator
- Locations
- Tariffs
- Australia

#### Properties

- [Rate Limits](https://app.chargefox.com/developers/docs/rate_limits)
- [Documentation](https://app.chargefox.com/developers/docs/getting_started)
- [Specification](https://github.com/ocpi/ocpi) — the OCPI standard itself, maintained by the EV Roaming Foundation
- [Legal](https://www.chargefox.com/legal/roaming-partner-terms-and-conditions)
- [Partners](https://www.chargefox.com/partners)

## Common Properties

- [Website](https://www.chargefox.com/)
- [Documentation](https://app.chargefox.com/developers/docs/getting_started)
- [API Reference](https://app.chargefox.com/developers/docs/fleets)
- [Rate Limits](https://app.chargefox.com/developers/docs/rate_limits)
- [Application](https://app.chargefox.com/)
- [Blog](https://www.chargefox.com/news)
- [Support](https://support.chargefox.com/hc/en-au)
- [Contact](https://www.chargefox.com/get-in-touch)
- [Privacy](https://www.chargefox.com/legal/privacy-policy)
- [Terms of Service](https://www.chargefox.com/legal/terms-conditions)
- [GitHub Organization](https://github.com/chargefox)
- [LinkedIn](https://www.linkedin.com/company/chargefox)
- [About](https://www.chargefox.com/company)

## Mandate Posture

| Field | Value |
| --- | --- |
| Mandate regime | `none` (CDR energy considered and ruled out) |
| Mandate status | `not-applicable` — absent from the ACCC CDR Register's 84 energy data-holder brands, checked 2026-07-27 |
| Data standard | OCPI 2.1.1 / 2.2 / 2.2.1, CPO role (plus OpenAPI 3.0.1 for the Fleets API) |
| Consumer data API | No — no route for an individual driver's usage or billing data, by anyone |
| Market data open | No — locations and tariffs sit behind OCPI Token auth; no anonymous feed anywhere |
| Access gate | `partner-only`, with `customer-account-required` stacked on the Fleets side |
| Auth model | HTTP Bearer (Fleets) and OCPI Token (roaming). No OpenID Connect, no OAuth, no discovery document. |

## Maintainers

- Kin Lane — kin@apievangelist.com
