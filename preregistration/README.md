# Pre-registration

Study `VN-SRC-2026-10`. Copies of the pre-registration as committed in the
publisher's harness repository, which is private. Taken from commit
`351391976d68a3610e9ea362bc512b125d9c17cf` (2026-10-15T15:32:43+07:00).

| File | What it is |
| --- | --- |
| spec-v1.0.md | The frozen specification: question, surfaces, size, classification summary and the six pre-registered numbers. |
| questions.md | The frozen question set, with the translation check. |
| classification.md | The frozen classification rules and worked examples. |
| amendments.md | The append-only amendments, entries 0 to 3. |

## Freeze

Recorded in amendments.md entry 0.

- Freeze commit: `6524dd5075675dcc0afb6ba2cdf5892cc67452ef`, branch `vn-sources-pilot`
- Commit timestamp: 2026-10-15T09:03:17+07:00 (2026-10-15T02:03:17Z)
- Nothing was dispatched before the freeze. The first answer was collected at
  2026-10-15T02:45:39Z.

| File | Blob at the freeze commit |
| --- | --- |
| `preregistration/vn-sources/spec-v1.0.md` | `392f214620fe206948d7dde85292712a4ca3ce23` |
| `preregistration/vn-sources/questions.md` | `36a7eccfa94795a97d7d1cb1c668bd2045ca1259` |
| `preregistration/vn-sources/classification.md` | `ee578cbf493ce7bc120ec54f2280827611f50087` |
| `wave-vn-sources/wave-config.json` | `b0dcae1645e96b9caf9d633a28fd3ebdd8448bb1` |
| `wave-vn-sources/inputs/query-set-v1.0.json` | `6441d33ba94f81d41521b7b7f25521a588f2fc6e` |
| `wave-vn-sources/inputs/jobs.json` | `5866090a7f022c72443863d8ec31481041b305e1` |

The three frozen files were unchanged at the source commit: their blobs there
are the blobs above. The last three files are not copied here.

## What was changed in these copies

spec-v1.0.md, questions.md and amendments.md are copied verbatim.
`git hash-object` on spec-v1.0.md and questions.md here gives the freeze blobs
above.

classification.md is copied verbatim except for the redactions below, so its
blob here does not match the freeze blob. Three business websites in the worked
examples are replaced with their identifiers, and URL paths are withheld.

| Original | Replaced with |
| --- | --- |
| A hotel group's domain (8 places, counting subdomains, plus its brand name once) | `[BW-077]` |
| A hotel's domain (2 places) | `[BW-255]` |
| A hotel group's domain (2 places) | `[BW-189]` |
| A retailer's domain (2 places) | `[BW-165]` |
| Subdomains of `[BW-077]` and `[BW-189]` that name a brand or a place | `[subdomain]`, `[another subdomain]` |
| URL paths on expedia.com, trip.com, thehoteljournal.com, vietnam.travel, foody.vn and on `[BW-077]`, `[BW-165]` and `[BW-255]` | `[path withheld]` |

The paths and file names these documents refer to, such as `archive/vn-sources/`
and `scripts/dispatch.py`, are in the harness repository and are not part of
this deposit.
