# SIMBIO-SYS calibration database handoff

Updated: 2026-07-16

## Repository role

This repository is the shared calibration-data source for HRIC, STC, and VIHI.
It contains metadata, the CSV index, and the files consumed by
`CalibDBReader`.

## Current contract

- `manifest.json` contains `instrument: SIMBIO-SYS` and `version: 1.1`.
- `calib_db.csv` is the database index.
- CSV `File` values are relative to the repository root and include `data/`.
- `version.yml` is obsolete and has been removed.

## Current indexed records

The CSV contains five records:

- one VIHI instrument-transfer-function matrix;
- VIHI bad-pixel and dead-pixel matrices;
- one STC instrument-transfer-function matrix;
- one HRIC instrument-transfer-function matrix.

All five referenced files currently exist.

## Verification

Use the local `CalibDBReader` checkout:

```bash
uv run calibDB version ../simbio_calib_db
uv run calibDB dbdisplay ../simbio_calib_db/calib_db.csv --check
```

## Next work

- Expand the CSV index to cover the available cosmetic, distortion, dark
  current, and response products.
- Add automated schema and referential-integrity checks to this repository.
- Remove untracked operating-system metadata and temporary editor artifacts
  from the data tree where safe.
