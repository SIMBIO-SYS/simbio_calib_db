# SIMBIO-SYS Calibration Database

This repository contains the calibration database for the SIMBIO-SYS suite on
board ESA's BepiColombo mission.

Current database version: `1.2`.

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
