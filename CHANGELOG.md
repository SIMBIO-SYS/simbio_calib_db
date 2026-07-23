# Changelog

All notable changes to the SIMBIO-SYS calibration database are documented in
this file.

## Unreleased

### Changed

- Move the active HRIC response product out of the duplicated
  `data/response/hric/` directory and update its CSV reference.
- Replace the obsolete HRIC XML label with a PDS4 `.lblx` ancillary label
  whose core elements use explicit `pds:` prefixes.
- Replace the former 32 MiB two-plane HRIC payload with a 2048 × 2048
  float32 response matrix of 16 MiB.
- Add channel metadata and prepare the HRIC and STC manifests for database
  version `2.0`, release `2026-07-31`.
- Rebuild the VIHI response payload as a 265 × 256 big-endian float64 zero
  matrix and align its CSV shape, label file size, record count and checksum
  (`SIMCAL-015`).
- Prepare the VIHI manifest as version `2.0`, release `2026-07-31`.

### Verified

- The HRIC payload MD5 is `2c7ab85a893283e98c931e9511add182`.
- The HRIC label is well-formed XML and describes the payload as a two-axis
  ancillary image with dimensionless values.
- The VIHI response payload is 542,720 bytes, loads through CalibDBReader as a
  `(265, 256)` big-endian float64 array, and has MD5
  `cd2be5f11c8d422f076328cf81b914eb`.

### Documentation

- Record that the Calibrator-wide SPICE kernel folder and `MetakernelInfo`
  integration does not change database indexes, manifests, or payload paths.
- Record the VIHI payload/CSV/PDS4 metadata alignment as resolved
  (`SIMCAL-015`).
- Record the incorrect HRIC housekeeping identity retained in the VIHI ITF
  label as `SIMCAL-018`; no label correction is included.

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
