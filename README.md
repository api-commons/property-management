# Property Management

The common property vocabulary -- properties and the units in them, the leases that let them, the tenants who live there, the work orders that maintain them and the listings that market them -- distilled from the APIs that already exist. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Property Management, PropTech, Rentals, Real Estate, Commercial Real Estate, Multifamily in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| property | 53 | 49 | address 67%, id 67% | properties 27, property 21, buildings 12, communities 2 |
| unit | 9 | 8 | id 62%, number 62%, property_id 62% | units 7, spaces 1, apartments 1 |
| lease | 9 | 9 | id 89%, start_date 78%, end_date 67%, property_id 67%, status 67%, rent 56%, tenant_ids 56% | leases 6, tenancies 3, lease 1 |
| tenant | 8 | 8 | id 88%, phone 50% | tenants 8, residents 1 |
| work_order | 5 | 5 | id 100%, status 80%, description 60%, property_id 60% | work-orders 1, workorders 1, work_orders 1, maintenance-requests 1 |
| listing | 19 | 17 | id 71% | listings 16, listing 6 |

Cohort: 143 providers with harvested OpenAPI, 62 crosswalks. Of the providers exposing a property: 40% create, 81% list, 72% fetch, 21% update, 17% delete. Generated 2026-11-11 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- A lease agrees completely: start and end dates, status, rent, the property and the tenants are all core across the providers that expose one.
- A property agrees only on id and address. Type, size, year built and unit count are each modelled by too few providers to share -- residential management, commercial portfolios and MLS listings describe a building differently.
- Listings agree on nothing but an id, although the industry has a listing standard (RESO): the listing APIs published here name their fields differently enough that the aliases cannot reconcile them. Worth a RESO-specific pass, as Open Banking and CDR got.
- Units, tenants and work orders are thin (5-9 providers): most property-management systems keep their APIs to partners.
- Measured against the RESO Data Dictionary 2.0, the standard covers the listing and property core and defines about 600 more fields that no provider in the cohort publishes as OpenAPI -- the clearest measure in the commons of how much richer a standard is than what reaches an API description.

## Measured against the standard

The census reads what providers publish. Where the domain has a standard, the standard's own
definition is read as one more voice: how much of the measured core it defines, and what it
defines that no provider in the cohort publishes. Crosswalks are in [standards/](standards).

| Standard | Concept | Core defined by the standard | Fields only the standard has |
| --- | --- | ---: | ---: |
| [reso-dd-2.0](https://github.com/RESOStandards/reso-tools/blob/main/reso-certification/tests/fixtures/commander/RESODataDictionary-2.0.xml) | `listing` | 100% | 615 |
| [reso-dd-2.0](https://github.com/RESOStandards/reso-tools/blob/main/reso-certification/tests/fixtures/commander/RESODataDictionary-2.0.xml) | `property` | 100% | 601 |

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- Real Estate is in the cohort, so listings are a concept here alongside the management resources.
- A tenant counts only at a provider that also exposes leases (`requires: [lease]`): tenants are an organization in the identity context.
- Buildings and communities are properties; apartments and spaces beneath a building are units; tenancies and rental agreements are leases.
- Work orders cover maintenance and service requests: one resource, named for the job.
- Duplicates merged: Alto into Zoopla, Realtor.ca into CREA.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
