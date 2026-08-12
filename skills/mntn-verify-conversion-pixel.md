---
name: mntn-verify-conversion-pixel
description: >-
  Diagnose an MNTN conversion pixel that is not firing or not attributing — check pixel health and
  warnings, list captured verification events, scrape the page to see what is actually deployed,
  listen for live pageviews and conversions, and confirm the attribution windows and query-parameter
  blocklist that shape what gets credited. Use when MNTN conversions are missing or under-reporting.
api: MNTN Performance TV (PTV) API
spec: openapi/mntn-ptv-pixel-openapi.yml
base_url: https://api.mountain.com/ptv
operations:
  - pixel.getHealthOverview
  - pixel.checkWarnings
  - pixel.events
  - pixel.scrapePage
  - pixel.listenPageviews
  - pixel.listenConversions
  - pixel.checkRedisEvents
  - pixel.handleOrderIds
  - pixel.getSignalStrategies
  - pixel.getGa4Ids
  - pixel.storeGa4Ids
  - pixel.updateConversionOptOut
  - attribution.windows.get
  - attribution.windows.update
  - attribution.blacklistQueryParams.get
  - attribution.blacklistQueryParams.update
  - attribution.salesCycle.get
generated: '2026-08-12'
method: generated
source: openapi/_original/mntn-ptv-openapi.json
---

# Diagnose an MNTN conversion pixel

Every operation here is scoped to one advertiser: `/api/v1/advertisers/{advertiserId}/…`.
Authenticate with `X-API-Key` or a bearer JWT.

## 1. Start with the summary

- `POST /api/v1/advertisers/{advertiserId}/pixel/health-overview` (`pixel.getHealthOverview`) — the
  single best first call; it summarises whether the pixel is firing at all.
- `POST /api/v1/advertisers/{advertiserId}/pixel/warnings/check` (`pixel.checkWarnings`) — MNTN's own
  diagnosis of what looks wrong.

If health looks fine but conversions are still missing, the problem is more likely attribution
configuration (section 4) than deployment.

## 2. Confirm what is actually on the page

- `POST /api/v1/advertisers/{advertiserId}/pixel/verification/scrape-page` (`pixel.scrapePage`) —
  MNTN fetches the URL you give it and reports which pixels it can see. This distinguishes "pixel
  never deployed" from "pixel deployed but not firing".
- `GET /api/v1/advertisers/{advertiserId}/pixel/verification/events` (`pixel.events`) — what has
  actually been received.
- `POST /api/v1/advertisers/{advertiserId}/pixel/verification/check-redis-events`
  (`pixel.checkRedisEvents`) — checks the live ingestion buffer.

## 3. Watch it happen live

- `POST /api/v1/advertisers/{advertiserId}/pixel/verification/listen-pageviews`
  (`pixel.listenPageviews`)
- `POST /api/v1/advertisers/{advertiserId}/pixel/verification/listen-conversions`
  (`pixel.listenConversions`)

Open these, then trigger the event in a browser. If a pageview arrives but a conversion does not, the
tracking pixel is installed and the *conversion* pixel is missing or on the wrong page.

- `POST /api/v1/advertisers/{advertiserId}/pixel/verification/handle-order-ids`
  (`pixel.handleOrderIds`) — use when conversions arrive but deduplicate wrongly.

## 4. Check what attribution will actually credit

A correctly firing pixel still under-reports if the windows or blocklist exclude the traffic:

- `GET /api/v1/advertisers/{advertiserId}/attribution/windows` (`attribution.windows.get`), and
  `PATCH` the same path (`attribution.windows.update`) to widen them.
- `GET /api/v1/advertisers/{advertiserId}/attribution/blacklist-query-params`
  (`attribution.blacklistQueryParams.get`), `PUT` to change it
  (`attribution.blacklistQueryParams.update`). Blocked query parameters strip attribution signal —
  an over-broad blocklist is a common silent cause of missing conversions.
- `GET /api/v1/advertisers/{advertiserId}/attribution/sales-cycle` (`attribution.salesCycle.get`) —
  average days from first site visit to conversion. If your sales cycle is longer than your
  attribution window, conversions are being dropped by design, not by a bug.
- `POST /api/v1/advertisers/{advertiserId}/attribution/estimates` (`attribution.estimates.run`) —
  model the effect of a change before making it.

## 5. Signal configuration

- `GET /api/v1/advertisers/{advertiserId}/pixel/signal-strategies` (`pixel.getSignalStrategies`)
- `GET`/`PUT /api/v1/advertisers/{advertiserId}/pixel/ga4-ids` (`pixel.getGa4Ids`,
  `pixel.storeGa4Ids`) — GA4 measurement ids for cross-checking against Google Analytics.
- `PUT /api/v1/advertisers/{advertiserId}/pixel/conversion-opt-out`
  (`pixel.updateConversionOptOut`) — **privacy-affecting**; changes whether conversions are collected
  at all. Read the current value before writing.

## Notes and cautions

- The pixel snippet itself is generated per account in the MNTN UI and is **not version-pinned** —
  there is no package or pinned CDN path, so you cannot audit which build a site is running. See
  `components/mntn-components.yml`.
- Several diagnostic operations here are `POST` but read-only in effect. None of them accepts an
  idempotency key, so repeat them freely for reads and carefully for the three `PUT` writes.
- Error responses on this API are the ad-hoc `{"error": "<message>"}` shape, not RFC 9457.
