# cce-marc-linkage

The **training bundle** for [pd-matcher](https://github.com/jpstroop/pd-matcher):
the human-verified ground-truth labels and the MARC records they reference, used
to train the learned (LightGBM) matcher.

This is a single-institution snapshot (Princeton MARC against the
NYPL-transcribed Catalog of Copyright Entries) — **not** a universal answer key.
Every catalog is different, so the reusable product is the labeling and *this
training data*, not a fixed list of matches.

## Files

| Path | Contents |
|---|---|
| `label_vault.jsonl` | The label vault — one JSON object per adjudicated `(MARC record, CCE registration)` pair. The source of truth. |
| `marc.xml` | MARCXML `<collection>` of every MARC record referenced by the vault, so pairs can be re-scored without the full source catalog. |
| `LICENSE` | CC0 1.0 Universal. |

The copyright (CCE) side of each pair lives in NYPL's transcriptions
([registrations](https://github.com/NYPL/catalog_of_copyright_entries_project),
[renewals](https://github.com/NYPL/cce-renewals)), which pd-matcher consumes as
its own submodules.

## Training from this bundle

pd-matcher mounts this repo as its `data/training/` submodule. To train the
learned matcher: pull this bundle plus the NYPL CCE submodules, build the index,
then run `pd-matcher train-scorer`. Full instructions are in pd-matcher's
[docs/LEARNED_MATCHER.md](https://github.com/jpstroop/pd-matcher/blob/main/docs/LEARNED_MATCHER.md).

## License

[CC0 1.0 Universal](LICENSE) — no rights reserved. The underlying Catalog of
Copyright Entries is a work of the U.S. Copyright Office and is in the public
domain; the MARC records are Princeton's catalog data; the labels are released
CC0.

## `label_vault.jsonl` schema

One JSON object per line. Fields:

| field | type | meaning |
|---|---|---|
| `schema` | int | Vault schema version. |
| `verdict` | string | `match`, `no_match`, or `unsure`. |
| `marc_control_id` | string | MARC catalog control ID. |
| `nypl_uuid` | string | NYPL UUID for the CCE registration. |
| `cce_regnum` | string \| null | CCE registration number. |
| `cce_renewal_id` | string \| null | NYPL id for the renewal record, or `null` if not renewed. |
| `cce_renewal_oreg` | string \| null | Renewal's transcribed reference to its original registration. |
| `marc_identifiers` | object | `{ "lccn": string\|null, "oclc": string\|null, "isbns": string[] }` extracted from the MARC record. |
| `categories` | string[] | Structured rationale tags: `marc_whole_cce_part`, `cce_whole_marc_part`, `translation`, `different_edition`, `ocr_confusion`, `same_title_different_work`, `generic_title`. Often empty. |
| `note` | string \| null | The labeler's free-text rationale. |
| `labeled_at` | string | ISO 8601 timestamp (UTC). |
| `labeler` | string | Reviewer username. |

## `marc.xml` schema

MARCXML `<collection>` per the standard schema at
<https://www.loc.gov/standards/marcxml/>. Records carry the full original field
set with no stripping or normalization.

---

Built with [pd-matcher](https://github.com/jpstroop/pd-matcher).
