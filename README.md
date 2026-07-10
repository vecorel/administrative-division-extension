# Administrative Division Extension Specification

- **Title:** Administrative Division
- **Identifier:** <https://vecorel.org/administrative-division-extension/v0.2.0/schema.yaml>
- **Property Name Prefix:** admin_
- **Extension Maturity Classification:** Proposal
- **Owner**: @m-mohr

This document explains the Administrative Division Extension to the
[Vecorel specification](https://github.com/vecorel/specification).

It defines administrative divisions from the continent level down to
fourth-level subdivisions. Countries and their principal subdivisions use
[ISO-3166](https://www.iso.org/iso-3166-country-codes.html) codes; deeper
levels carry the codes of whichever boundary dataset was used, which is
identified through the collection-level `admin_source_*` properties.

- Examples:
  - [GeoJSON](examples/geojson/)
  - [GeoParquet](examples/geoparquet/)
- [Schema](schema/schema.yaml)
- [Changelog](./CHANGELOG.md)

## Properties

Each administrative level has a `_name` and a `_code` variant. Populate
whichever your boundary source provides — names, codes, or both.

At least one of `admin_country_code` or `admin_country_name` is
**REQUIRED**. Providing `admin_country_code` is strongly recommended
whenever the source can be mapped to ISO 3166-1. (The schema language
cannot express this either/or constraint, so it is normative here but not
enforced by schema validation.) This requirement may be satisfied at the
collection level (as in the shipped GeoJSON example, where
`admin_country_code` sits on the FeatureCollection) or at the feature
level.

| Property Name          | Type   | Description |
| ---------------------- | ------ | ----------- |
| admin_continent        | string | Continent that contains the feature. Recommended values follow the [FAO GAUL](https://data.apps.fao.org/catalog/dataset/gaul-2024) continent names: `Africa`, `America`, `Asia`, `Europe`, `Oceania` (GAUL 2024 uses the singular `America`, not `Americas`, and has no separate `Antarctica` value — Antarctica and Greenland are coded `--`). |
| admin_country_code     | string | ISO 3166-1 alpha-2 country code (aka admin0). Always ISO — never a source-native code. |
| admin_country_name     | string | Country name as given by the source dataset. |
| admin_subdivision_code | string | Code for the principal subdivision (province, state; aka admin1). Use the second part of the [ISO 3166-2](https://www.iso.org/obp/ui/#iso:pub:PUB500001:en) code (e.g. `CA` for `US-CA`) where defined; otherwise the source dataset's native code. |
| admin_subdivision_name | string | Name of the principal subdivision. |
| admin_level2_code      | string | Source-native code for the second-level unit (county, department, district; aka admin2), e.g. a P-code or GAUL code. |
| admin_level2_name      | string | Name of the second-level unit. |
| admin_level3_code      | string | Source-native code for the third-level unit (aka admin3). |
| admin_level3_name      | string | Name of the third-level unit. |
| admin_level4_code      | string | Source-native code for the fourth-level unit (aka admin4). |
| admin_level4_name      | string | Name of the fourth-level unit. |

Country and subdivision codes can be found at:

- Search for country codes at <https://www.iso.org/obp/ui/#search> via the Alpha-2 code column.
- The subdivision codes can then be accessed at <https://www.iso.org/obp/ui/#iso:pub:PUB500001:en>

Please note that for some countries subdivision codes are not available.

Codes below the subdivision level have no global vocabulary — their meaning
is defined by the boundary dataset identified in the `admin_source_*`
properties below.

## Source Properties

These properties are provided at the collection level only. They record
which boundary dataset the administrative values came from and how they
were assigned — without them, source-native codes cannot be interpreted.

| Property Name           | Type   | Description |
| ----------------------- | ------ | ----------- |
| admin_source_name       | string | Human-readable name of the boundary dataset, e.g. `Overture Maps Divisions`, `FAO GAUL L2`, `fieldmaps.io COD`, `US State Dept LSIB`. |
| admin_source_url        | string | URL of the dataset or its authoritative record. |
| admin_source_version    | string | Release or version identifier of the dataset, e.g. `2026-05-20.0`. |
| admin_source_method     | string | How the values were assigned. Suggested values: `spatial-join` (computed by intersecting geometries with the boundary dataset), `manual`, `provided` (values came with the source data). |
| admin_source_processing | string | Free-text processing lineage, e.g. the exact command used to perform the join. |

## Mapping Boundary Datasets

Any boundary dataset can be joined into this scheme. Mappings for commonly
used sources:

| Extension property     | [Overture Divisions](https://docs.overturemaps.org/guides/divisions/) | [FAO GAUL L2](https://data.apps.fao.org/catalog/dataset/gaul-2024) | [COD / fieldmaps.io](https://fieldmaps.io/) | [geoBoundaries](https://www.geoboundaries.org/) | [LSIB](https://geodata.state.gov/) |
| ---------------------- | --- | --- | --- | --- | --- |
| admin_continent        | — | `continent` | — | — | — |
| admin_country_code     | `country` | via ISO3 → alpha-2 lookup | via ISO3 → alpha-2 lookup | `shapeGroup` via ISO3 → alpha-2 lookup | via country lookup |
| admin_country_name     | `names.primary` | `gaul0_name` | `ADM0_EN` | `shapeName` (ADM0) | `country` |
| admin_subdivision_code | `region` (part after `-`) | `gaul1_code` | `ADM1_PCODE` | `shapeISO` (part after `-`), else `shapeID` (ADM1) | — |
| admin_subdivision_name | `names.primary` | `gaul1_name` | `ADM1_EN` | `shapeName` (ADM1) | — |
| admin_level2_code      | — | `gaul2_code` | `ADM2_PCODE` | `shapeID` (ADM2) | — |
| admin_level2_name      | — | `gaul2_name` | `ADM2_EN` | `shapeName` (ADM2) | — |
| admin_level3_code      | — | — | `ADM3_PCODE` | `shapeID` (ADM3) | — |
| admin_level3_name      | — | — | `ADM3_EN` | `shapeName` (ADM3) | — |
| admin_level4_code      | — | — | `ADM4_PCODE` | `shapeID` (ADM4) | — |
| admin_level4_name      | — | — | `ADM4_EN` | `shapeName` (ADM4) | — |

[GADM](https://gadm.org/) follows the same pattern (per-level names plus
source-native codes) and maps the same way.

GAUL codes (`gaul0_code`, `gaul1_code`, `gaul2_code`) are stored as integers
in the source dataset and must be cast to strings when populated into these
(string-typed) properties.

geoBoundaries publishes each administrative level as a separate file
(ADM0–ADM5); the parenthetical ADM level above indicates which file each
column comes from. Its `shapeISO` field carries full ISO 3166-2 codes at the
ADM1 level (e.g. `LU-EC`) but is typically empty at deeper levels, where the
stable `shapeID` serves as the source-native code.

### Examples

1. California, USA (Overture, ISO codes):
   `admin_country_code` = `US` and `admin_subdivision_code` = `CA`
2. Bavaria, Germany (Overture, ISO codes):
   `admin_country_code` = `DE` and `admin_subdivision_code` = `BY`
3. A GAUL-based collection (names only — valid because `admin_country_name`
   satisfies the country requirement):
   `admin_continent` = `Africa`, `admin_country_name` = `Kenya`,
   `admin_subdivision_name` = `Nakuru`, `admin_level2_name` = `Njoro`,
   with `admin_source_name` = `FAO GAUL L2`.

## Why an underscore instead of a colon?

Most Vecorel extensions prefix properties as `prefix:name`. This extension
uses `admin_` instead: these columns are the most common choice for
[hive-style partitioning](https://duckdb.org/docs/data/partitioning/hive_partitioning),
and a colon is an illegal character in Windows file paths. Partition-aware
readers also URL-encode the colon, surfacing ghost `admin%3A...` columns
(see [#4](https://github.com/vecorel/administrative-division-extension/issues/4)).

## Contributing

See the [contributing guideline](CONTRIBUTING.md) for more details.
