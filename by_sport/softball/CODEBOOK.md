> **v2.1 release note (2026-08-14; updated for v2.1.1).** The shipped roster files for this sport have **20,661 rows × 21 columns**: the v2.1.1 bio-sidecar split moved `major`, `previous_school`, `height_raw`/`height_in`, `weight_raw`/`weight_lbs` out of every roster file into `data/ncaa_athlete_bio_2025-26` (join on `athlete_id`), so "27-col schema" references below describe the pre-split sign-off layout, and the bio coverage table below still describes this sport's athletes — the fields simply ship in the sidecar. Figures below were computed at sport sign-off, before the release build removed 9 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 124 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Softball 2026 — Enriched (v2.1 staging)

Two files, joined on `athlete_id` (spring 2026 season; women's sport):
- `ncaa_softball_2025-26_combined` (20,670 x 27) — locked 27-col schema
- `ncaa_softball_2025-26_stats` (17,160) — `gp, gs, ab, r, h, doubles, triples, hr, rbi, tb, bb, so, sb, sb_att, avg, ip, era, w, l, sv, k, app`

## Coverage (n=20,670, all women)

| column | coverage |
|---|---|
| height_in | 80.6% |
| weight_lbs | 0.0% (not published — publication convention) |
| major | 34.9% |
| previous_school | 22.4% |

Athletes with >=1 stat: 83.0%;
pitchers with pitching lines: 3,879.

## Notes
- batting and pitching parsed with CONTEXT-AWARE table mapping: pitching
  tables' h/r/ab/2b/hr columns are against-pitcher values and never enter
  batting columns (bug found by live-page verification, fixed 2026-08-13)
- `k` = pitcher strikeouts; `so` = batter strikeouts
- ERA up to 99.00 is real (site cap; 7-inning ERA on tiny IP samples)
- career tables skipped at parse (caption filter) + season-plausibility
  row filter (ab<=320 etc.) as defense in depth
- 18 firehawk suffix-dup rows in upstream release; 'Holt,/Lauren' (Cornell)
  inverted-name release bug -> v2.1 fix ledger
