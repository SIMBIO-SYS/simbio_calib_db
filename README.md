# SIMBIO-SYS Calibration Database

This repository contains the calibration database for the SIMBIO-SYS suite on
board ESA's BepiColombo mission.

The root manifest is version `1.2`; the HRIC, STC, and VIHI channel manifests
are prepared as version `2.0` with release date `2026-07-31`.

The repository now exposes one database root per instrument:

```text
hric/
├── manifest.json
├── sim_hric_cal_db_v1.0.csv
└── data/
stc/
├── manifest.json
├── sim_stc_cal_db_v1.0.csv
└── data/
vihi/
├── manifest.json
├── sim_vihi_cal_db_v1.0.csv
└── data/
```

Each instrument database consists of:

- `manifest.json`, containing the database identity and version;
- `sim_<instrument>_cal_db_v1.0.csv`, indexing validity and file metadata;
- `data/`, containing the calibration matrices and associated labels.

Values in the CSV `File` column are relative to the corresponding instrument
directory. They include the `data/` prefix, for example:

```text
data/response/stc/sim_cal_stc_resp_eq_T263_IBR01_v1.0.dat
```

The active HRIC response is
`data/response/sim_cal_hric_resp_eq_IBR01_T268_v1.0.dat`: a 2048 × 2048
little-endian float32 matrix (16 MiB) with a namespaced PDS4 ancillary
`.lblx` label. The former duplicated `data/response/hric/` level is obsolete.

The current VIHI response payload is a 265 × 256 zero matrix stored as
big-endian float64 (542,720 bytes; MD5
`cd2be5f11c8d422f076328cf81b914eb`). Its CSV and PDS4 array metadata are
aligned and labelled loading succeeds (`SIMCAL-015`). The label Identification
Area still carries obsolete HRIC housekeeping identity metadata, tracked as
`SIMCAL-018`.

This convention is shared by `CalibDBReader`, `simCal`, `stcCal`, `hricCal`,
and `vihiCal`. The historical root-level combined index remains for
compatibility, but new configuration points at an instrument subdirectory.

SPICE kernels are intentionally not stored or indexed here. The Calibrator
runtime supplies them through `folders.kernels` and `MetakernelInfo`; this
database remains limited to instrument calibration records and payloads.

Binary `.dat` payloads are stored with Git LFS. Install Git LFS before cloning
or updating the repository so the calibration matrices are materialized:

```bash
brew install git-lfs       # macOS, once per machine
git lfs install
git lfs pull
```

## Manifest

```json
{
  "instrument": "SIMBIO-SYS",
  "version": "1.2"
}
```

The previous `version.yml` metadata file has been replaced by `manifest.json`.
Instrument manifests may additionally declare `channel` and `release`.

## Validation

From a `CalibDBReader` development environment:

```bash
uv run calibDB version /path/to/simbio_calib_db/stc
uv run calibDB dbdisplay \
  /path/to/simbio_calib_db/stc/sim_stc_cal_db_v1.0.csv --check
```

The `--check` option displays ✅ for existing calibration files and ❌ for
missing files.

## Calibration DB fields

- **Description**: processing-step description used in messages and logs.
- **Channel**: SIMBIO-SYS channel.
- **Calibration_Step**: name of the calibration step using the matrix.
- **Size**: matrix dimensions.
- **Mask**: value used to mask pixels; `0` disables masking.
- **Type**: matrix data type.
- **Func**: calibration function identifier.
- **Start**: validity start date.
- **End**: validity end date; `Now` means open-ended validity.
- **File**: path to the matrix relative to the repository root.

See [`CHANGELOG.md`](CHANGELOG.md) for database changes.
