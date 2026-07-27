---
name: Reconcile fleet charging costs against a Chargefox invoice
description: >-
  Pull a Chargefox fleet's charge-session usage for a billing period, pull the
  invoices for the same period, and reconcile session-level cost against the
  invoiced amount so a finance or fleet-ops agent can explain every line.
api: openapi/chargefox-fleets-api-openapi.json
operations:
  - GET /api/fleets/v1/usage
  - GET /api/fleets/v1/invoices
operation_ids:
  - listFleetUsage
  - listFleetInvoices
operation_id_note: >-
  The harvested contract declares no operationId on any operation. The ids above
  are assigned by overlays/chargefox-fleets-api-overlay.yaml; the authoritative
  binding is the method + path pair, which is verbatim from the spec.
---

# Reconcile fleet charging costs against a Chargefox invoice

Use this when a fleet customer asks "what did we actually spend on charging last
month, and does the invoice match?"

## Before you start

- **You need a token you already have.** Chargefox issues fleet bearer tokens
  manually — there is no signup, no sandbox and no self-service key. If you do
  not have one, the only correct action is to tell the user to contact
  `fleetsupport@chargefox.com`. Do not attempt to obtain a credential.
- Base URL is `https://app.chargefox.com`. Send
  `Authorization: Bearer <token>` on every request.
- Everything in this skill is a `GET`. Nothing here changes any state.

## Steps

1. **Fetch the period's usage.**
   `GET /api/fleets/v1/usage?date_from=<YYYY-MM-DD>&date_to=<YYYY-MM-DD>`
   Records come back under `data.sessions[]`. Each session carries `uuid`,
   `auth_id` (the RFID tag that started it), `vehicle`, `customer`, `location`,
   `consumption` (kWh), `cost_to_driver`, `invoice_number` and a
   `charging_periods[]` array where the real money lives — each period has its
   own `tariff`, `gst`, `amount` and `currency`.

2. **Page through everything.** The page size is fixed at 100. Read
   `pagination.total_entries` to know how many records exist and follow
   `pagination.next` — an absolute URI — until it is absent. Do not guess page
   counts.

3. **Respect the budget between pages.** `/usage` allows **18 requests per 15
   minutes** and **50 per hour**, per token. Read `X-RateLimit-Remaining` on
   every response; when you hit a `429`, sleep until the Unix timestamp in
   `X-RateLimit-Reset` and resume. A 12-month pull of a large fleet will exceed
   the hourly ceiling — plan the pull, do not hammer it.

4. **Fetch the invoices for the same window.**
   `GET /api/fleets/v1/invoices?date_from=<YYYY-MM-DD>&date_to=<YYYY-MM-DD>`
   Records come back under `data.invoices[]`. Each invoice carries
   `invoice_number`, `invoice_date`, `status`, `currency`, `total_tax`,
   `total_amount_incl_tax`, `total_paid_incl_tax`, `total_due_incl_tax`, plus
   embedded `line_items[]`, `vehicles[]` and a flattened `charge_sessions[]`.
   This endpoint's limit is tighter still — **18 requests per hour**.

5. **Join on `invoice_number`.** Every billed session in `data.sessions[]`
   carries the `invoice_number` it was billed on; a session with
   `invoice_number: null` is **not yet invoiced** and must be excluded from the
   reconciliation, not treated as a discrepancy.

6. **Reconcile.** For each invoice, sum `charging_periods[].amount` across the
   sessions bearing that `invoice_number` and compare against the charging line
   items on the invoice. Then cross-check the embedded
   `invoice.charge_sessions[]` — it is the provider's own flattened view of the
   same sessions, with `total_cost_incl_tax` per session — and report any
   session present in one view but not the other.

7. **Report GST separately.** Chargefox splits tax out on every object
   (`charging_periods[].gst`, `invoice.total_tax`, and the `*_incl_tax` fields).
   Never add a GST field to an `incl_tax` field — the tax is already inside it.

## Rules that will bite you

- **Duration units are unreliable.** The schema describes `total_time` and
  `charging_periods[].duration` as minutes, but the published examples carry
  second-scale values (a 15-day session shows `total_time: 1262880`). Treat them
  as seconds and say so in any output. Never present a duration without stating
  the assumption.
- **`/usage` and `/sessions` are different questions.** `/usage` is the fleet as
  a *driver* — its vehicles charging anywhere on the network, priced with
  `cost_to_driver`. `/sessions` is the fleet as a *site host* — anyone charging
  on its stations, with `total_revenue` and `total_cost`. Using the wrong one
  produces a confidently wrong number. If the fleet both drives and hosts, you
  need both.
- **Currency is explicit.** Read the `currency` field; do not assume AUD even
  though every published example is AUD.
- **Errors carry no body.** `401`, `403` and `429` are declared with no response
  schema and no error code. `401` means the token is missing or invalid, `403`
  means the token is not entitled to that resource. Do not retry a `401` or
  `403` — only a `429` is retryable, and only after `X-RateLimit-Reset`.

## Cross-references

- `conventions/chargefox-conventions.yml` — pagination, filtering, units, money
- `rate-limits/chargefox-rate-limits.yml` — all published limits
- `errors/chargefox-problem-types.yml` — the full declared error surface
- `data-model/chargefox-data-model.yml` — how sessions, vehicles and invoices link
- `examples/chargefox-fleets-usage-200.json`, `examples/chargefox-fleets-invoices-200.json` — real response shapes
