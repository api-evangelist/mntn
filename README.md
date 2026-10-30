# MNTN

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

MNTN (mountain.com) is a Connected TV performance marketing platform that lets brands plan, launch, target, measure, and optimize outcome-based TV advertising across streaming inventory. Its programmable surface is two distinct HTTP APIs: the Performance TV (PTV) platform API at api.mountain.com, an OpenAPI 3.1 contract covering advertisers, organizations, campaigns, flights, creatives, audiences and geo-lists, conversion pixel setup and verification, attribution windows and estimates, private marketplace (PMP) partners/deals/deal-groups/channels, reference vocabularies, and reporting; and the Reporting API 3.0 at api3.mountain.com, a partner reporting surface (/apilist metadata, /apidata query execution) plus an asynchronous /batch CSV export API that returns time-limited signed download URLs. Both are documented with published OpenAPI definitions rendered through Scalar. Authentication is an MNTN-issued advertiser API key (X-API-Key header on the platform API, key query parameter on the reporting API) or a bearer JWT. Errors are RFC 9457 problem+json with traceId and errorCode.

**Areas:** Connected TV · ctv-advertising · Advertising · Performance Marketing · Streaming TV · Media Buying · Attribution · Audience Targeting · Conversion Tracking · Programmatic Advertising · private-marketplace · marketing-reporting

## MNTN on APIs.io

Everything profiled for MNTN is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/mntn/](https://apis.io/providers/mntn/)** — or go straight to one of the APIs or properties below.

### APIs (18)

- [MNTN Reporting API 3.0](https://apis.io/apis/mntn/mntn-reporting-api-30/) — Public partner reporting surface at api3.mountain.com. GET /apilist returns the tables and columns available to the authenticated advertiser; GET and POST…
- [MNTN Advertisers API](https://apis.io/apis/mntn/mntn-advertisers-api/) — Advertiser directory.
- [MNTN Attribution API](https://apis.io/apis/mntn/mntn-attribution-api/) — Advertiser attribution windows, estimates, sales cycle, and verified-visit blacklist settings.
- [MNTN Audiences API](https://apis.io/apis/mntn/mntn-audiences-api/) — Audience and geo-list management.
- [MNTN Batch API](https://apis.io/apis/mntn/mntn-batch-api/) — Async CSV exports under **`/batch`…** (submit, poll status, signed download URLs)
- [MNTN Campaigns API](https://apis.io/apis/mntn/mntn-campaigns-api/) — Campaign list, lookup, and lifecycle.
- [MNTN Creatives API](https://apis.io/apis/mntn/mntn-creatives-api/) — Creative library lookup, with normalized video processing status.
- [MNTN Flights API](https://apis.io/apis/mntn/mntn-flights-api/) — Flight (budget window) reads and updates within a campaign.
- [MNTN Organizations API](https://apis.io/apis/mntn/mntn-organizations-api/) — Organization name and mode updates (proxied to advertiser-service).
- [MNTN Pixel API](https://apis.io/apis/mntn/mntn-pixel-api/) — Pixel configuration, health, verification, and GA4 management.
- [MNTN Pmp Campaign Deals API](https://apis.io/apis/mntn/mntn-pmp-campaign-deals-api/) — Deal-group assignment to campaigns.
- [MNTN Pmp Channels API](https://apis.io/apis/mntn/mntn-pmp-channels-api/) — Private marketplace channel reference set.
- [MNTN Pmp Deal Groups API](https://apis.io/apis/mntn/mntn-pmp-deal-groups-api/) — Private marketplace deal-group management and deal membership.
- [MNTN Pmp Deals API](https://apis.io/apis/mntn/mntn-pmp-deals-api/) — Private marketplace deal reads and lifecycle.
- [MNTN Pmp Partners API](https://apis.io/apis/mntn/mntn-pmp-partners-api/) — Private marketplace partner directory.
- [MNTN Reference API](https://apis.io/apis/mntn/mntn-reference-api/) — Enumerations and lookup data shared across endpoints.
- [MNTN Apidata API](https://apis.io/apis/mntn/mntn-apidata-api/) — The Apidata API from MNTN — 1 operation(s) for apidata.
- [MNTN Apilist API](https://apis.io/apis/mntn/mntn-apilist-api/) — The Apilist API from MNTN — 1 operation(s) for apilist.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [mntn-mcp.yml](https://apis.io/servers/mntn/mntn-mcp-yml/)

#### Collections (17)

Postman and Open Collection request collections.

- [MNTN Async Batch Export API - Batch](https://apis.io/collections/mntn/open-mntn-batch-export/)
- [PTV API - advertisers](https://apis.io/collections/mntn/open-mntn-ptv-advertisers/)
- [PTV API - attribution](https://apis.io/collections/mntn/open-mntn-ptv-attribution/)
- [PTV API - audiences](https://apis.io/collections/mntn/open-mntn-ptv-audiences/)
- [PTV API - campaigns](https://apis.io/collections/mntn/open-mntn-ptv-campaigns/)
- [PTV API - creatives](https://apis.io/collections/mntn/open-mntn-ptv-creatives/)
- [PTV API - flights](https://apis.io/collections/mntn/open-mntn-ptv-flights/)
- [PTV API - organizations](https://apis.io/collections/mntn/open-mntn-ptv-organizations/)
- [PTV API - pixel](https://apis.io/collections/mntn/open-mntn-ptv-pixel/)
- [PTV API - pmp-campaign-deals](https://apis.io/collections/mntn/open-mntn-ptv-pmp-campaign-deals/)
- [PTV API - pmp-channels](https://apis.io/collections/mntn/open-mntn-ptv-pmp-channels/)
- [PTV API - pmp-deal-groups](https://apis.io/collections/mntn/open-mntn-ptv-pmp-deal-groups/)
- [PTV API - pmp-deals](https://apis.io/collections/mntn/open-mntn-ptv-pmp-deals/)
- [PTV API - pmp-partners](https://apis.io/collections/mntn/open-mntn-ptv-pmp-partners/)
- [PTV API - reference](https://apis.io/collections/mntn/open-mntn-ptv-reference/)
- [PTV API - reporting](https://apis.io/collections/mntn/open-mntn-ptv-reporting/)
- [MNTN Reporting API - API](https://apis.io/collections/mntn/open-mntn-reporting-api/)

#### Overlays (3)

OpenAPI overlays, including conversational phrasing for each operation.

- [Mntn Batch Overlay](https://apis.io/overlays/mntn/mntn-batch-overlay/)
- [Mntn Ptv Overlay](https://apis.io/overlays/mntn/mntn-ptv-overlay/)
- [Mntn Reporting Overlay](https://apis.io/overlays/mntn/mntn-reporting-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Mntn Authentication](https://apis.io/security/mntn/mntn-authentication/)
- [Mntn Domain Security](https://apis.io/security/mntn/mntn-domain-security/)
- [Mntn Trust Center](https://apis.io/security/mntn/mntn-trust-center/)
- [Mntn Vulnerability Disclosure](https://apis.io/security/mntn/mntn-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Mntn Plans Pricing](https://apis.io/plans/mntn/mntn-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Mntn Rate Limits](https://apis.io/rate-limits/mntn/mntn-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://mountain.com/)
- [Documentation](https://help.mountain.com/en/collections/13086998-apis)
- [Getting started](https://help.mountain.com/en/articles/6511970-access-your-reporting-api-key)
- [API reference](https://api.mountain.com/docs)
- [Sign up](https://auth.mountain.com/sign-up)
- [Support](https://help.mountain.com/en/)
- [Changelog](https://help.mountain.com/en/collections/16047071-track-releases)
- [Blog](https://mountain.com/blog/)
- [Terms of service](https://mountain.com/terms-and-conditions/)
- [Privacy policy](https://mountain.com/privacy-policy/)

## API Evangelist rating

**Kin Score 48.6** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 78.6 |
| Contract quality | 54.0 |
| Contract governance | 4.5 |
| Operational transparency | 34.2 |
| Developer ergonomics | 49.4 |
| Access clarity | 50.0 |

**Agent readiness 31.8** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | partial |
| Operation Distinctiveness | distinct |

**Regulatory alignment 35.3** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/mntn/](https://apis.io/providers/mntn/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for MNTN: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/mntn/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
