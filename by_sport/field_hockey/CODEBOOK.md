> **v2.1 release note (2026-08-14; updated for v2.1.1).** The shipped roster files for this sport have **6,300 rows × 21 columns**: the v2.1.1 bio-sidecar split moved `major`, `previous_school`, `height_raw`/`height_in`, `weight_raw`/`weight_lbs` out of every roster file into `data/ncaa_athlete_bio_2025-26` (join on `athlete_id`), so "27-col schema" references below describe the pre-split sign-off layout, and the bio coverage table below still describes this sport's athletes — the fields simply ship in the sidecar. Figures below were computed at sport sign-off, before the release build removed 5 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 21 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Field Hockey 2025 — Enriched (v2.1 staging)

Two files, joined on `athlete_id` (fall 2025; women's sport):
- `ncaa_field_hockey_2025-26_combined` (6,305 x 27) — locked 27-col schema
- `ncaa_field_hockey_2025-26_stats` (5,439) — `gp, gs, minutes, goals, assists, points, shots, sog, gwg, pk_goals, pk_att, yc, rc, gk_ga, gk_gaa, gk_saves, gk_sv_pct, gk_shutouts`

Points follow the NCAA convention `pts = 2*goals + assists`.

## Coverage (n=6,305, all women)
| column | coverage |
|---|---|
| height_in | 79.7% |
| weight_lbs | 0.0% (not published) |
| major | 44.3% (best of any sport) |
| previous_school | 11.7% |

Athletes with >=1 stat: 86.3%.

## Notes
- 'Club Team' columns (sidearm custom fields) are NOT previous_school and
  are correctly excluded
- 10 firehawk suffix-dup rows in upstream release (v2.1 ledger)
- some school pages differ from NCAA-official totals by one game
  (school-page = our source of record; e.g. LIU vs NCAA for Haagmans)
