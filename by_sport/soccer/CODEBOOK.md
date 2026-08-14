> **v2.1 release note (2026-08-14; updated for v2.1.1).** The shipped roster files for this sport have **54,537 rows × 21 columns**: the v2.1.1 bio-sidecar split moved `major`, `previous_school`, `height_raw`/`height_in`, `weight_raw`/`weight_lbs` out of every roster file into `data/ncaa_athlete_bio_2025-26` (join on `athlete_id`), so "27-col schema" references below describe the pre-split sign-off layout, and the bio coverage table below still describes this sport's athletes — the fields simply ship in the sidecar. Figures below were computed at sport sign-off, before the release build removed 22 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 247 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Soccer 2025 — Enriched (v2.1 staging)

Two files, joined on `athlete_id`:
- `ncaa_soccer_2025-26_combined` (54,559 rows x 27 cols) — locked 27-col schema
- `ncaa_soccer_2025-26_stats` (42,750 rows) — soccer vocabulary:
  `gp, gs, minutes, goals, assists, points, shots, sog, gwg, pk_goals, pk_att, yc, rc, gk_ga, gk_gaa, gk_saves, gk_sv_pct, gk_shutouts`

Points follow the NCAA convention `pts = 2*goals + assists`.

## Coverage

| column | Men (n=26,097) | Women (n=28,462) |
|---|---|---|
| height_in | 87.1% | 82.7% |
| weight_lbs | 57.2% | 0.0% |
| major | 37.6% | 36.0% |
| previous_school | 26.1% | 23.6% |

Athletes with >=1 stat: 78.4%.
Women's weight is not published (publication convention).

## Notes
- Sidearm goalkeeper season lines are client-rendered (JS payload) — gk_*
  columns fill mainly from Presto-platform schools; treat GK coverage as partial
- 88 of 156 client-rendered (WMT/JS) team pages recovered via headless pass;
  the remainder have release columns only
- 44 suffix-dup rows exist in the upstream published soccer release
  (firehawk dual-render bug) — queued for the v2.1 main-file fix
- height_in validated to [56, 84]; raw always preserved
