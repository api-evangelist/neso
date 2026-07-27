# National Energy System Operator (NESO) (neso)

The National Energy System Operator (NESO) is Great Britain's publicly owned, operationally independent electricity system operator and whole-energy system planner, created on 1 October 2024 when the UK government purchased National Grid Electricity System Operator Limited and folded in gas system planning from National Gas Transmission. NESO balances the GB electricity system in real time, runs the connections queue, publishes Future Energy Scenarios, and sits at the centre of the value chain between generators, interconnectors, transmission and distribution networks, and suppliers. Its API posture is the sector's classic split, and NESO lands firmly on the open side of that split. Market and system data is genuinely open and anonymous — the NESO Data Portal exposes 128 datasets over a public CKAN API at api.neso.energy with no key, no account and no application, all under the permissive NESO Open Data Licence, and the Carbon Intensity API for Great Britain that NESO develops is one of the most openly consumed public energy APIs in the world. There is no consumer data surface at all, and none is expected, because NESO holds no retail customer relationships, Britain has no consumer energy data-portability mandate comparable to the Australian Consumer Data Right, and smart-meter consumption data travels through the licensed Smart DCC monopoly rather than through the system operator. NESO's open-data obligation comes from Ofgem's Data Best Practice Guidance embedded in its RIIO-2 licence, and unlike most of the sector that obligation is visibly implemented rather than merely claimed.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/neso/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/neso/refs/heads/main/apis.yml)

## Tags

- Energy
- United Kingdom
- Electricity
- Energy Markets
- Grid
- Open Data
- Carbon
- Renewables
- Gas
- Demand Response

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

### NESO Data Portal API

The public CKAN 2.8.7 API behind the NESO Data Portal, serving 128 open datasets covering GB electricity demand, generation, balancing, ancillary services, constraints, interconnectors, connection registers, network charges, the Demand Flexibility Service, and Future Energy Scenarios. Anonymous and unauthenticated — `package_list`, `package_search`, `package_show`, `resource_search`, `resource_show`, `datastore_search` and `datastore_search_sql` (PostgreSQL) all answer 200 with no key. All 128 datasets carry the NESO Open Data Licence. NESO asks for a maximum of one request per second against the CKAN API and two requests per minute against the datastore.

- **Human URL:** [https://www.neso.energy/data-portal/api-guidance](https://www.neso.energy/data-portal/api-guidance)
- **Base URL:** `https://api.neso.energy/api/3/action`

#### Tags

- Open Data
- Energy Markets
- Electricity
- Demand
- CKAN

#### Properties

- [Documentation](https://www.neso.energy/data-portal/api-guidance)
- [Documentation](https://www.neso.energy/data-portal)
- [Licensing](https://www.neso.energy/data-portal/neso-open-licence)

### Carbon Intensity API

The official Carbon Intensity API for Great Britain, developed by NESO, giving national and regional carbon intensity of GB electricity — actual, forecast up to 96+ hours ahead, half-hourly generation mix, per-fuel emission factors, and regional breakdowns by DNO region, region id, or postcode. Fully anonymous REST/JSON over HTTPS with no API key and no registration; documented as a Slate reference site rather than as a downloadable OpenAPI contract.

- **Human URL:** [https://carbon-intensity.github.io/api-definitions/](https://carbon-intensity.github.io/api-definitions/)
- **Base URL:** `https://api.carbonintensity.org.uk`

#### Tags

- Carbon
- Electricity
- Renewables
- Grid
- Open Data

#### Properties

- [API Reference](https://carbon-intensity.github.io/api-definitions/)
- [Documentation](https://carbonintensity.org.uk/)
- [GitHub Organization](https://github.com/carbon-intensity)
- [Source Code](https://github.com/carbon-intensity/api-definitions)

## Common Properties

- [Website](https://www.neso.energy/)
- [Documentation](https://www.neso.energy/data-portal/api-guidance)
- [Portal](https://www.neso.energy/data-portal)
- [Licensing](https://www.neso.energy/data-portal/neso-open-licence)
- [LinkedIn](https://www.linkedin.com/company/neso-energy)
- [Blog RSS](https://www.neso.energy/rss.xml)
- [News](https://www.neso.energy/news)

## Maintainers

- Kin Lane — kin@apievangelist.com

## Artifacts

Enrichment round 2026-07-27. NESO publishes no OpenAPI, Swagger, GraphQL, AsyncAPI or gRPC
contract on any host — re-probed across `api.neso.energy`, `api.carbonintensity.org.uk` and
`carbon-intensity.github.io` — so no spec was harvested and none was authored. Everything below is
either a document fetched from NESO, a live-probe result, or an honest derivation.

- [`authentication/neso-authentication.yml`](authentication/neso-authentication.yml) — both APIs fully anonymous: no key, OAuth, OIDC or mTLS.
- [`conventions/neso-conventions.yml`](conventions/neso-conventions.yml) — pagination, Solr/SQL filtering, error envelopes, CORS, tracing, change detection. Idempotency is recorded as *absent* (both surfaces are read-only).
- [`rate-limits/neso-rate-limits.yml`](rate-limits/neso-rate-limits.yml) — 1 req/sec CKAN API, 2 req/min Datastore API; advisory, enforced by IP blocking, not signalled in headers.
- [`errors/neso-error-codes.yml`](errors/neso-error-codes.yml) — error bodies observed verbatim from live probes. Not RFC 9457.
- [`lifecycle/neso-lifecycle.yml`](lifecycle/neso-lifecycle.yml) — CKAN Action API 3; Carbon Intensity unversioned and part-beta. No status page, SLA or deprecation policy exists.
- [`conformance/neso-conformance.yml`](conformance/neso-conformance.yml) — CKAN, ISO 8601, CORS, RFC 9116, OGL v3.0, Ofgem Data Best Practice. No IEC CIM, IEEE 2030.5, Green Button, OpenADR or CDR.
- [`data-model/neso-data-model.yml`](data-model/neso-data-model.yml) — Organization/Package/Resource/DatastoreRecord and IntensityPeriod/Region/GenerationMix.
- [`well-known/neso-well-known.yml`](well-known/neso-well-known.yml) + [`well-known/neso-security.txt`](well-known/neso-security.txt) — a real RFC 9116 security.txt on the corporate host.
- [`security/neso-vulnerability-disclosure.yml`](security/neso-vulnerability-disclosure.yml) — published VDP via HackerOne; 5-working-day response, no monetary rewards.
- [`security/neso-domain-security.yml`](security/neso-domain-security.yml) — TLS 1.3 everywhere; DNSSEC + SPF + DMARC reject on `neso.energy`, none of the three on `carbonintensity.org.uk`.
- [`packages/neso-packages.yml`](packages/neso-packages.yml) — no first-party SDK on any registry; three third-party libraries recorded.
- [`mcp/neso-mcp.yml`](mcp/neso-mcp.yml) — 17 candidate tools over the two APIs. NESO operates no MCP server.
- [`skills/_index.yml`](skills/_index.yml) — four agent skills grounded in probe-verified HTTP operations.
- [`llms/neso-llms.txt`](llms/neso-llms.txt) — generated; NESO publishes no `llms.txt`.
