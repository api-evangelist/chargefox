---
name: Report charge-station revenue for a Chargefox site host
description: >-
  Pull charge sessions completed on a fleet's own charge stations and produce a
  site-host revenue report — revenue, cost, margin and GST, broken down by
  location, EVSE and connector.
api: openapi/chargefox-fleets-api-openapi.json
operations:
  - GET /api/fleets/v1/sessions
  - GET /api/fleets/v1/invoices
operation_ids:
  - listFleetSessions
  - listFleetInvoices
operation_id_note: >-
  The harvested contract declares no operationId. These ids are assigned by
  overlays/chargefox-fleets-api-overlay.yaml; the authoritative binding is the
  method + path pair, verbatim from the spec.
---

# Report charge-station revenue for a Chargefox site host

Use this for the *other* side of the Chargefox relationship: an organisation
that **owns or hosts charge stations** and wants to know what those stations
earned and cost. This is a different question from fleet charging spend — see
`chargefox-reconcile-fleet-charging-costs.md` for that.

## Before you start

- Requires a Chargefox bearer token entitled to the site host's organisation.
  Not self-service; contact `fleetsupport@chargefox.com`.
- Base URL `https://app.chargefox.com`, header `Authorization: Bearer <token>`.
- Read-only. There is no operation to change a tariff, open or close a station,
  or start/stop a session on the public API.

## Steps

1. **Pull the sessions on your stations.**
   `GET /api/fleets/v1/sessions?date_from=<YYYY-MM-DD>&date_to=<YYYY-MM-DD>`
   Records arrive under `data.sessions[]`. This projection is site-host shaped —
   each session carries:
   - `location` with `name`, `address`, `timezone`, an `operator` (the charging
     network, Chargefox) and a `sub_operator` (the organisation managing the
     station), plus the `evse` and its `connector`
   - `total_revenue` and `gst_on_total_revenue`
   - `total_cost` and `gst_on_total_cost`
   - `consumption` (kWh), `total_time`, `total_parking_time`
   - `charging_periods[]` with per-block `tariff`, `gst` and `amount`

   Note it does **not** carry `vehicle`, `customer` or `cost_to_driver` — those
   only exist on `/usage`. A site host does not get to see who the driver is
   beyond the `auth_id`.

2. **Page and pace.** 100 records per page; follow `pagination.next`. The
   budget is **18 requests / 15 minutes** and **50 / hour**, per token. A busy
   site over a long window will hit the hourly ceiling — read
   `X-RateLimit-Remaining`, and on a `429` wait until the Unix timestamp in
   `X-RateLimit-Reset`.

3. **Group and aggregate.** Roll up by `location.name`, then by
   `location.evse.evse_id`, then by `location.evse.connector.id`. For each
   group report:
   - energy delivered = sum of `consumption` (kWh)
   - gross revenue = sum of `total_revenue`
   - cost = sum of `total_cost`
   - margin = revenue − cost
   - GST collected = sum of `gst_on_total_revenue`
   - GST paid = sum of `gst_on_total_cost`
   - utilisation signal = sum of `total_time` versus sum of
     `total_parking_time` — idle time after the charge completes is the number
     that drives whether an idle fee is worth introducing.

4. **Convert times to the station's local day.** Each `location` carries an IANA
   `timezone` (e.g. `Australia/Sydney`) and every timestamp carries a UTC offset.
   Chargefox's status page runs on `Australia/Melbourne`. For a national host
   with stations across multiple states, group by local calendar day using the
   station's own timezone — never by UTC day, or the daily peaks will smear.

5. **Tie back to the invoice, if asked.**
   `GET /api/fleets/v1/invoices?date_from=<YYYY-MM-DD>&date_to=<YYYY-MM-DD>`
   Site-host invoices come back on the same endpoint as fleet invoices — the
   contract states it "includes both fleet and site host invoices". Read
   `description` and `line_items[].description` to tell them apart; there is no
   type field. Limit here is **18 requests per hour**.

## Rules that will bite you

- **The schema says `sub_operator`; the published example says `suboperator`.**
  Read both keys and prefer whichever is present. This is a real inconsistency
  in Chargefox's own contract, recorded in
  `data-model/chargefox-data-model.yml`.
- **A session can have zero revenue.** Free or comped charging appears as
  `total_revenue: 0` with a real `consumption` and a real `total_cost` — a
  negative-margin session, not a data error.
- **`tariff` is per charging period, not per session.** A single session
  routinely splits into an energy block and a parking block with different
  tariffs; summing `charging_periods[].amount` is the only correct way to derive
  what was charged.
- **Duration fields are described as minutes but published as second-scale
  values.** Treat as seconds and state the assumption.
- `401` = missing/invalid token, `403` = not entitled to that site host's data,
  `429` = throttled. Only `429` is retryable; none returns an error body.

## Cross-references

- `conventions/chargefox-conventions.yml` — envelope, pagination, money and units
- `rate-limits/chargefox-rate-limits.yml` — the sessions and invoices budgets
- `data-model/chargefox-data-model.yml` — Location → EVSE → Connector, and the two session projections
- `examples/chargefox-fleets-sessions-200.json` — the site-host response shape
