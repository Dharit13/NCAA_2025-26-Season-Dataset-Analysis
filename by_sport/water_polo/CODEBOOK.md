> **v2.1 release note (2026-08-14; updated for v2.1.1).** The shipped roster files for this sport have **2,333 rows × 21 columns**: the v2.1.1 bio-sidecar split moved `major`, `previous_school`, `height_raw`/`height_in`, `weight_raw`/`weight_lbs` out of every roster file into `data/ncaa_athlete_bio_2025-26` (join on `athlete_id`), so "27-col schema" references below describe the pre-split sign-off layout, and the bio coverage table below still describes this sport's athletes — the fields simply ship in the sidecar. Figures below were computed at sport sign-off, before the release build removed 2 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 0 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Water Polo 2025-26 — Enriched (v2.1 staging)

Two files, joined on `athlete_id`. Men = fall 2025; Women = spring 2026.
- `ncaa_water_polo_2025-26_combined` (2,335 x 27) — locked 27-col schema
- `ncaa_water_polo_2025-26_stats` (1,612) — `gp, gs, goals, assists, points, shots, steals, exclusions, drawn_exclusions, blocks, gk_ga, gk_gaa, gk_saves, gk_sv_pct`

## Coverage
| column | Men (n=1,127) | Women (n=1,208) |
|---|---|---|
| height_in | 80.3% | 63.4% |
| weight_lbs | 47.6% | 0.0% |
| major | 43.7% | 37.1% |
| previous_school | 17.2% | 17.9% |

Athletes with >=1 stat: 69.0%.

## Notes
- GP up to 61 is real (Occidental's counting convention, verified live)
- LIU serves no stats page (schedule redirect) — nulls are structural
- 4 firehawk suffix-dup rows (v2.1 ledger)
