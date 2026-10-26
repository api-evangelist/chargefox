# Chargefox (chargefox)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
