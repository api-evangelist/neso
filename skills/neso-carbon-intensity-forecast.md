---
name: Read and forecast GB carbon intensity
description: Get the current, historic and forecast carbon intensity of Great Britain's electricity, plus the half-hourly generation mix and per-fuel emission factors, to time a workload or a charge for the cleanest half hour.
api: Carbon Intensity API
base_url: https://api.carbonintensity.org.uk
spec: none published - operations below are documented paths verified by anonymous probe on 2026-07-27
operations:
  - GET /intensity
  - GET /intensity/{from}/fw24h
  - GET /intensity/{from}/fw48h
  - GET /intensity/{from}/{to}
  - GET /intensity/date/{date}
  - GET /intensity/date/{date}/{period}
  - GET /intensity/stats/{from}/{to}
  - GET /intensity/factors
  - GET /generation
  - GET /generation/{from}/{to}
generated: '2026-07-27'
method: generated
source: https://carbon-intensity.github.io/api-definitions/
---

# Read and forecast GB carbon intensity

The Carbon Intensity API for Great Britain is developed by NESO and is completely open — no key,
no registration, CORS enabled, so it works from a browser as well as a server. The native grain is
the **GB half-hour settlement period**, 48 per day, timestamped in ISO 8601 UTC.

## Before you start

- Base URL: `https://api.carbonintensity.org.uk`
- Auth: none. Do not send an Authorization header.
- Every response is `{"data": [...]}`; every period carries `from`, `to` and an `intensity` object
  with `forecast`, `actual` and `index`.
- `index` is one of `very low`, `low`, `moderate`, `high`, `very high` — use it for user-facing
  language and the integer `forecast`/`actual` (gCO2/kWh) for arithmetic.
- `actual` is `null` for any period that has not settled yet. Never treat a null actual as zero.
- No pagination exists. The window is in the path.

## Steps

1. **Read right now.** `GET /intensity` returns the current half hour only.

2. **Look ahead.** `GET /intensity/{from}/fw24h` or `/fw48h` where `{from}` is
   `YYYY-MM-DDThh:mmZ`. This returns every half hour forward from that point — 48 or 96 entries.
   To find the cleanest slot for a shiftable workload, take the minimum `intensity.forecast`
   across the returned array.

3. **Look back.** `GET /intensity/{from}/pt24h` for the previous 24 hours, `GET /intensity/{from}/{to}`
   for an arbitrary window, or `GET /intensity/date/{date}` for a whole day.
   `GET /intensity/date/{date}/{period}` takes a settlement period 1–48.

4. **Summarise a period.** `GET /intensity/stats/{from}/{to}` returns max, average and min for the
   window; append `/{block}` to bucket it into n-hour blocks.

5. **Explain the number.** `GET /generation` gives the half-hourly mix as an array of
   `{fuel, perc}` across biomass, coal, imports, gas, nuclear, other, hydro, solar and wind.
   `GET /intensity/factors` gives the gCO2/kWh factor applied to each fuel.

## Errors you will actually see

- `400` with `{"error": {"code": "400 Bad Request", "message": "Please enter a valid path e.g. /intensity/"}}`
  — the path is not one of the documented resources.
- `400` with `"Please enter a valid date in ISO8601 format 'YYYY-MM-DD' i.e. .../intensity/date/2017-08-25"`
  — your `{date}` is malformed. `{from}`/`{to}` want `YYYY-MM-DDThh:mmZ`, `{date}` wants `YYYY-MM-DD`.
- There is no `401`, `403` or `429` on this API.

## Notes

- The Generation Mix and Regional sections of the published reference are marked **beta** — that
  is the only stability signal NESO gives here, and the API carries no version segment.
- Keep the `x-amzn-requestid` response header if you need to report a problem; it is the only
  request identifier this API returns.
