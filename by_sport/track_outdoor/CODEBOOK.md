> **v2.1.0 release note (2026-08-14).** The shipped file has **64,231 rows**. Figures below were computed at sport sign-off, before the release build removed 111 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 167 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Track Outdoor 2025-26 — Enriched (v2.1 staging, roster-only)

One file (no stats sidecar — performance data for this sport lives on
external systems, e.g. TFRRS/SwimCloud, out of scope):
- `ncaa_track_outdoor_2025-26_combined` (64,342 x 27) — locked 27-col schema

## Coverage
| column | Men (n=32,987) | Women (n=31,355) |
|---|---|---|
| height_in | 24.2% | 25.5% |
| weight_lbs | 9.6% | 0.0% |
| major | 26.5% | 29.2% |
| previous_school | 11.5% | 13.1% |

Low height/weight coverage is the PUBLICATION CONVENTION for individual
sports — most programs list only name/class/hometown/events. Verified by
live-page agents: pages with zero height spans are genuinely unpublished.

## Notes
- Sidearm NEXTGEN platform schools recovered via the JSON roster API
  (/api/v2/rosters) — 3 backfill passes; structured previousSchool included
- previous_school semantics vary by school: some label their high-school
  column "Previous School" — treat as source-labeled, cross-check high_school
- 194 firehawk suffix-dup rows in upstream release (v2.1 ledger)
- residual rejoin gap: WMT-platform schools (e.g. Stanford) + roster churn
