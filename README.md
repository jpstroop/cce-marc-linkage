# cce-marc-linkage

A human-labeled training set linking MARC bibliographic records to U.S. Copyright Office Catalog of Copyright Entries (CCE) registrations and renewals. Each labeled pair carries a verdict (`match`, `no_match`, or `unsure`).

## Files

| Path | Contents |
|---|---|
| `marc.xml` | MARCXML `<collection>` of every distinct MARC record referenced by the dataset. |
| `training.jsonl` | One JSON object per line, every adjudicated verdict. Sorted by `labeled_at` ascending. |
| `matches.jsonl` | Same schema, filtered to `match` rows only. Same row order. |
| `LICENSE` | CC0 1.0 Universal. |

## License

[CC0 1.0 Universal](LICENSE) — no rights reserved.

## Schema for `training.jsonl` and `matches.jsonl`

Both files share the same row schema. One JSON object per line. Fields in declaration order:

| field | type | meaning |
|---|---|---|
| `lccn` | `string \| null` | Library of Congress Control Number from MARC `010 $a`. |
| `oclc` | `string \| null` | OCLC number from MARC `035 $a`, prefix stripped. |
| `isbns` | `string[]` | ISBNs from MARC `020 $a`, parser-order preserved. May be empty. |
| `cce_regnum` | `string \| null` | CCE registration number. Primary CCE identifier. |
| `cce_renewal_id` | `string \| null` | NYPL's id for the renewal record, or `null` if not renewed. |
| `cce_renewal_oreg` | `string \| null` | Renewal's transcribed reference to its original registration cite. |
| `nypl_uuid` | `string` | NYPL's UUID for the CCE registration record. |
| `verdict` | `string` | One of `match`, `no_match`, `unsure`. |
| `categories` | `string[]` | Structured rationale tags. Possible values: `marc_whole_cce_part`, `cce_whole_marc_part`, `translation`, `ocr_confusion`, `same_title_different_work`, `generic_title`. Often empty. |
| `labeled_at` | `string` | ISO 8601 timestamp (UTC). |
| `labeler` | `string` | Reviewer username. |
| `marc_control_id` | `string` | MARC catalog control ID. Provenance only. |

## Schema for `marc.xml`

MARCXML `<collection>` per the standard schema at <https://www.loc.gov/standards/marcxml/>. Records carry the full original field set with no stripping or normalization.

---

Built with [pd-matcher](https://github.com/jpstroop/pd-matcher).
