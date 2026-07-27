---
name: Audit a Chargefox fleet's vehicle register and RFID cards
description: >-
  Pull a fleet's managed-vehicle register from Chargefox, identify active versus
  retired vehicles and vehicles with no RFID card attached, then cross-check the
  register against which cards actually appear on charge sessions.
api: openapi/chargefox-fleets-api-openapi.json
operations:
  - GET /api/fleets/v1/vehicles
  - GET /api/fleets/v1/usage
operation_ids:
  - listFleetVehicles
  - listFleetUsage
operation_id_note: >-
  The harvested contract declares no operationId. These ids are assigned by
  overlays/chargefox-fleets-api-overlay.yaml; the authoritative binding is the
  method + path pair, verbatim from the spec.
---

# Audit a Chargefox fleet's vehicle register and RFID cards

Use this when a fleet needs to know which vehicles are on the platform, which
are being billed, and which RFID cards are unaccounted for.

## Before you start

- Requires a Chargefox fleet bearer token. There is no self-service issuance —
  if the user has no token, direct them to `fleetsupport@chargefox.com` and
  stop.
- Base URL `https://app.chargefox.com`, header `Authorization: Bearer <token>`.
- Read-only. This skill cannot add, retire or modify a vehicle — Chargefox
  exposes no write operations on the public API. Register changes go through
  Chargefox or the dashboard.

## Steps

1. **Pull the register.** `GET /api/fleets/v1/vehicles`
   Records arrive under `data.vehicles[]`. Each vehicle has `uuid`,
   `contract_number`, `vin`, `registration_number`, `effective_from`,
   `effective_to`, an embedded `customer` (the fleet organisation) and a
   `cards[]` array of `id_tag` values.

   Note this operation takes **only** `page` — there is no `date_from` /
   `date_to` filter here, unlike the other three operations. Filter by
   `effective_from` / `effective_to` yourself after fetching.

2. **Page to the end.** Fixed 100 records per page. Follow
   `pagination.next` until absent; `pagination.total_entries` tells you the
   expected total. This is the one operation with **no published rate limit and
   no declared 429 response** — it is still polite to page sequentially rather
   than in parallel.

3. **Classify each vehicle.**
   - *Active*: `effective_to` is `null`.
   - *Retired*: `effective_to` is a date — it will still appear on historical
     invoices and sessions before that date. Do not delete it from a report.
   - *Uncarded*: `cards[]` is empty. This vehicle cannot start a charge session
     via RFID and is a common cause of "why is this car never in the usage
     report".
   - Multiple fleets can appear in one response — group by `customer.uuid`, not
     by assuming a single organisation. The published example returns vehicles
     for two distinct customers.

4. **Cross-check against real charging.**
   `GET /api/fleets/v1/usage?date_from=<YYYY-MM-DD>&date_to=<YYYY-MM-DD>`
   Every session carries `auth_id` — the RFID `id_tag` that started it — and a
   `vehicle` object with `vin`, `contract_number` and `registration_number`.
   Build the set of `id_tag`s from step 1 and the set of `auth_id`s from the
   sessions, then report:
   - **Cards on the register that never charged** in the period (idle assets, or
     a card issued but never handed out).
   - **`auth_id` values seen on sessions that are not on the register** — an
     unregistered or recently reassigned card, worth flagging.
   - **Active vehicles with zero sessions** — join by `vin`, which the contract
     states must be unique. `contract_number` and `registration_number` are both
     nullable on the session-side vehicle object, so `vin` is the only reliable
     join key.

5. **Mind the rate budget on step 4.** `/usage` allows 18 requests / 15 minutes
   and 50 / hour per token. If the register is large and the window is long,
   pull the vehicle register first (cheap, unlimited) and the usage second.

## Rules that will bite you

- **`vin` is the join key, not `registration_number`.** Registration is nullable
  on session vehicle objects and changes when a plate is transferred.
- **A retired vehicle is not an error.** `effective_to` in the past plus
  historical sessions is the normal shape of a fleet that turns over cars.
- **Do not infer a card is inactive from its absence in sessions.** The register
  only lists *active* cards attached to the vehicle; a card that charged in a
  prior period may have since been detached.
- `401` = token missing/invalid, `403` = token not entitled to this fleet.
  Neither is retryable and neither returns an error body.

## Cross-references

- `data-model/chargefox-data-model.yml` — Vehicle → Card → ChargeSession links
- `conventions/chargefox-conventions.yml` — pagination and filtering rules
- `rate-limits/chargefox-rate-limits.yml` — why `/vehicles` is the cheap call
- `examples/chargefox-fleets-vehicles-200.json` — a register with a retired and an uncarded vehicle
