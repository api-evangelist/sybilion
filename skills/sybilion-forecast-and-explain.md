---
name: Forecast a monthly series and explain what drove it
description: Submit a monthly business time series to Sybilion, poll the async job until it settles, then retrieve the forecast chart and the external-driver attribution artifact.
api: openapi/sybilion-operational-api-openapi.yml
operations:
  - POST /api/v1/forecasts
  - GET /api/v1/forecasts/{id}
  - GET /api/v1/forecasts/{id}/artifacts/{name}
  - GET /api/v1/me
generated: '2026-08-11'
method: generated
source: openapi/sybilion-operational-api-openapi.yml + https://sybilion.dev/docs/quickstart
---

# Forecast a monthly series and explain what drove it

**Operations are named by method + path, not operationId.** The published OpenAPI declares no
`operationId` on any operation, so there is nothing to grep for and nothing to invent. Every step
below cites the method and path exactly as they appear in `paths` in the spec.

## Before you start

- Base URL is `https://api.sybilion.dev`. Auth is `Authorization: Bearer $SYBILION_API_TOKEN`
  (an `sk_ops_…` key from the Developers Portal).
- The series must be **monthly**. Every key is the **first day of the month** (`2024-06-01`, never
  `2024-06-15`) or the request is rejected.
- You need **40–120 observations** depending on the horizon, and the most recent point cannot be
  older than 12 months. Horizons are 1–12.
- Forecasts are **billed** and take **tens of seconds to a few minutes**.

## Step 1 — check the balance before spending it

`GET /api/v1/me`

Read `available_eur_cents`, not `balance_eur_cents`. Available is balance minus active holds, and
the hold is what the forecast submit checks. `api_usage_tier` tells you which concurrency cap you
are under. If `available_eur_cents` is low, stop here and tell the user to top up — do not submit
and collect a 402.

## Step 2 — submit the job

`POST /api/v1/forecasts` → **202** with a `job_id`.

Required body fields (schema `ForecastRequestV1`): `pipeline_version` (`"v1"`), `frequency`
(`"monthly"`), `recency_factor`, `timeseries_metadata`, `timeseries`, plus **at least one** of
`soft_horizon` / `hard_horizon`. Optional: `backtest`, `run_baseline`, `strictly_positive`,
`optimization_budget`, `max_num_features`, `trend_num_classes`, `filters`, `aux_timeseries`.

`timeseries_metadata` is not decoration. `title`, `description` and `keywords` are how the model
matches this series against external drivers — a vague title produces vague attribution.

Optionally narrow the search with `filters.regions[]` and `filters.categories[]`, using ids from
`GET /api/v1/regions` and `GET /api/v1/categories`. Only the integer range 1–9999 is enforced, so a
wrong id will **not** error — it will just quietly filter to nothing. Look ids up; never guess them.

**Do not retry a submit blind.** `X-Request-ID` is not honoured on this operation. A retry creates a
second job and a second hold.

Failure modes to handle before anything else:
- `422` — `{"error":"validation_failed","details":[{field,message}]}`. Exactly one field is reported
  per response, so fix, resubmit, repeat.
- `400` — malformed JSON, or a JSON **type** error (a string where `backtest` or `strictly_positive`
  expects a boolean). This is caught by the decoder, so it is a 400 with a different envelope, not a 422.
- `402` — `insufficient available credits for hold`. Balance, not quota.
- `429` — `too many concurrent jobs` means the tier concurrency cap, evaluated *before* the hold.
  `rate limit` means the per-minute cap. Same status, different fix, and only the message tells you which.
- `413` — the body exceeded 2 MiB.

## Step 3 — poll until it settles

`GET /api/v1/forecasts/{id}` with the `job_id`.

Status moves `queued → running →` terminal. Poll on a backoff — there is **no `Retry-After` and no
rate-limit header of any kind**, so pick a conservative interval (a few seconds, widening) rather
than tight-looping into a 429. When the job is terminal the response carries the `artifacts[]` list
(`name`, `href`, `content_type`, `size`).

If you download too early you get **409** (job not yet completed). That is a "keep polling" signal,
not an error to surface to the user.

## Step 4 — retrieve the outputs

`GET /api/v1/forecasts/{id}/artifacts/{name}` for each artifact you need. `external_signals.json`
carries the ranked external drivers with feature-importance scores and Granger lag relationships —
that is the "why", and it is the reason to use Sybilion over a generic forecaster.

Artifacts are JSON shaped `{"version": "1.1", "data": {…}}`. Treat `version` as a contract version;
new fields may appear inside `data` at the same major version, so read defensively.

Artifacts stream up to 100 MiB and honour `Range` (returns **206**). Use a range read for a large file.

## Step 5 — do not sit on the result

Jobs, forecasts and artifacts fall out of a post-settlement visibility window and then return **404**
— indistinguishable from "never existed" or "not yours". The window length is not published, so
persist anything you need to keep as soon as the job settles.

## Reporting the result

Report the median with the **80% and 90% quantile bands**, and name the top drivers with their lag.
A Sybilion forecast without its attribution is just a number, and the whole point of the artifact is
that the recommendation survives review.

## Correlating a failure

Every response carries an `x-trace-id` header and every error body echoes it as `trace_id`. Include
that value in any support request to support@sybilion.com — it is the only handle Sybilion has on
your call.
