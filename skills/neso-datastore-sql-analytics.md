---
name: Run SQL analytics against a NESO data file
description: Use the CKAN datastore SQL endpoint to aggregate, filter and join NESO's tabular energy data server-side instead of pulling whole files.
api: NESO Data Portal API
base_url: https://api.neso.energy/api/3/action
spec: none published - operations below are real HTTP paths verified by anonymous probe on 2026-07-27
operations:
  - GET /api/3/action/package_show
  - GET /api/3/action/datastore_search
  - GET /api/3/action/datastore_search_sql
generated: '2026-07-27'
method: generated
source: https://www.neso.energy/data-portal/api-guidance
---

# Run SQL analytics against a NESO data file

The NESO datastore is PostgreSQL, exposed read-only over HTTP. This is the most powerful and the
most easily abused part of the portal — NESO caps it at **two requests per minute** for that
reason. Aggregate server-side; do not loop.

## Before you start

- Endpoint: `GET https://api.neso.energy/api/3/action/datastore_search_sql?sql={sql}`
- Auth: none.
- Rate: **2 requests/minute**. Design one query that answers the question rather than many small ones.
- Only `SELECT` works. There is no write path.

## Steps

1. **Resolve the resource id.** `GET /package_show?id={dataset}` and take the `id` of the
   resource you want. Confirm `datastore_active: true`.

2. **Learn the column names.** `GET /datastore_search?resource_id={resource_id}&limit=1` and read
   `result.fields[]` — that array is the typed column schema. Column names in this catalogue are
   frequently capitalised and spaced (`"SETT_DATE"`, `"SETT_PERIOD"`, `"Date"`, `"Constraints"`).

3. **Write the query with double quotes everywhere.** Both the resource id (the table name) and
   every field id must be in double quotes; string literals use single quotes. NESO's own
   published examples:

   ```sql
   SELECT "Date" FROM "b98095a8-310a-4fee-8d51-e20531c49465"
   WHERE "Date" >= '2022-04-01' AND "Date" <= '2022-04-02' LIMIT 500
   ```

   ```sql
   SELECT SUM("Constraints") FROM "7bcd8e25-c148-4cdb-b46f-394f88b92db5"
   WHERE "SETT_PERIOD" BETWEEN '7' AND '14' AND "SETT_DATE" = '2020-04-01T00:00:00'
   ```

4. **URL-encode the whole `sql` parameter.** Spaces, quotes and `*` all need encoding or the
   portal will reject the request.

5. **Always bound the result.** Add a `LIMIT`, or aggregate with `SUM`/`AVG`/`GROUP BY`. Joining
   multiple resources in one statement is supported and is cheaper than two round trips against a
   2-per-minute budget.

6. **Read the envelope.** Success is `{"success": true, "result": {"fields": [...], "records": [...]}}`.

## Errors you will actually see

- A validation error inside the CKAN envelope when a field id or resource id is unquoted or
  misspelled — check step 2 output before blaming the SQL.
- `404 Not Found Error` when the resource id does not exist.
- No `429` exists. Exceeding the rate guidance gets the IP blocked instead, with no warning in
  the response.

## Do not

- Do not use SQL to page through an entire file row by row. If you need the whole file, use the
  resource's download `url` from `package_show` once.
- Do not schedule this query more often than the data actually changes. Check
  `resource_show?id={resource_id}` → `last_modified` first.
