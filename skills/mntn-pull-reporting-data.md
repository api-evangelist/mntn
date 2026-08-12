---
name: mntn-pull-reporting-data
description: >-
  Pull MNTN Connected TV performance data through the Reporting API 3.0 — discover available tables
  and columns with /apilist, run a multidimensional query against /apidata, and escalate to the
  asynchronous /batch export when the result is too large. Use when asked to extract MNTN campaign,
  creative, placement or attribution metrics.
api: MNTN Reporting API 3.0
spec: openapi/mntn-reporting-api-openapi.yml
base_url: https://api3.mountain.com
operations:
  - list
  - getData
  - postData
  - submitExport
  - getExportStatus
  - listExports
  - regenerateUrl
generated: '2026-08-12'
method: generated
source: openapi/_original/mntn-reporting-api-openapi.json, openapi/_original/mntn-batch-export-api-openapi.json
---

# Pull reporting data from MNTN

## Before you start

Authentication is the advertiser API key passed as the **`key` query parameter** — not a header.
Get it from the MNTN platform UI under My Account → API. Because the credential rides in the URL it
will appear in proxy logs, browser history and referrer headers: never log the full request URL, and
never put it in a shareable link.

Use `https://api3.mountain.com`. `api.mountain.com` was reporting API 1.0 and **stopped returning
data on 2026-04-01**; that hostname now serves a completely different product (the Performance TV
platform API).

## Steps

1. **Discover the schema.** `GET /apilist?key=<key>&format=json` (`list`) returns the tables and
   columns available to *this* advertiser. Always start here — entitlements differ per account, so a
   column that exists for one advertiser may not exist for another.

2. **Understand the naming.** Columns are fully qualified `table.column` identifiers. Info tables
   carry dimensions, Summary tables carry metrics, and they join on shared keys. Examples:
   `campaigninfo.name`, `graph.day`, `graph.impressions`.

3. **Run the query.** `GET /apidata` (`getData`) for small pulls:

   ```
   GET /apidata?key=<key>&begin=mtd&format=json&sum=graph.day&data=graph.day,graph.impressions
   ```

   Use `POST /apidata` (`postData`) when the filter payload is large. Filters are JSON-encoded, with
   `And` / `Or` / `Not` composition — for example `{"campaigninfo.id":{"in":["200"]}}`.

4. **Choose a format.** `format` accepts `json`, `csv`, `human` and Excel. This is a query parameter,
   not content negotiation — your `Accept` header is ignored.

5. **Handle 413 as a routing instruction, not a failure.** A `413 Payload Too Large` means the query
   exceeds server memory. MNTN's documented remedy is to resubmit the *same* query asynchronously.

## Large exports

1. `POST /batch` (`submitExport`) → `202 Accepted` with a `batchId`. Requires the
   `r2ds.exports.enabled` entitlement; a 403 here means the account is not provisioned for exports.
   **Not idempotent** — on a timeout, call `GET /batch` (`listExports`) and match your query before
   resubmitting, or you will queue a duplicate.
2. `GET /batch/{batchId}` (`getExportStatus`) → poll. When status is `SUCCEEDED` the response
   carries a time-limited signed download URL. No poll interval, no `Retry-After` and no maximum job
   duration are published, so pick a conservative interval yourself.
3. `POST /batch/{batchId}/regenerate-url` (`regenerateUrl`) reissues a signed URL without re-running
   the query. A `409` means the job is in a terminal non-downloadable state and a `410` means the
   result is permanently gone — in both cases resubmit with `POST /batch`.
4. `GET /batch?status=…&limit=…&offset=…` (`listExports`) lists recent jobs.

## Pacing

MNTN publishes no enforced rate limit. The one piece of public guidance is to not repeat the same
query more than **once every two hours per advertiser id**, because the data will not have changed.
`POST /batch` can return `429`, but it carries no `Retry-After` and no `RateLimit-*` headers — you
must back off on your own schedule. See `rate-limits/mntn-rate-limits.yml`.

## Errors

This API returns proper RFC 9457 `application/problem+json` with `type`, `title`, `status`, `detail`,
`instance`, `timestamp`, `errorCode`, an OpenTelemetry `traceId`, and a field-level `errors[]` array
on 400s. Quote the `traceId` when contacting support@mountain.com. Full catalogue in
`errors/mntn-problem-types.yml`.

## Attribution caveat

API 3.0 defaults to **First Touch** attribution; 1.0 and 2.0 defaulted to Last Touch. Numbers pulled
through 3.0 will not reconcile against older extracts unless you account for that, and data before
2023-01-01 is not available in the 3.0 dimensional dataset at all.
