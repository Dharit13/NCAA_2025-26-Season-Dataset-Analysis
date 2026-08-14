> **v2.1.0 release note (2026-08-14).** The shipped file has **38,033 rows**. Figures below were computed at sport sign-off, before the release build removed 16 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 200 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Baseball 2026 — Enriched (v2.1 staging)

Two files, joined on `athlete_id` (spring 2026; men's sport):
- `ncaa_baseball_2025-26_combined` (38,049 x 27) — locked 27-col schema
- `ncaa_baseball_2025-26_stats` (27,470) — `gp, gs, ab, r, h, doubles, triples, hr, rbi, tb, bb, so, sb, sb_att, avg, ip, era, w, l, sv, k, app`

## Coverage (n=38,049, all men)
| column | coverage |
|---|---|
| height_in | 90.0% |
| weight_lbs | 76.6% |
| major | 32.3% |
| previous_school | 33.8% (JUCO pipeline) |

Athletes with >=1 stat: 72.2%;
pitchers with pitching lines: 12,666.
`k` = pitcher strikeouts; `so` = batter strikeouts. Context-aware table
mapping keeps pitching-against values out of batting columns.

## Notes
- stats 72.2% is the lowest of the ball sports — baseball carries 35+ man
  rosters with many non-appearing players (verified correct on live pages),
  plus WMT-platform schools (e.g. Nebraska) 404 on pinned roster AND stats
  URLs (known platform gap, same as soccer)
- ERA up to 999 kept: 0.2-IP blowups are real 9-inning-ERA arithmetic
- 32 firehawk suffix-dup rows in upstream release (v2.1 ledger)
