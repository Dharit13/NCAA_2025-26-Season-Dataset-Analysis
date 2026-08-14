> **v2.1 release note (2026-08-14; updated for v2.1.1).** The shipped roster files for this sport have **321 rows × 21 columns**: the v2.1.1 bio-sidecar split moved `major`, `previous_school`, `height_raw`/`height_in`, `weight_raw`/`weight_lbs` out of every roster file into `data/ncaa_athlete_bio_2025-26` (join on `athlete_id`), so "27-col schema" references below describe the pre-split sign-off layout, and the bio coverage table below still describes this sport's athletes — the fields simply ship in the sidecar. Figures below were computed at sport sign-off, before the release build removed 10 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 0 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Triathlon 2025-26 — Enriched (v2.1 staging, roster-only)

One file: `ncaa_triathlon_2025-26_combined` (331 x 27) — locked 27-col schema.

## Coverage
| column | Men (n=0) | Women (n=331) |
|---|---|---|
| height_in | — | 37.2% |
| weight_lbs | — | 0.0% |
| major | — | 28.7% |
| previous_school | — | 7.3% |

Sparse bio = publication convention. Majors on athlete BIO pages (not roster
pages) are out of scope (profile scraping = 30x fetch cost, rejected early).
20 firehawk dup rows (ledger).
