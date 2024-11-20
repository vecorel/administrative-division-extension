# Administrative Division Extension Specification

- **Title:** Administrative Division
- **Identifier:** <https://fiboa.github.io/administrative-division-extension/v0.1.0/schema.yaml>
- **Property Name Prefix:** admin
- **Extension Maturity Classification:** Proposal
- **Owner**: @m-mohr

This document explains the Administrative Division Extension to the
[Field Boundaries for Agriculture (fiboa) Specification](https://github.com/fiboa/specification).

It defines administrative divisions on the country and subdivision level based on ISO-3166.

- Examples:
  - [GeoJSON](examples/geojson/)
  - [GeoParquet](examples/geoparquet/)
- [Schema](schema/schema.yaml)
- [Changelog](./CHANGELOG.md)

## Properties

The properties in the table below can be used in these parts of fiboa documents:

- [x] Collection
- [x] Feature Properties

| Property Name          | Type   | Description |
| ---------------------- | ------ | ----------- |
| admin:country_code     | string | **REQUIRED.** ISO 3166-1 alpha-2 country code (aka admin0). Two-letter country code for the country that contains the field, e.g. DE for Germany. Can be found at <https://www.iso.org/obp/ui/#search> under the Alpha-2 code column. |
| admin:subdivision_code | string | ISO 3166-2 codes for identifying the principal subdivisions (e.g., provinces or states) of a country (aka admin1) that contains the field. The feature only contains the second part of the ISO 3166-2 code, to reduce redundancy. |

## Contributing

See the [contributing guideline](CONTRIBUTING.md) for more details.
