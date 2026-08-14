> **v2.1 release note (2026-08-14; updated for v2.1.1).** The shipped roster files for this sport have **28,178 rows × 21 columns**: the v2.1.1 bio-sidecar split moved `major`, `previous_school`, `height_raw`/`height_in`, `weight_raw`/`weight_lbs` out of every roster file into `data/ncaa_athlete_bio_2025-26` (join on `athlete_id`), so "27-col schema" references below describe the pre-split sign-off layout, and the bio coverage table below still describes this sport's athletes — the fields simply ship in the sidecar. Figures below were computed at sport sign-off, before the release build removed 52 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 113 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Cross Country 2025-26 — Enriched (v2.1 staging, roster-only)

One file (no stats sidecar — performance data for this sport lives on
external systems, e.g. TFRRS/SwimCloud, out of scope):
- `ncaa_cross_country_2025-26_combined` (28,230 x 27) — locked 27-col schema

## Coverage
| column | Men (n=14,185) | Women (n=14,045) |
|---|---|---|
| height_in | 29.5% | 30.8% |
| weight_lbs | 10.9% | 0.0% |
| major | 28.9% | 32.7% |
| previous_school | 12.6% | 14.1% |

Low height/weight coverage is the PUBLICATION CONVENTION for individual
sports — most programs list only name/class/hometown/events. Verified by
live-page agents: pages with zero height spans are genuinely unpublished.

## Notes
- Sidearm NEXTGEN platform schools recovered via the JSON roster API
  (/api/v2/rosters) — 3 backfill passes; structured previousSchool included
- previous_school semantics vary by school: some label their high-school
  column "Previous School" — treat as source-labeled, cross-check high_school
- 76 firehawk suffix-dup rows in upstream release (v2.1 ledger)
- residual rejoin gap: WMT-platform schools (e.g. Stanford) + roster churn
