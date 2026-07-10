# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- `_name` variants for all levels plus `admin_continent`, and new levels
  `admin_level2` through `admin_level4` (name + code each), so any boundary
  dataset (Overture, GAUL, CODs, LSIB, GADM, geoBoundaries) maps into one
  scheme. [#2](https://github.com/vecorel/administrative-division-extension/issues/2)
- Collection-level `admin_source_name`, `admin_source_url`,
  `admin_source_version`, `admin_source_method`, and
  `admin_source_processing` properties identifying the boundary dataset
  used and the processing applied. [#3](https://github.com/vecorel/administrative-division-extension/issues/3)

### Changed

- **Breaking:** Renamed `admin:country_code` to `admin_country_code` and
  `admin:subdivision_code` to `admin_subdivision_code`. A colon is an illegal
  character in Windows file paths, which broke hive-style partitioning on
  these columns, and partition-aware readers surfaced URL-encoded ghost
  columns. [#4](https://github.com/vecorel/administrative-division-extension/issues/4)
- `admin_country_code` is no longer unconditionally required: at least one
  of `admin_country_code` or `admin_country_name` must be provided
  (documented requirement; not schema-enforceable in SDL).
- `admin_subdivision_code` no longer enforces the ISO 3166-2 pattern, so
  source-native subdivision codes are valid.

### Deprecated

- ...

### Removed

- ...

### Fixed

- ...

## [v0.1.0] - 2025-08-15

- First release

This extension is based on the [fiboa administrative-division extension v0.1.0](https://github.com/fiboa/administrative-division-extension/).

[Unreleased]: <https://github.com/vecorel/administrative-division-extension/compare/v0.1.0...main>
[v0.1.0]: <https://github.com/vecorel/administrative-division-extension/tree/v0.1.0>
