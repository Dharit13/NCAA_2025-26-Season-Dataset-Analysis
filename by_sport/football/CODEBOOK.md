> **v2.1.0 release note (2026-08-14).** The shipped file has **72,512 rows**. Figures below were computed at sport sign-off, before the release build removed 62 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 418 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Football 2025 — Enriched (v2.1 staging)

Two files, joined on `athlete_id` (fall 2025; men's sport):
- `ncaa_football_2025-26_combined` (72,574 x 27) — locked 27-col schema
- `ncaa_football_2025-26_stats` (34,982) — position-group columns:
  `gp, pass_cmp, pass_att, pass_yds, pass_td, pass_int, rush_att, rush_yds, rush_td, rec, rec_yds, rec_td, tackles_solo, tackles_ast, tackles_tot, tfl, sacks, def_int, pbu, fgm, fga, punts, punt_avg`

## Coverage (n=72,574)
| column | coverage |
|---|---|
| height_in | 90.3% |
| weight_lbs | 89.2% (highest of any sport — mass is the scouting language) |
| major | 25.7% |
| previous_school | 23.6% (JUCO/transfer pipeline) |

Athletes with >=1 stat: 48.2% —
structurally low: 100+ man rosters where redshirts/scout team record no stats
(verified as true non-appearances on live pages, incl. a twin-brother case).

## Notes
- stats extracted from the Sidearm __NUXT_DATA__ embedded payload (pages
  server-render only the passing tab); caption-routed table maps for Presto
- weight floor 85: Team IMPACT child honorees are real published roster
  entries (e.g. 4'6"/90 at Fordham) — kept faithfully
- known omission: Presto combined 'Sacks-YDS' cells ("1.0-4") not split —
  sacks may be null where a Presto page publishes them
- 122 firehawk suffix-dup rows in upstream release (largest; v2.1 ledger)
