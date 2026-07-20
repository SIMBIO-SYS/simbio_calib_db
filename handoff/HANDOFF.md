# SIMBIO-SYS calibration database handoff

Updated: 2026-07-20

Current database version: `1.2`.

## Repository role

This repository is the shared calibration-data source for HRIC, STC, and VIHI.
It contains metadata, the CSV index, and the files consumed by
`CalibDBReader`.

## Current contract

- Root and instrument `manifest.json` files contain version `1.2`.
- Active database roots are `hric/`, `stc/`, and `vihi/`.
- Each root contains `manifest.json`, `data/`, and a
  `sim_<instrument>_cal_db_v1.0.csv` index.
- CSV `File` values are relative to the instrument root and include `data/`.
- All `.dat` payloads are Git LFS objects. Run `git lfs pull` after checkout;
  a clone without Git LFS contains pointer text rather than calibration data.
- `version.yml` is obsolete and has been removed.

## Current indexed records

The three instrument CSV files contain five records in total:

- one VIHI instrument-transfer-function matrix;
- VIHI bad-pixel and dead-pixel matrices;
- one STC instrument-transfer-function matrix;
- one HRIC instrument-transfer-function matrix.

All five referenced files currently exist. The STC transfer-function payload
is a 16 MiB raw float32 matrix with shape `2048 x 2048`.

## Verification

Use the local `CalibDBReader` checkout:

```bash
uv run calibDB version ../simbio_calib_db/stc
uv run calibDB dbdisplay \
  ../simbio_calib_db/stc/sim_stc_cal_db_v1.0.csv --check
```

Current verification: all three indexes resolve their referenced files, all
four manifests report version `1.2`, the corrected STC label parses, Git LFS
passes `fsck`, and the STC payload is a 2048 × 2048 `float32` matrix.

## Next work

- Expand the CSV index to cover the available cosmetic, distortion, dark
  current, and response products.
- Add automated schema and referential-integrity checks to this repository.
- Remove untracked operating-system metadata and temporary editor artifacts
  from the data tree where safe.
