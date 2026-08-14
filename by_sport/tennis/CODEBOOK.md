> **v2.1.0 release note (2026-08-14).** The shipped file has **14,019 rows**. Figures below were computed at sport sign-off, before the release build removed 178 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 23 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Tennis 2025-26 — Enriched (v2.1 staging, roster-only)

One file: `ncaa_tennis_2025-26_combined` (14,197 x 27) — locked 27-col schema.
No stats sidecar (performance data external: Golfstat/ITA/etc.).

## Coverage
| column | Men (n=6,906) | Women (n=7,291) |
|---|---|---|
| height_in | 51.5% | 49.5% |
| weight_lbs | 17.1% | 0.0% |
| major | 32.1% | 32.2% |
| previous_school | 17.6% | 16.5% |

Sparse bio = publication convention (agents verified page absences).
previous_school is source-labeled (may be an HS at some schools — cross-check
the high_school column). 234 firehawk dup rows (ledger).
Recovery arsenal applied: nextgen API, WMT website-api, wayback unwrapping,
headless rendering.
