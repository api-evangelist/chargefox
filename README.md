# Chargefox

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

Chargefox is Australia's largest public electric-vehicle charging network and, since 2023, a charging software platform rather than a hardware owner. Founded in 2017 and headquartered in Melbourne, it was acquired outright in 2022 by Australian Motoring Services, the joint vehicle of six state motoring clubs — NRMA, RACV, RACQ, RAA, RAC and RACT — which makes it one of the very few member-owned pieces of national energy infrastructure in the country. Its own company page claims 2,200+ public charging plugs, 5,000+ charging sessions a day and 170,000+ app downloads. In the Australian energy value chain it sits downstream of the retailer and the meter: it does not generate, transmit or sell electricity as a licensed retailer, it operates the charge points other businesses, councils and governments own, authorises drivers, meters the session, prices it and settles it. That position is exactly why the Consumer Data Right does not touch it. Chargefox does NOT appear among the 84 energy data-holder brands on the ACCC CDR Register checked on 2026-07-27 — even though Arcline by RACV, an energy retailer owned by one of Chargefox's own shareholder clubs, does — so the statutory mandate that produced Australia's identical fifty-bank banking contract stops at the retail electricity licence and never reaches the charge point. What Chargefox has instead is an entirely voluntary, entirely commercial API posture built on a genuine industry standard. It publishes a real, anonymously readable developer documentation site at https://app.chargefox.com/developers/docs carrying a Redoc-rendered OpenAPI 3.0.1 contract for a four-endpoint Fleets API, and its own rate-limit documentation enumerates a full Open Charge Point Interface CPO implementation across OCPI 2.1.1, 2.2 and 2.2.1 covering locations, sessions, CDRs, tariffs, tokens and commands. Every one of those endpoints is closed. Anonymous probes returned 401 with `WWW-Authenticate: Token realm="Application"` on the OCPI paths and 401 on the Fleets paths, and no anonymous locations, tariff or network-status feed of any kind could be found, so Chargefox publishes zero open market data and zero consumer data — a documented standard, fully implemented, entirely behind a commercial gate.

**Areas:** Energy · Australia · EV Charging · Electricity · Utilities · OCPI · Charge Point Operator · Roaming · Fleet Management · Mobility · Charging Sessions · Electrification

## Chargefox on APIs.io

Everything profiled for Chargefox is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/chargefox/](https://apis.io/providers/chargefox/)** — or go straight to one of the APIs or properties below.

### APIs (2)

- [Chargefox OCPI CPO API](https://apis.io/apis/chargefox/chargefox-ocpi-cpo-api/) — Chargefox's Open Charge Point Interface implementation in the Charge Point Operator role, used for roaming so that another network's drivers can authorise,…
- [Chargefox Fleets API](https://apis.io/apis/chargefox/chargefox-fleets-api/) — The Fleets API from Chargefox — 4 operation(s) for fleets.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [chargefox-mcp.yml](https://apis.io/servers/chargefox/chargefox-mcp-yml/)

#### Examples (4)

Example requests and responses.

- [Chargefox Fleets Invoices 200](https://apis.io/examples/chargefox/chargefox-fleets-invoices-200/)
- [Chargefox Fleets Sessions 200](https://apis.io/examples/chargefox/chargefox-fleets-sessions-200/)
- [Chargefox Fleets Usage 200](https://apis.io/examples/chargefox/chargefox-fleets-usage-200/)
- [Chargefox Fleets Vehicles 200](https://apis.io/examples/chargefox/chargefox-fleets-vehicles-200/)

#### Collections (1)

Postman and Open Collection request collections.

- [Fleets API](https://apis.io/collections/chargefox/open-chargefox-fleets-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Chargefox Fleets Api Overlay](https://apis.io/overlays/chargefox/chargefox-fleets-api-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Chargefox Authentication](https://apis.io/security/chargefox/chargefox-authentication/)
- [Chargefox Domain Security](https://apis.io/security/chargefox/chargefox-domain-security/)
- [Chargefox Trust Center](https://apis.io/security/chargefox/chargefox-trust-center/)

#### Rate limits (1)

Published request limits.

- [Chargefox Rate Limits](https://apis.io/rate-limits/chargefox/chargefox-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.chargefox.com/)
- [Documentation](https://app.chargefox.com/developers/docs/getting_started)
- [Getting started](https://app.chargefox.com/developers/docs/getting_started)
- [API reference](https://app.chargefox.com/developers/docs/fleets)
- [Support](https://support.chargefox.com/hc/en-au)
- [Blog](https://www.chargefox.com/news)
- [Trust centre](https://trust.chargefox.com/)
- [Terms of service](https://www.chargefox.com/legal/terms-conditions)
- [Privacy policy](https://www.chargefox.com/legal/privacy-policy)
- [GitHub](https://github.com/chargefox)
- [Status](https://www.chargefox.com/status)

## API Evangelist rating

**Kin Score 47.9** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 48.0 |
| Contract governance | 18.2 |
| Operational transparency | 50.0 |
| Developer ergonomics | 58.9 |
| Access clarity | 35.5 |

**Agent readiness 33.1** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | na |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | na |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | verified |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | na |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 24.6** — Energy & Utilities

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/chargefox/](https://apis.io/providers/chargefox/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Chargefox: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/chargefox/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
