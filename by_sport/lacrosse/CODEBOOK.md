> **v2.1 release note (2026-08-14; updated for v2.1.1).** The shipped roster files for this sport have **29,221 rows × 21 columns**: the v2.1.1 bio-sidecar split moved `major`, `previous_school`, `height_raw`/`height_in`, `weight_raw`/`weight_lbs` out of every roster file into `data/ncaa_athlete_bio_2025-26` (join on `athlete_id`), so "27-col schema" references below describe the pre-split sign-off layout, and the bio coverage table below still describes this sport's athletes — the fields simply ship in the sidecar. Figures below were computed at sport sign-off, before the release build removed 15 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 48 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Lacrosse 2026 — Enriched (v2.1 staging)

Two files, joined on `athlete_id` (spring 2026 season, 2025-26 athletic year):
- `ncaa_lacrosse_2025-26_combined` (29,236 x 27) — locked 27-col schema
- `ncaa_lacrosse_2025-26_stats` (24,789) — `gp, gs, goals, assists, points, shots, sog, gwg, gb, to, ct, dc, fow, fo_att, minutes, gk_ga, gk_gaa, gk_saves, gk_sv_pct`

Points follow the lacrosse convention `pts = goals + assists`.
`dc` (draw controls) is women-only; `fow/fo_att` (faceoffs) men-only.

## Coverage

| column | Men (n=15,852) | Women (n=13,384) |
|---|---|---|
| height_in | 90.5% | 81.2% |
| weight_lbs | 76.2% | 0.0% |
| major | 40.4% | 38.6% |
| previous_school | 15.1% | 11.5% |

Athletes with >=1 stat: 84.8%.

## Notes
- weight band [90,380]: five 305–370 lb D3 men are school-published values
- some Presto schools 404 on /2025-26/stats (dead stats URL — nulls reflect
  the missing page, not verified zero seasons)
- 30 firehawk suffix-dup rows in the upstream release (v2.1 fix ledger)
- label-as-value bug (empty 'Previous College:' fields captured as text)
  found by live-page verification and fixed dataset-wide 2026-08-13
