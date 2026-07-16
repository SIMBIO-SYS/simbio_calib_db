# SIMBIO-SYS Calibration Database

This repository contains the calibration database for the SIMBIO-SYS suite on
board ESA's BepiColombo mission.

The database consists of:

- `manifest.json`, containing the database identity and version;
- `calib_db.csv`, indexing calibration validity and file metadata;
- `data/`, containing the calibration matrices and associated labels.

Values in the CSV `File` column are relative to the repository root. They must
therefore include the `data/` prefix, for example:

```text
data/response/stc/sim_cal_stc_resp_eq_T263_IBR01_v1.0.dat
```

This convention is shared by `CalibDBReader`, `simCal`, `stcCal`, `hricCal`,
and `vihiCal`.

## Manifest

```json
{
  "instrument": "SIMBIO-SYS",
  "version": "1.1"
}
```

The previous `version.yml` metadata file has been replaced by `manifest.json`.

## Validation

From a `CalibDBReader` development environment:

```bash
uv run calibDB version /path/to/simbio_calib_db
uv run calibDB dbdisplay /path/to/simbio_calib_db/calib_db.csv --check
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
