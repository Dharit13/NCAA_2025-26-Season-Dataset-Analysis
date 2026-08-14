> **v2.1.0 release note (2026-08-14).** The shipped file has **500 rows**. Figures below were computed at sport sign-off, before the release build removed 19 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 3 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Skiing 2025-26 — Enriched (v2.1 staging, roster-only)

One file: `ncaa_skiing_2025-26_combined` (519 x 27) — locked 27-col schema.

## Coverage
| column | Men (n=266) | Women (n=253) |
|---|---|---|
| height_in | 22.2% | 18.6% |
| weight_lbs | 5.3% | 0.0% |
| major | 27.4% | 22.1% |
| previous_school | 13.2% | 15.0% |

Sparse bio = publication convention. Majors on athlete BIO pages (not roster
pages) are out of scope (profile scraping = 30x fetch cost, rejected early).
38 firehawk dup rows (ledger).
