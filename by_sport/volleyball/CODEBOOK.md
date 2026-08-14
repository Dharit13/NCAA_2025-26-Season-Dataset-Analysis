> **v2.1 release note (2026-08-14; updated for v2.1.1).** The shipped roster files for this sport have **21,394 rows × 21 columns**: the v2.1.1 bio-sidecar split moved `major`, `previous_school`, `height_raw`/`height_in`, `weight_raw`/`weight_lbs` out of every roster file into `data/ncaa_athlete_bio_2025-26` (join on `athlete_id`), so "27-col schema" references below describe the pre-split sign-off layout, and the bio coverage table below still describes this sport's athletes — the fields simply ship in the sidecar. Figures below were computed at sport sign-off, before the release build removed 14 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 95 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Volleyball 2025-26 — Enriched (v2.1 staging)

Two files, joined on `athlete_id`. Women = fall 2025; Men = winter/spring 2026.
- `ncaa_volleyball_2025-26_combined` (21,408 x 27) — locked 27-col schema
- `ncaa_volleyball_2025-26_stats` (17,767) — `mp, sets, kills, errors, ta, pct, assists, digs, aces, serve_errors, block_solos, block_assists, blocks, recv_errors, points`

Hitting pct = (kills − errors) / ta. `blocks` may be half-valued
(block-assist convention); `bs`/`ba` components included where published.

## Coverage
| column | Men (n=3,276) | Women (n=18,132) |
|---|---|---|
| height_in | 88.8% | 90.3% |
| weight_lbs | 28.4% | 0.0% |
| major | 39.7% | 36.8% |
| previous_school | 17.2% | 23.4% |

Athletes with >=1 stat: 83.0%.
NOTE: men's volleyball weight is mostly unpublished (28%) — the first men's
sport following the no-weight convention.

## Notes
- 7'1"–7'2" middle blockers are real (band [56,88])
- 27 firehawk suffix-dup rows in upstream release (v2.1 ledger)
