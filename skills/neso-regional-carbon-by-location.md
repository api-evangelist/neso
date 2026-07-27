---
name: Get regional GB carbon intensity by postcode or DNO region
description: Resolve a Great Britain location to its distribution network region and read that region's current and forecast carbon intensity and generation mix.
api: Carbon Intensity API
base_url: https://api.carbonintensity.org.uk
spec: none published - operations below are documented paths verified by anonymous probe on 2026-07-27
operations:
  - GET /regional
  - GET /regional/postcode/{postcode}
  - GET /regional/regionid/{regionid}
  - GET /regional/england
  - GET /regional/scotland
  - GET /regional/wales
  - GET /regional/intensity/{from}/fw24h/postcode/{postcode}
  - GET /regional/intensity/{from}/fw48h/regionid/{regionid}
  - GET /regional/intensity/{from}/{to}/postcode/{postcode}
generated: '2026-07-27'
method: generated
source: https://carbon-intensity.github.io/api-definitions/
---

# Get regional GB carbon intensity by postcode or DNO region

Great Britain's grid is not uniform — North Scotland can be running 99%+ wind while another
distribution region is gas-heavy in the same half hour. The regional endpoints break intensity and
generation mix down by **DNO licence area**. Anonymous, CORS-open, no key.

## Before you start

- Base URL: `https://api.carbonintensity.org.uk`
- A region entry looks like:
  `{"regionid": 1, "dnoregion": "Scottish Hydro Electric Power Distribution", "shortname": "North Scotland", "intensity": {...}, "generationmix": [...]}`
- **Regional entries carry `forecast` and `index` only — there is no `actual` at regional level.**
  Do not report a regional number as measured.
- `{postcode}` is the **outward** code only (the part before the space, e.g. `RG10`), not a full postcode.
- This whole section is marked **beta** in NESO's published reference.

## Steps

1. **Snapshot everything.** `GET /regional` returns the current half hour for every DNO region at
   once — one request, all regions. Prefer this over looping region ids.

2. **Resolve a location.**
   - By postcode: `GET /regional/postcode/{postcode}`
   - By region id: `GET /regional/regionid/{regionid}`
   - By nation: `GET /regional/england`, `/regional/scotland`, `/regional/wales`

3. **Forecast a location forward.**
   `GET /regional/intensity/{from}/fw24h/postcode/{postcode}` or `.../fw48h/regionid/{regionid}`,
   where `{from}` is `YYYY-MM-DDThh:mmZ`. Use `/pt24h/...` for the previous 24 hours and
   `/{from}/{to}/...` for an arbitrary window.

4. **Read the mix.** Each period carries `generationmix[]` as `{fuel, perc}` over biomass, coal,
   imports, gas, nuclear, other, hydro, solar and wind. Percentages are of that region's
   generation in that half hour.

5. **Pick a slot.** For "when should this run here?", take the forward array from step 3 and select
   the minimum `intensity.forecast`, then convert back to the `from` timestamp of that period.

## Errors you will actually see

- `400` `{"error": {"code": "400 Bad Request", "message": "Please enter a valid path e.g. /intensity/"}}`
  — usually a full postcode with the inward code attached, or a missing path segment.
- An unrecognised outward code returns an empty or unmatched region rather than a hard failure —
  check that `data[].regions[]` is non-empty before indexing into it.

## Do not

- Do not cache a region id ↔ postcode mapping as permanent; resolve through the API.
- Do not mix national and regional numbers in the same figure. National has `actual`, regional
  does not, and they are computed over different footprints.
