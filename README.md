# National Energy System Operator (NESO) (neso)

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
