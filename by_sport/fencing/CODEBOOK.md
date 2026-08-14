> **v2.1.0 release note (2026-08-14).** The shipped file has **1,212 rows**. Figures below were computed at sport sign-off, before the release build removed 2 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 0 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Fencing 2025-26 — Enriched (v2.1 staging, roster-only)

One file: `ncaa_fencing_2025-26_combined` (1,214 x 27) — locked 27-col schema.
No stats sidecar (performance data external: Golfstat/ITA/etc.).

## Coverage
| column | Men (n=588) | Women (n=626) |
|---|---|---|
| height_in | 40.3% | 44.6% |
| weight_lbs | 18.9% | 0.0% |
| major | 31.8% | 25.4% |
| previous_school | 0.3% | 0.5% |

Sparse bio = publication convention (agents verified page absences).
previous_school is source-labeled (may be an HS at some schools — cross-check
the high_school column). 0 firehawk dup rows (ledger).
Recovery arsenal applied: nextgen API, WMT website-api, wayback unwrapping,
headless rendering.
