# Changelog

All notable changes to the SIMBIO-SYS calibration database are documented in
this file.

## Unreleased

### Changed

- Replaced `version.yml` with `manifest.json`.
- Set the database manifest to instrument `SIMBIO-SYS`, version `1.1`.
- Updated every CSV `File` value to be relative to the repository root and to
  include the `data/` prefix.
- Documented validation through the `CalibDBReader` CLI.

### Verified

- All five calibration files currently referenced by `calib_db.csv` exist
  under the paths stored in the `File` column.
