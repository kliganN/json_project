# json_project

> A compact JSON reference of Russian region codes with IANA time zones and current UTC offsets.

## Overview

`json_project` contains a single dataset: [`regions.json`](./regions.json).

Each record maps:

- a region name;
- a two-digit region code;
- an IANA time zone identifier;
- the current UTC offset in seconds.

This dataset is useful for:

- region code lookup and validation;
- resolving a time zone by region code;
- lightweight backend or frontend services that need a local reference without a database;
- ETL jobs, bots, CLI tools, and internal integrations.

## Project Structure

```text
json_project/
├── README.md
└── regions.json
```

## Data Format

The root element in `regions.json` is an array of objects.

### Record Schema

| Field | Type | Description |
| --- | --- | --- |
| `name` | `string` | Region name |
| `region` | `string` | Two-digit region code, for example `77` |
| `timezoneId` | `string` | IANA time zone ID, for example `Europe/Moscow` |
| `timezoneOffset` | `number` | Current UTC offset in seconds |

### Example Record

```json
{
  "name": "Республика Адыгея",
  "region": "01",
  "timezoneId": "Europe/Moscow",
  "timezoneOffset": 10800
}
```

## Current Status

- `89` records
- `24` unique time zones
- valid JSON syntax
- unique region codes
- all `timezoneId` values are valid in the current IANA tzdb
- all `timezoneOffset` values match current offsets for `2026-03-31`

## Validation

Basic syntax validation:

```bash
python3 -m json.tool regions.json > /dev/null
```

## Important Notes

- `timezoneOffset` is a current offset, not a historical time model. For past or future dates, use `timezoneId` and calculate the offset for the target date.
- The file stores one primary time zone per region code. That is practical for most use cases, but it does not capture every historical or local edge case.
- The dataset does not attempt to include all additional multi-digit vehicle code variants such as `97`, `99`, `102`, and similar values. It uses the base two-digit codes plus the current special codes present in the latest reference list.

## Sources

- Region codes and names were checked against the current government registration code list: [ConsultantPlus, Government Resolution No. 1507 dated September 21, 2020, revision dated July 10, 2025](https://www.consultant.ru/document/cons_doc_LAW_363244/1557851561df73c5e772db2455214c4c01c56cb6/)
- Time zone identifiers were normalized against the IANA time zone database: [IANA tzdb `zone1970.tab`](https://data.iana.org/time-zones/tzdb-2025a/zone1970.tab)

## Next Steps

If this project grows, the next sensible additions would be:

- a JSON Schema for automatic validation;
- a small validation script for region and time zone checks;
- dataset versioning based on update date.
