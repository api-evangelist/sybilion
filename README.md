# Sybilion

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

Sybilion is a decision layer for industrial companies, providing economic and causal forecasting that connects external market dynamics to internal exposure for procurement, trading and risk teams. It forecasts monthly business time series up to twelve months ahead with 80% and 90% quantile bands, ranks the external macroeconomic drivers that move a series with Granger lag relationships and feature-importance scores, backtests against the last twelve months, and raises alerts on the market movements turning relevant to a customer's exposure right now. The same causal model is reachable two ways: an anonymously-specified OpenAPI 3.0.3 REST API at api.sybilion.dev using Bearer API keys, and a hosted OAuth MCP server at mcp.sybilion.dev that ChatGPT, Claude and the TradingView Remix extension connect to directly. Billing is a prepaid EUR-cent balance with expiring credit tranches and pre-flight holds. Official Python, Go, TypeScript and R SDKs are published.

**Areas:** Industrial market intelligence · Commodity price forecasting · Economic Forecasting · Time-series forecasting · Procurement · Supply Chain Risk · Trading analytics · AI decision support · MCP · Agent-Native · Causal Inference · Anomaly Detection

## Sybilion on APIs.io

Everything profiled for Sybilion is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/sybilion/](https://apis.io/providers/sybilion/)** — or go straight to one of the APIs or properties below.

### APIs (10)

- [Sybilion MCP Server](https://apis.io/apis/sybilion/sybilion-mcp-server/) — Hosted Streamable-HTTP MCP server providing agent-native tools for forecasts (submit_forecast, get_forecast, get_forecast_chart, get_forecast_artifact),…
- [Sybilion Alerts API](https://apis.io/apis/sybilion/sybilion-alerts-api/) — The Alerts API from Sybilion — 1 operation(s) for alerts.
- [Sybilion Categories API](https://apis.io/apis/sybilion/sybilion-categories-api/) — The Categories API from Sybilion — 1 operation(s) for categories.
- [Sybilion Drivers API](https://apis.io/apis/sybilion/sybilion-drivers-api/) — The Drivers API from Sybilion — 1 operation(s) for drivers.
- [Sybilion Forecasts API](https://apis.io/apis/sybilion/sybilion-forecasts-api/) — The Forecasts API from Sybilion — 3 operation(s) for forecasts.
- [Sybilion Health API](https://apis.io/apis/sybilion/sybilion-health-api/) — The Health API from Sybilion — 1 operation(s) for health.
- [Sybilion Jobs API](https://apis.io/apis/sybilion/sybilion-jobs-api/) — The Jobs API from Sybilion — 1 operation(s) for jobs.
- [Sybilion Me API](https://apis.io/apis/sybilion/sybilion-me-api/) — The Me API from Sybilion — 1 operation(s) for me.
- [Sybilion Regions API](https://apis.io/apis/sybilion/sybilion-regions-api/) — The Regions API from Sybilion — 1 operation(s) for regions.
- [Sybilion Usage API](https://apis.io/apis/sybilion/sybilion-usage-api/) — The Usage API from Sybilion — 1 operation(s) for usage.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [sybilion](https://apis.io/servers/sybilion/sybilion/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Sybilion Agentic Access](https://apis.io/agentic-access/sybilion/sybilion-agentic-access/)

#### Collections (1)

Postman and Open Collection request collections.

- [Sybilion API](https://apis.io/collections/sybilion/open-sybilion-operational-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Sybilion Operational Api Overlay](https://apis.io/overlays/sybilion/sybilion-operational-api-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Sybilion Authentication](https://apis.io/security/sybilion/sybilion-authentication/)
- [Sybilion Domain Security](https://apis.io/security/sybilion/sybilion-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Sybilion Scopes](https://apis.io/scopes/sybilion/sybilion-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Sybilion Plans Pricing](https://apis.io/plans/sybilion/sybilion-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Sybilion Rate Limits](https://apis.io/rate-limits/sybilion/sybilion-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.sybilion.com/)
- [Documentation](https://sybilion.dev/docs/)
- [Getting started](https://sybilion.dev/docs/quickstart)
- [API reference](https://api.sybilion.dev/docs)
- [Pricing](https://sybilion.dev/docs/tiers)
- [Sign up](https://sybilion.dev/signup)
- [Support](https://sybilion.dev/docs/community)
- [Blog](https://www.sybilion.com/blog)
- [Privacy policy](https://www.sybilion.com/legal/privacy-policy)
- [GitHub](https://github.com/Sybilion-AI)

## API Evangelist rating

**Kin Score 47.2** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 68.3 |
| Contract quality | 47.9 |
| Contract governance | 4.5 |
| Operational transparency | 34.2 |
| Developer ergonomics | 56.5 |
| Access clarity | 52.6 |

**Agent readiness 55.6** — agent-native

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | served (partial credit) |
| Idempotency | documented (partial credit) |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 30.4** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/sybilion/](https://apis.io/providers/sybilion/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Sybilion: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/sybilion/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
