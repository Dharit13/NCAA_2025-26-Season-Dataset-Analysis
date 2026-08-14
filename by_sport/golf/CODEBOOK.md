> **v2.1 release note (2026-08-14; updated for v2.1.1).** The shipped roster files for this sport have **13,447 rows × 21 columns**: the v2.1.1 bio-sidecar split moved `major`, `previous_school`, `height_raw`/`height_in`, `weight_raw`/`weight_lbs` out of every roster file into `data/ncaa_athlete_bio_2025-26` (join on `athlete_id`), so "27-col schema" references below describe the pre-split sign-off layout, and the bio coverage table below still describes this sport's athletes — the fields simply ship in the sidecar. Figures below were computed at sport sign-off, before the release build removed 201 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 57 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Golf 2025-26 — Enriched (v2.1 staging, roster-only)

One file: `ncaa_golf_2025-26_combined` (13,648 x 27) — locked 27-col schema.
No stats sidecar (performance data external: Golfstat/ITA/etc.).

## Coverage
| column | Men (n=8,144) | Women (n=5,504) |
|---|---|---|
| height_in | 39.8% | 35.8% |
| weight_lbs | 14.4% | 0.0% |
| major | 29.0% | 27.3% |
| previous_school | 19.8% | 17.8% |

Sparse bio = publication convention (agents verified page absences).
previous_school is source-labeled (may be an HS at some schools — cross-check
the high_school column). 282 firehawk dup rows (ledger).
Recovery arsenal applied: nextgen API, WMT website-api, wayback unwrapping,
headless rendering.
