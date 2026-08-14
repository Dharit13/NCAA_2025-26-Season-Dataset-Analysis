> **v2.1.0 release note (2026-08-14).** The shipped file has **804 rows**. Figures below were computed at sport sign-off, before the release build removed 19 junk row(s) from this sport (duplicate renders / header artifacts) and repaired 0 name value(s); coverage percentages drift by at most ~2pp from the shipped file. Any 'suffix-dup rows' known-issue notes below are resolved in this release (ice hockey's Beloit 'Taylor' pair is two different athletes and both rows are kept).

# NCAA Bowling 2025-26 — Enriched (v2.1 staging, roster-only)

One file: `ncaa_bowling_2025-26_combined` (823 x 27) — locked 27-col schema.

## Coverage
| column | Men (n=0) | Women (n=823) |
|---|---|---|
| height_in | — | 38.2% |
| weight_lbs | — | 0.0% |
| major | — | 33.2% |
| previous_school | — | 14.7% |

Sparse bio = publication convention. Majors on athlete BIO pages (not roster
pages) are out of scope (profile scraping = 30x fetch cost, rejected early).
38 firehawk dup rows (ledger).
