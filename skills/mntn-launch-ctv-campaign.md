---
name: mntn-launch-ctv-campaign
description: >-
  Create, configure and launch a Connected TV campaign on the MNTN Performance TV API — resolve the
  advertiser, validate budget/goal/objective against MNTN's reference vocabularies, attach an
  audience and creatives, then launch and confirm. Use when asked to stand up or start an MNTN CTV
  campaign programmatically.
api: MNTN Performance TV (PTV) API
spec: openapi/mntn-ptv-campaigns-openapi.yml
base_url: https://api.mountain.com/ptv
operations:
  - advertisers.list
  - advertisers.get
  - reference.listBudgetTypes
  - reference.listGoalTypes
  - reference.listCampaignObjectives
  - reference.listCampaignStatuses
  - audiences.list
  - creatives.list
  - campaigns.create
  - campaigns.get
  - campaigns.update
  - campaigns.launch
  - campaigns.pause
generated: '2026-08-12'
method: generated
source: openapi/_original/mntn-ptv-openapi.json
---

# Launch a CTV campaign on MNTN

## Before you start

Authenticate with `X-API-Key: <key>` or `Authorization: Bearer <JWT>`. The key comes from the MNTN
platform UI under Account Settings → API. Everything below is scoped to one advertiser.

**This skill spends money.** `campaigns.create` creates a billable object and `campaigns.launch`
starts delivery against real inventory. MNTN publishes **no idempotency key** on any of these
operations, so a blind retry after a timeout can create or launch twice. Every retry step below is
written as *read first, then retry*.

## Steps

1. **Resolve the advertiser.** `GET /api/v1/advertisers` (`advertisers.list`, paged with
   `page`/`perPage`, filterable with `search`). Confirm with `GET /api/v1/advertisers/{id}`
   (`advertisers.get`). Hold the advertiser id — nearly every later call is scoped by it.

2. **Read the controlled vocabularies before you write anything.** MNTN publishes the legal values,
   so never guess them:
   - `GET /api/v1/reference/budget/types` (`reference.listBudgetTypes`)
   - `GET /api/v1/reference/budget/goals` (`reference.listGoalTypes`)
   - `GET /api/v1/reference/campaign/objectives` (`reference.listCampaignObjectives`)
   - `GET /api/v1/reference/campaign/statuses` (`reference.listCampaignStatuses`)

3. **Pick the audience.** `GET /api/v1/audiences?advertiserId=…` (`audiences.list`). If no audience
   fits, `POST /api/v1/campaigns/{id}/recommended-audience`
   (`CampaignsController_createRecommendedAudience_v1`) asks MNTN to build one — note it runs
   *after* the campaign exists, so it belongs in step 6, not here.

4. **Confirm creatives exist.** `GET /api/v1/creatives` (`creatives.list`). A campaign with no
   approved creative cannot deliver. Creatives have no update operation — `creatives.create` and
   `creatives.delete` only.

5. **Create the campaign.** `POST /api/v1/campaigns` (`campaigns.create`) with the
   `CreateCampaignDto` body: `advertiserId`, `audienceIds`, and the budget/goal/objective values you
   validated in step 2. Keep the returned campaign id.
   *If this call times out:* do **not** resubmit. Call `campaigns.list` filtered on the advertiser
   and match your intended campaign name first.

6. **Tune before launching.** `PATCH /api/v1/campaigns/{id}` (`campaigns.update`) for budget and
   audience changes; `GET`/`PATCH /api/v1/campaigns/{id}/flight/{flight_id}` (`flights.get`,
   `flights.patch`) for the delivery window. Note `flight_id` is snake_case — the only such path
   parameter in the API.

7. **Launch.** `POST /api/v1/campaigns/{id}/launch` (`campaigns.launch`). Delivery and spend begin.
   *If this call times out:* call `campaigns.get` and read the status against
   `reference.listCampaignStatuses` before retrying.

8. **Verify.** `GET /api/v1/campaigns/{id}` (`campaigns.get`). Confirm the status is a live value
   from the status vocabulary.

## Stopping and cleaning up

- `POST /api/v1/campaigns/{id}/pause` (`campaigns.pause`) halts delivery. Reversible.
- `DELETE /api/v1/campaigns/{id}` (`campaigns.archive`) archives it. **No published undo path.**

## Error handling

The Performance TV API declares only a bare `default` response on 78 of its 85 operations and returns
an ad-hoc `{"error": "<message>"}` body, not RFC 9457 — so you cannot rely on a `type`, `errorCode`
or `traceId` here (the sibling reporting API on `api3.mountain.com` does return problem+json). Treat a
401 as a bad or missing key, and any non-2xx on a write as *unknown outcome* — re-read the resource
before acting again. See `errors/mntn-problem-types.yml` and `conventions/mntn-conventions.yml`.
