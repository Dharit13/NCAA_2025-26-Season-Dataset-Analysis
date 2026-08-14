> **v2.1 release note (2026-08-14; updated for v2.1.1).** The shipped roster files for this sport have **32,614 rows × 21 columns**: the v2.1.1 bio-sidecar split moved `major`, `previous_school`, `height_raw`/`height_in`, `weight_raw`/`weight_lbs` out of every roster file into `data/ncaa_athlete_bio_2025-26` (join on `athlete_id`), so "27-col schema" references below describe the pre-split sign-off layout, and the bio coverage table below still describes this sport's athletes — the fields simply ship in the sidecar. Figures below were computed at sport sign-off, before the release build removed 0 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 194 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Basketball 2025-26 — Enriched (v2.1 staging)

Two files, joined on `athlete_id`:
- `ncaa_basketball_2025-26_combined` (32,614 rows x 27 cols) — locked
  27-col schema (published 19 + identity/bio 8)
- `ncaa_basketball_2025-26_stats` (26,818 rows) — basketball vocabulary:
  `gp, gs, minutes, fgm, fga, tpm, tpa, ftm, fta, pts, oreb, dreb, reb, ast, stl, blk, to, pf`

## Coverage

| column | Men (n=17,346) | Women (n=15,268) |
|---|---|---|
| height_in | 89.1% | 88.9% |
| weight_lbs | 69.2% | 0.0% |
| major | 29.9% | 32.9% |
| previous_school | 34.0% | 27.4% |

Athletes with >=1 stat: 82.2%.
Weight for women's programs is not published (publication convention).

## Notes
- height_in validated to [56, 93] in — the 93 is real (Olivier Rioux, Florida,
  7'9'', verified live + 305 lbs); values outside the band are school-side
  typos, kept verbatim in height_raw with height_in = NA
- percentages (fg%, 3pt%, ft%) deliberately not scraped — recompute from
  made/attempted
- ~4% of teams lack a parseable stats page (JS shells / Presto 404s)
- bare roster URLs had rolled to 2026-27; season-pinned re-fetch
  (/roster/2025-26) recovered 948 teams — all data is the 2025-26 season
