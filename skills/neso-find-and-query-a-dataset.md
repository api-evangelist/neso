---
name: Find and query a NESO Data Portal dataset
description: Locate one of the 128 open NESO datasets, resolve it to a queryable data file, and pull only the rows you need from the datastore without downloading the whole file.
api: NESO Data Portal API
base_url: https://api.neso.energy/api/3/action
spec: none published - operations below are real HTTP paths verified by anonymous probe on 2026-07-27
operations:
  - GET /api/3/action/organization_list
  - GET /api/3/action/package_search
  - GET /api/3/action/package_show
  - GET /api/3/action/resource_show
  - GET /api/3/action/datastore_search
generated: '2026-07-27'
method: generated
source: https://www.neso.energy/data-portal/api-guidance
---

# Find and query a NESO Data Portal dataset

The NESO Data Portal is a CKAN 2.8.7 catalogue. Nothing here needs a key, an account or a
referrer — send a plain HTTPS GET. Vocabulary matters: CKAN's *Organization* is the portal's
**Data Group**, *Package* is a **Dataset**, and *Resource* is a **Data File**.

## Before you start

- Base URL: `https://api.neso.energy/api/3/action`
- Auth: none. Do not send an Authorization header.
- Rate: at most **1 request/second** to these actions, and **2 requests/minute** to any
  `datastore_*` action. NESO blocks IP addresses that degrade the service.
- Licence: NESO Open Data Licence v1.0. Attribute redistributed data as
  "Supported by National Energy SO Open Data".

## Steps

1. **Narrow by data group (optional).** `GET /organization_list` returns the 16 groups:
   `balancing`, `demand`, `dfs`, `generation`, `interconnectors`, `connection-registers`,
   `constraint-management`, `network-charges`, `future-energy-scenarios`, `ancillary-services`,
   `electricity-market-reform`, `strategic-energy-planning`, `system`, `trade-data`,
   `carbon-intensity1`, `plans-reports-analysis`.

2. **Search for the dataset.** `GET /package_search?q=BSUOS&rows=10`. The response is the CKAN
   envelope: `{"success": true, "result": {"count": N, "results": [...]}}`. `q` accepts Solr
   syntax, so field queries and `facet.field=["license_id"]` work. Page with `rows` and `start`.

3. **Open the dataset.** `GET /package_show?id={name-or-uuid}` — for example
   `?id=historic-demand-data`. Read `result.resources[]`: each entry is a data file with `id`,
   `name`, `format`, `last_modified` and `datastore_active`. **Only resources with
   `datastore_active: true` can be queried row-by-row.**

4. **Check whether it has changed before you fetch.** `GET /resource_show?id={resource_id}` and
   compare `last_modified` to what you already hold. NESO documents this explicitly as the
   alternative to polling the file — do this instead of re-downloading on a schedule.

5. **Pull just the rows you need.**
   `GET /datastore_search?resource_id={resource_id}&limit=100&offset=0` returns
   `result.fields[]` (the typed column schema), `result.records[]` and `result.total`. Add
   exact-match filters with `filters={"SETT_DATE":"2020-04-01T00:00:00"}`. Follow
   `result._links.next` to page forward. Stay inside 2 requests/minute here.

## Errors you will actually see

- `404` with `{"success": false, "error": {"message": "Not found", "__type": "Not Found Error"}}`
  — the dataset or resource id does not exist. Resolve it with `package_search` first.
- `400` with the bare JSON string `"Bad request - Action name not known: <action>"` — you used an
  action this portal does not support. Note this one falls **outside** the success/error envelope.
- Always branch on `success` before reading `result`.

## Do not

- Do not attempt writes. Every supported action is a read; there is no public write surface and
  therefore no idempotency key to send.
- Do not download a whole data file when a `datastore_search` filter or a SQL `WHERE` clause
  would do — that is the specific behaviour NESO asks consumers to avoid.
