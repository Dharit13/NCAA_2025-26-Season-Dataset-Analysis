> **v2.1.0 release note (2026-08-14).** The shipped file has **321 rows**. Figures below were computed at sport sign-off, before the release build removed 10 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 0 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Triathlon 2025-26 — Enriched (v2.1 staging, roster-only)

One file: `ncaa_triathlon_2025-26_combined` (331 x 27) — locked 27-col schema.

## Coverage
| column | Men (n=0) | Women (n=331) |
|---|---|---|
| height_in | — | 37.2% |
| weight_lbs | — | 0.0% |
| major | — | 28.7% |
| previous_school | — | 7.3% |

Sparse bio = publication convention. Majors on athlete BIO pages (not roster
pages) are out of scope (profile scraping = 30x fetch cost, rejected early).
20 firehawk dup rows (ledger).
