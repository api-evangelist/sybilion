---
name: Use the Sybilion MCP connector from an agent
description: Connect an MCP client to the hosted Sybilion server over OAuth and drive the forecast -> poll -> chart -> alerts tool sequence, knowing which REST capabilities the connector does not expose.
api: mcp/sybilion-mcp.yml
operations:
  - submit_forecast
  - get_forecast
  - get_forecast_chart
  - get_forecast_artifact
  - get_alerts
  - list_regions
  - list_categories
generated: '2026-08-11'
method: generated
source: https://sybilion.dev/docs/integrations + https://www.sybilion.com/mcp
---

# Use the Sybilion MCP connector from an agent

Server: `https://mcp.sybilion.dev/mcp` (Streamable HTTP).

**Tool names below are published by Sybilion in its own docs and MCP page. They were not obtained by
introspection** — anonymous `tools/list` returns `401 no bearer token` with
`WWW-Authenticate: Bearer resource_metadata="https://mcp.sybilion.dev/.well-known/oauth-protected-resource/mcp"`.
Input schemas are only visible to an authenticated client. The argument shapes cited here are the
ones Sybilion publishes in its troubleshooting table.

## Connecting

Authentication is **OAuth, not an API key** — there is nothing to paste into the connector config.
The server implements the full discovery chain, so a compliant client needs only the URL:

- `/.well-known/oauth-protected-resource` → names the authorization server
- `/.well-known/oauth-authorization-server` → authorize, token, revoke, register endpoints
- dynamic client registration at `/oauth/register` (so no pre-registered client id is needed)
- PKCE `S256`, scopes `openid profile email offline_access`

Client-specific setup:
- **Claude** — Customize → Connectors → Add custom connector → the URL. Team/Enterprise: an Owner
  adds it org-wide first.
- **ChatGPT** — requires Developer Mode and a paid plan; Settings → Apps → Advanced settings →
  Create App with the connector URL, keep OAuth.
- **TradingView Remix** — side panel → Settings → MCP Servers → add the URL with OAuth.

The scopes are identity scopes only. There is no resource scope, so approving the connector grants
every tool it exposes, including the two that spend money. Treat consent as all-or-nothing.

## The tool sequence

For a forecast question: `submit_forecast` → `get_forecast` → `get_forecast_chart`
(→ `get_forecast_artifact` for the driver detail).

For "what is moving against us right now": `get_alerts` on its own.

Scope either with catalog ids from `list_regions` and `list_categories` first.

## Things that will bite an agent

- **Forecasts are async and slow.** The connector paces its own polling and can take tens of seconds
  to a few minutes. Do not re-invoke `submit_forecast` because nothing came back — you will start a
  second billed job.
- **`get_alerts` is a billed write call**, despite reading like a lookup. So is `submit_forecast`.
  Some clients ask for per-call approval on write tools; that is correct behaviour, not a fault.
- **Arguments must be a structured object, not a string.** Sybilion documents this as a real
  client bug: if retrieval fails, call again with `{"job_id": "<uuid>"}` for `get_forecast` and
  `get_forecast_chart`, and `{"job_id": "<uuid>", "artifact_name": "<name>"}` for
  `get_forecast_artifact`.
- **Monthly series only.** Sub-monthly frequency is roadmap, not shipped. The data rules from the
  REST API still apply: first-day-of-month keys, 40–120 observations, most recent point within
  12 months, horizons 1–12.
- **There is no drivers tool.** `POST /api/v1/drivers` exists on REST but has no MCP counterpart. To
  explain a series through the connector you must run a forecast and read
  `external_signals.json` via `get_forecast_artifact`. If a user asks only "what drives X", the
  connector still costs them a forecast.
- **No jobs, usage or billing tools.** You cannot list past jobs or read the billing history from
  MCP. An account tool exists (the ChatGPT setup notes mention account tools and "check my balance"
  is a documented prompt) but its name is not published anywhere, so do not assume a specific one.
- **Results expire.** Retrieve and summarise artifacts in the same session; they fall out of a
  post-settlement visibility window of unpublished length.

## When to use REST instead

Use the REST API directly when you need standalone driver ranking, job history, usage/billing
reconciliation, or `X-Request-ID` retry-safety on a billed call. The connector is the conversational
surface; it is deliberately narrower than the API underneath it.
