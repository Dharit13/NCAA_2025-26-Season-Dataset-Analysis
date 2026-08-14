> **v2.1.0 release note (2026-08-14).** The shipped file has **1,046 rows**. Figures below were computed at sport sign-off, before the release build removed 0 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 0 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Stunt 2025-26 — Enriched (v2.1 staging, roster-only)

One file: `ncaa_stunt_2025-26_combined` (1,046 x 27) — locked 27-col schema.

## Coverage
| column | Men (n=0) | Women (n=1,046) |
|---|---|---|
| height_in | — | 33.6% |
| weight_lbs | — | 0.0% |
| major | — | 25.4% |
| previous_school | — | 25.5% |

Sparse bio = publication convention. Majors on athlete BIO pages (not roster
pages) are out of scope (profile scraping = 30x fetch cost, rejected early).
0 firehawk dup rows (ledger).
