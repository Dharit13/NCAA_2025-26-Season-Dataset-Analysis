> **v2.1.0 release note (2026-08-14).** The shipped file has **10,727 rows**. Figures below were computed at sport sign-off, before the release build removed 23 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 18 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Wrestling 2025-26 — Enriched (v2.1 staging, roster-only)

One file: `ncaa_wrestling_2025-26_combined` (10,750 x 27) — locked 27-col schema.
No stats sidecar (performance data external: Golfstat/ITA/etc.).

## Coverage
| column | Men (n=8,794) | Women (n=1,956) |
|---|---|---|
| height_in | 33.3% | 34.3% |
| weight_lbs | 53.6% | 31.1% |
| major | 32.3% | 37.6% |
| previous_school | 14.8% | 16.2% |

Sparse bio = publication convention (agents verified page absences).
previous_school is source-labeled (may be an HS at some schools — cross-check
the high_school column). 16 firehawk dup rows (ledger).
Recovery arsenal applied: nextgen API, WMT website-api, wayback unwrapping,
headless rendering.
