> **v2.1.0 release note (2026-08-14).** The shipped file has **7,529 rows**. Figures below were computed at sport sign-off, before the release build removed 6 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 52 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Ice Hockey 2025-26 — Combined Dataset (preview)

**Architecture.** One combined main file per the published all-sports schema
plus identity/bio columns; sport-specific season stats live in a separate
per-sport file (stat columns differ by sport), joined on `athlete_id`:

```python
df = combined.merge(stats, on="athlete_id", how="left")
```

## Main file: ncaa_ice_hockey_2025-26_combined (7535 rows x 27 cols)

Column order: `athlete_id, first_name, last_name, major, previous_school,
height_raw, height_in, weight_raw, weight_lbs`, then the 18 published roster
columns (`sport ... source_url`) unchanged.

| new column | Men (n=4483) | Women (n=3052) |
|---|---|---|
| first_name / last_name | 100% | 100% |
| major | 13.8% | 15.9% |
| previous_school | 39.0% | 31.1% |
| height_in | 88.9% | 84.5% |
| weight_lbs | 75.3% | 0.0% |

## Stats file: ncaa_ice_hockey_2025-26_stats (6349 rows — athletes with >=1 stat)

Hockey-specific columns: `gp, goals, assists, points, plus_minus, minutes,
goalie_ga, goalie_gaa, goalie_saves, goalie_sv_pct`. Other sports will ship
their own stats file with their own column vocabulary (basketball: minutes,
fg/3pt/ft, rebounds...; soccer: shots, goals, saves...).

Known caveats (from verification battery):
- `minutes` is unreliable for goaltenders (skater-table zeros win the merge)
- extreme goalie rates (GAA > 8, SV% of 0.000/1.000) are real small-sample
  backup lines, not errors — filter on `gp` or `goalie_saves` for analysis
- 6 duplicate athlete pairs exist in the upstream published release
  (collision-suffixed `_2` ids); both copies carry the same stat line

All values are school-published facts (public roster + stats pages,
re-scraped 2026-08-13). Research-tier variables (race, income/SES) are not
in these files and are never distributed.
