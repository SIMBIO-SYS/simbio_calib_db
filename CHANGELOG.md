# Changelog

All notable changes to the SIMBIO-SYS calibration database are documented in
this file.

## Unreleased

## Version 1.2 - 2026-07-20

### Changed

- Split the active calibration indexes and data into `hric/`, `stc/`, and
  `vihi/` database roots.
- Rename instrument indexes to `sim_<instrument>_cal_db_v1.0.csv`.
- Move VIHI response and cosmetic products out of duplicated HRIC/STC paths
  into `vihi/data`.
- Store HRIC and STC response products below their corresponding instrument
  roots.
- Track binary `.dat` calibration payloads through Git LFS, including the
  192 MiB HRIC response matrix.
- Replaced `version.yml` with `manifest.json`.
- Set the root and instrument manifests to version `1.2`.
- Updated every CSV `File` value to be relative to its instrument root and to
  include the `data/` prefix.
- Documented validation through the `CalibDBReader` CLI.

### Verified

- Every file referenced by the three instrument CSV indexes exists relative to
  its instrument database root.
- The STC response matrix is a raw `2048 x 2048` float32 payload of 16 MiB.
- The corrected STC PDS4 ancillary label parses successfully with its declared
  namespace and matching element hierarchy.
