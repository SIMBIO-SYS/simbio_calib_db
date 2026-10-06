# SIMBIO-SYS calibration database handoff

Updated: 2026-10-06

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
- Each CSV has a companion PDS4 `.lblx` index label in the current working tree.
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

All five referenced files currently exist and are zero-filled. The STC and active HRIC
transfer-function payloads are 16 MiB raw float32 matrices with shape
`2048 x 2048`, labelled as big-endian `IEEE754MSBSingle`. The HRIC CSV now points directly below `data/response/`; its
`.lblx` label uses explicit `pds:` prefixes and replaces the obsolete XML.
The VIHI ITF payload is a 265 × 256 big-endian float64 zero matrix (542,720
bytes). Its CSV and PDS4 array metadata are aligned, and labelled loading
succeeds (`SIMCAL-015`). The label Identification Area still contains HRIC
housekeeping identity metadata; this separate defect is `SIMCAL-018`.

The VIHI bad- and dead-pixel maps are 256 × 256 raw boolean arrays, stored as
65,536 unsigned bytes each (`0` false, `1` true), in last-index-fastest order
without headers. Both currently contain only zero bytes and have MD5
`fcd6bcb56c1689fcef28b57c22475bad`.

The historical root-level combined CSV and duplicated data tree have been
removed locally. The current data/index/label edits are separate from the
2026-10-06 documentation commit; review them before publication.

## Verification

Use the local `CalibDBReader` checkout:

```bash
uv run calibDB version ../simbio_calib_db/stc
uv run calibDB dbdisplay \
  ../simbio_calib_db/stc/sim_stc_cal_db_v1.0.csv --check
```

Local verification on 2026-10-06: all five indexed files exist, their byte sizes
and MD5 checksums match the payload labels, and all eight active payload/index
labels parse as XML. All three CSVs use CRLF, ten columns and header length 73
bytes. Index-label sizes, checksums, offsets and field counts match the CSVs;
HRIC/STC record counts match, but the VIHI label declares one instead of three.
This is a local consistency check, not PDS4 schema or scientific validation.

## Next work

- Expand the CSV index to cover the available cosmetic, distortion, dark
  current, and response products.
- Add automated schema and referential-integrity checks to this repository.
- Correct the VIHI ITF identification metadata (`SIMCAL-018`) and dead-pixel title.
- Correct the VIHI index label record count from one to three.
- Align the VIHI bad-pixel summary dimensions with its 256 × 256 array.
- Review the VIHI labels' zero scaling factors and replace or approve the
  zero-filled payloads before scientific use.
- Remove untracked operating-system metadata and temporary editor artifacts
  from the data tree where safe.
