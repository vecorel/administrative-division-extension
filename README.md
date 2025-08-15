# Administrative Division Extension Specification

- **Title:** Administrative Division
- **Identifier:** <https://vecorel.github.io/administrative-division-extension/v0.1.0/schema.yaml>
- **Property Name Prefix:** admin
- **Extension Maturity Classification:** Proposal
- **Owner**: @m-mohr

This document explains the Administrative Division Extension to the
[Vecorel specification](https://github.com/vecorel/specification).

It defines administrative divisions on the country and subdivision level based on
[ISO-3166](https://www.iso.org/iso-3166-country-codes.html).

- Examples:
  - [GeoJSON](examples/geojson/)
  - [GeoParquet](examples/geoparquet/)
- [Schema](schema/schema.yaml)
- [Changelog](./CHANGELOG.md)

## Properties

| Property Name          | Type   | Description |
| ---------------------- | ------ | ----------- |
| admin:country_code     | string | **REQUIRED.** ISO 3166-1 alpha-2 country code (aka admin0). Two-letter country code for the country that contains the field. |
| admin:subdivision_code | string | ISO 3166-2 codes for identifying the principal subdivisions (e.g., provinces or states) of a country (aka admin1) that contains the field. The feature only contains the second part of the ISO 3166-2 code to reduce redundancy. |

The codes can be found at:

- Search for country codes at <https://www.iso.org/obp/ui/#search> via the Alpha-2 code column.
- The subdivision codes can then be accessed at <https://www.iso.org/obp/ui/#iso:pub:PUB500001:en>

Please note that for some countries subdivision codes are not available.

### Examples

1. California, USA:
   `admin:country_code` = `US` and `admin:subdivision_code` = `CA`
2. Bavaria, Germany:
   `admin:country_code` = `DE` and `admin:subdivision_code` = `BY`

## Contributing

See the [contributing guideline](CONTRIBUTING.md) for more details.
