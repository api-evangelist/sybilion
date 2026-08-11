---
name: Rank external drivers for a series without double-billing
description: Call the synchronous billed drivers endpoint with a stable X-Request-ID so retries and upstream 502s cannot charge twice, using the region and category catalogs to scope the search.
api: openapi/sybilion-operational-api-openapi.yml
operations:
  - GET /api/v1/regions
  - GET /api/v1/categories
  - GET /api/v1/me
  - POST /api/v1/drivers
  - GET /api/v1/usage
generated: '2026-08-11'
method: generated
source: openapi/sybilion-operational-api-openapi.yml + https://sybilion.dev/docs/using-curl
---

# Rank external drivers for a series without double-billing

`POST /api/v1/drivers` is **synchronous and billed**. Unlike forecasts it charges immediately, which
makes retry discipline the whole skill. Note that this capability has **no MCP tool** — if you are
working through the Sybilion MCP connector you cannot reach it, and driver attribution is only
available as a by-product of a completed forecast.

Operations are cited by method + path because the published OpenAPI declares no `operationId`.

## Step 1 — resolve catalog ids first

`GET /api/v1/regions` → `{"items":[{"id","name","latitude","longitude"}]}`
`GET /api/v1/categories` → `{"items":[{"id","name"}]}`

Both return the complete list with no pagination, sorted by `id` ascending. Resolve the user's
words ("Europe", "Energy") to ids here. The API validates only that a filter id is an integer in
1–9999 — an id that does not exist is accepted and silently narrows the search to nothing. Never
guess an id.

## Step 2 — size the spend

`GET /api/v1/me` and read `available_eur_cents`.

The cost ceiling scales with `filters.limit`. If the pre-check finds the available balance below the
worst-case ceiling you get **402** — either `insufficient credits` or the more useful
`insufficient credits: need up to N, have M`. Two fixes: top up, or lower `filters.limit` until the
ceiling fits. Prefer lowering the limit when the user just wants the top drivers.

## Step 3 — mint a request id, then call

Generate a UUID **once**, before the first attempt:

```
REQ_ID=$(uuidgen)
curl -sS -X POST https://api.sybilion.dev/api/v1/drivers \
  -H "Authorization: Bearer $SYBILION_API_TOKEN" \
  -H "Content-Type: application/json" \
  -H "X-Request-ID: $REQ_ID" \
  -d @drivers_body.json
```

Body (schema `RecommendRequestV1`) requires `version` (`"v1"`), `recency_factor` and
`timeseries_metadata`; `timeseries` and `filters` are optional. `version` selects the validator only
and is stripped before the upstream call.

**Reuse the same `X-Request-ID` on every retry of that same logical request.** That is what
deduplicates the charge on success. Minting a fresh id on retry is how you get billed twice.

## Step 4 — handle the failures correctly

- **502** — upstream transport failure. This is the case the header exists for: retry with the
  **same** `X-Request-ID`. The docs say so explicitly.
- **503** — do **not** treat this as transient. On drivers it commonly means the feature is not
  enabled for this account. Retrying will not help; tell the user to contact support@sybilion.com.
- **429** — the per-minute cap on synchronous billed calls. There is no `Retry-After` and no
  rate-limit header, so back off on your own schedule and check the tier in the Developers Portal.
- **422** — `{"error":"validation_failed","details":[…]}`, one field at a time.
- **401** — bad or missing bearer key.

## Step 5 — reconcile what you were charged

`GET /api/v1/usage?page=1&limit=20&sort=created_at&order=desc`

Each row is one billed event: `endpoint`, `units`, `credits_charged`, `eur_cents_charged`,
`created_at`, and `async_job_id` when it came from a forecast. If a retried call produced **two**
rows for one logical request, the `X-Request-ID` dedupe did not take — capture the `trace_id` from
the responses and raise it with support.

Paginate with `page`/`limit` (1–200, default 50) and stop when `page == pagination.total_pages`.

## Reading the output

`DriverItemV1` gives you `driver_name`, `hash_id` and `score`. `hash_id` is the only stable handle
for an external series — but note that **no operation accepts it as an input**, so you cannot ask
for that driver again by id. If the user will want to come back to a driver, persist the name and
score yourself.
