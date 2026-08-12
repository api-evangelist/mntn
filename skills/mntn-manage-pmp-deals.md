---
name: mntn-manage-pmp-deals
description: >-
  Manage MNTN private marketplace inventory — register supply partners, create PMP deals, organise
  them into deal groups against a channel, and link a deal group to a campaign so it delivers against
  that inventory. Use when asked to wire up or audit MNTN PMP deals programmatically.
api: MNTN Performance TV (PTV) API
spec: openapi/mntn-ptv-pmp-deals-openapi.yml
base_url: https://api.mountain.com/ptv
operations:
  - pmp.channels.list
  - pmp.partners.list
  - pmp.partners.create
  - pmp.partners.get
  - pmp.partners.update
  - pmp.partners.delete
  - pmp.deals.list
  - pmp.deals.create
  - pmp.deals.get
  - pmp.deals.update
  - pmp.deals.delete
  - pmp.deals.bulkDeactivate
  - pmp.deals.listGroups
  - pmp.dealGroups.list
  - pmp.dealGroups.create
  - pmp.dealGroups.get
  - pmp.dealGroups.update
  - pmp.dealGroups.delete
  - pmp.dealGroups.bulkDelete
  - pmp.dealGroups.addDeal
  - pmp.dealGroups.removeDeal
  - pmp.campaigns.listDealGroups
  - pmp.campaigns.linkDealGroup
  - pmp.campaigns.unlinkDealGroup
generated: '2026-08-12'
method: generated
source: openapi/_original/mntn-ptv-openapi.json
---

# Manage private marketplace deals on MNTN

## The object graph

```
Channel ──< DealGroup >──< Deal >── Partner
                │
             Campaign
```

- A **Partner** is the supply-side counterparty (`partnerId`, plus `dspPartnerId` and `publisherId`
  on a deal).
- A **Deal** belongs to a partner and carries the negotiated terms.
- A **DealGroup** belongs to a **Channel** (`channelId`) and holds many deals. Deals and deal groups
  are **many-to-many** — a deal can sit in several groups.
- A **Campaign** links to deal groups, not to deals directly.

Read `data-model/mntn-data-model.yml` for the full derivation.

## Steps

1. **List the channels.** `GET /api/v1/channels` (`pmp.channels.list`). This is a reference
   collection — list-only, no writes. You need a `channelId` before you can create a deal group.

2. **Register or find the partner.** `GET /api/v1/partners` (`pmp.partners.list`, paged with
   `page`/`perPage`, filterable with `search`), then `POST /api/v1/partners`
   (`pmp.partners.create`) if it does not exist. `GET`/`PATCH`/`DELETE /api/v1/partners/{partnerId}`
   for the rest of the lifecycle.

3. **Create the deal.** `POST /api/v1/deals` (`pmp.deals.create`) with `CreateDealBodyDto` —
   `partnerDealId`, `partnerId`, `dspPartnerId`, `publisherId`. Filter existing deals with
   `GET /api/v1/deals?partnershipDealType=…` (`pmp.deals.list`).

4. **Create the deal group.** `POST /api/v1/deal-groups` (`pmp.dealGroups.create`) with the
   `channelId` from step 1.

5. **Put deals in the group.** `PUT /api/v1/deal-groups/{dealGroupId}/deals/{pmpDealId}`
   (`pmp.dealGroups.addDeal`). Remove with the matching `DELETE` (`pmp.dealGroups.removeDeal`).
   To see the reverse side, `GET /api/v1/deals/{pmpDealId}/deal-groups` (`pmp.deals.listGroups`).

6. **Attach to a campaign.** `PUT /api/v1/campaigns/{campaignId}/deal-groups/{dealGroupId}`
   (`pmp.campaigns.linkDealGroup`). Audit with
   `GET /api/v1/campaigns/{campaignId}/deal-groups` (`pmp.campaigns.listDealGroups`) and detach with
   the matching `DELETE` (`pmp.campaigns.unlinkDealGroup`).

## Bulk operations — handle with care

- `POST /api/v1/deals/bulk-deactivate` (`pmp.deals.bulkDeactivate`) takes `pmpDealIds[]`.
- `POST /api/v1/deal-groups/bulk-delete` (`pmp.dealGroups.bulkDelete`) takes `dealGroupIds[]`.

Both are `POST` with no idempotency key and both affect many objects at once. Enumerate with the
matching list operation and log the id set **before** you call either. A deal group deleted here is
also unlinked from every campaign that referenced it.

## Ordering rules

- A deal group needs a `channelId` that exists — create groups after reading `pmp.channels.list`.
- A deal needs a `partnerId` that exists — create the partner first.
- Deleting a partner while deals reference it, or a deal group while campaigns link it, is not
  documented as safe. Unlink downward before deleting upward.

## Errors

The Performance TV API declares only a bare `default` response on most operations and returns
`{"error": "<message>"}` rather than RFC 9457. Treat any non-2xx on the linking `PUT`s as unknown
state and re-read with `pmp.campaigns.listDealGroups` or `pmp.deals.listGroups` before retrying.
