> **v2.1 release note (2026-08-14; updated for v2.1.1).** The shipped roster files for this sport have **2,031 rows × 21 columns**: the v2.1.1 bio-sidecar split moved `major`, `previous_school`, `height_raw`/`height_in`, `weight_raw`/`weight_lbs` out of every roster file into `data/ncaa_athlete_bio_2025-26` (join on `athlete_id`), so "27-col schema" references below describe the pre-split sign-off layout, and the bio coverage table below still describes this sport's athletes — the fields simply ship in the sidecar. Figures below were computed at sport sign-off, before the release build removed 11 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 21 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Gymnastics 2025-26 — Enriched (v2.1 staging, roster-only)

One file: `ncaa_gymnastics_2025-26_combined` (2,042 x 27) — locked 27-col schema.
No stats sidecar (performance data external: Golfstat/ITA/etc.).

## Coverage
| column | Men (n=317) | Women (n=1,725) |
|---|---|---|
| height_in | 71.9% | 50.0% |
| weight_lbs | 9.1% | 0.0% |
| major | 7.6% | 24.3% |
| previous_school | 19.9% | 17.0% |

Sparse bio = publication convention (agents verified page absences).
previous_school is source-labeled (may be an HS at some schools — cross-check
the high_school column). 12 firehawk dup rows (ledger).
Recovery arsenal applied: nextgen API, WMT website-api, wayback unwrapping,
headless rendering.
