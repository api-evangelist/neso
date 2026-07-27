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
