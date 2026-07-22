# SIMBIO-SYS calibration database handoff

Updated: 2026-07-22

Root database version: `1.2`. HRIC, STC, and VIHI channel manifests are
prepared as version `2.0` for release `2026-07-31`.

## Repository role

This repository is the shared calibration-data source for HRIC, STC, and VIHI.
It contains metadata, the CSV index, and the files consumed by
`CalibDBReader`.

## Current contract

- The root `manifest.json` contains version `1.2`; HRIC, STC, and VIHI contain
  channel-specific version `2.0` metadata.
- Active database roots are `hric/`, `stc/`, and `vihi/`.
- Each root contains `manifest.json`, `data/`, and a
  `sim_<instrument>_cal_db_v1.0.csv` index.
- CSV `File` values are relative to the instrument root and include `data/`.
- All `.dat` payloads are Git LFS objects. Run `git lfs pull` after checkout;
  a clone without Git LFS contains pointer text rather than calibration data.
- `version.yml` is obsolete and has been removed.

SPICE kernels remain outside this repository. The runtime kernel folder and
`MetakernelInfo` introduced in the Calibrator software do not require index or
manifest changes here.

## Current indexed records

The three instrument CSV files contain five records in total:

- one VIHI instrument-transfer-function matrix;
- VIHI bad-pixel and dead-pixel matrices;
- one STC instrument-transfer-function matrix;
- one HRIC instrument-transfer-function matrix.

All five referenced files currently exist. The STC and active HRIC
transfer-function payloads are 16 MiB raw float32 matrices with shape
`2048 x 2048`. The HRIC CSV now points directly below `data/response/`; its
`.lblx` label uses explicit `pds:` prefixes and replaces the obsolete XML.
The VIHI ITF payload is a 256 x 256 float32 zero matrix (262,144 bytes), but
its CSV and label describe incompatible type/shape metadata. Labelled loading
therefore fails and is tracked centrally as `SIMCAL-015`.

## Verification

Use the local `CalibDBReader` checkout:

```bash
uv run calibDB version ../simbio_calib_db/stc
uv run calibDB dbdisplay \
  ../simbio_calib_db/stc/sim_stc_cal_db_v1.0.csv --check
```

Current verification: all three indexes resolve their referenced files; the
root manifest reports `1.2` and all channel manifests report `2.0`; the
corrected STC label parses; and the VIHI payload has MD5
`ec87a838931d4d5d2e94a04644788a55`. Full VIHI labelled-data validation remains
blocked by `SIMCAL-015`.

## Next work

- Expand the CSV index to cover the available cosmetic, distortion, dark
  current, and response products.
- Add automated schema and referential-integrity checks to this repository.
- Align the VIHI ITF CSV and PDS4 metadata with the payload (`SIMCAL-015`).
- Remove untracked operating-system metadata and temporary editor artifacts
  from the data tree where safe.
