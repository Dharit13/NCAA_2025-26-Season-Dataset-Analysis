# NCAA All Sports Rosters 2025-26

Individual-level roster data for **every NCAA championship sport** — all 28 sports,
all three divisions, both genders, one athletic year (2025-26):
**515,085 athlete rows across 1,089 schools** (~91.8% of officially reported NCAA
participations; ≈96.9% of sponsor-list teams).

## Release status

**Released.** Current tip is **v2.0.1** (2026-07-13) — conference/division label
correction on the public 19-column tier. Original v2.0 published 2026-07-08.

| | |
|---|---|
| **Dataset (HF, DOI)** | https://huggingface.co/datasets/dharits3/ncaa-college-athlete-rosters-2025-26 |
| **DOI** | [10.57967/hf/9512](https://doi.org/10.57967/hf/9512) |
| **Kaggle mirror** | https://www.kaggle.com/datasets/shahdha/ncaa-all-sports-rosters-2025-26 |
| **License** | CC0 1.0 (citation requested; no NIL / publicity grant) |
| **Version** | **2.0.1** |

## What's in this repo

```
├── data/
│   ├── ncaa_all_sports_rosters_2025-26.parquet   515,085 × 19 (public, de-identified)
│   └── CODEBOOK.md
├── by_sport/                                      convenience slices (identical 19-col schema)
├── metadata.json                                  build sidecar (counts, checksums, version)
├── index.html                                     Dataset Search / landing page
├── DATASHEET.md · ETHICS_REVIEW.md · DISCLOSURE_RISK.md · …
└── RELEASE_NOTES.md
```

**Public tier only:** 19 de-identified columns — no names, jersey numbers, photos, or
bios. An enriched 54-column analysis tier (Census town income, high-school typology,
Scorecard / mobility joins) is **research-only** and is not shipped here; those joins
are rebuildable from this public tier plus public sources (Census ACS, NCES, College
Scorecard, Opportunity Insights).

## Shape (v2.0.1)

| | |
|---|---|
| Athletes | **515,085** (Men 291,171 / Women 223,914) |
| Divisions | D1 185,912 · D2 124,902 · D3 204,271 |
| Origin | domestic 452,555 (87.9%) · international 51,391 (10.0%) · unknown 11,139 (2.2%) |
| Sports | 28 (every NCAA championship + emerging sport) |
| Schools | 1,089 |
| Athletic year | 2025-26 for every row |

## v2.0.1 corrections (summary)

- **−308 rows:** Louisiana Christian University (NAIA) contamination removed from the
  Louisiana / UL Lafayette scrape path (`lcwildcats.net`).
- **Conference & division labels** re-verified for the **2025-26** vintage (stale
  realignment / core-collision labels corrected; division mislabels fixed).
- **Schema unchanged** (same 19 public columns).

**Known intentional exceptions**

| Case | Behavior |
|---|---|
| **Shawnee State** | Empty `conference` (172 rows). NCAA D2 member but competing as NAIA in 2025-26 — no NCAA conference affiliation that year. |
| **Independents** | `conference = Independent` is correct for full independents such as **Notre Dame**, **Maranatha Baptist**, and **Salem WV** (and for sport-specific independents elsewhere). |

Full changelog: [RELEASE_NOTES.md](RELEASE_NOTES.md).

## Before you count across sports

**`track_indoor` and `track_outdoor` share source rows by design.** Schools publish one
track & field roster; indoor is materialized from the shared scrape and reconciled
against indoor sponsor lists. Distinct `athlete_id`s (`tfi_` / `tfo_`); **1,639**
overlapping school-gender teams. Never blindly dedupe across sports.

## Provenance & quality

- Source: official school athletics-site rosters, scraped for the 2025-26 athletic year
  and validated against NCAA sport-sponsorship lists.
- Public tiers are PII-sanitized and audited clean (`pii_audit.txt`, `sanitize_report.txt`).
- Coverage vs official NCAA participation figures:
  [OFFICIAL_COMPARISON.md](OFFICIAL_COMPARISON.md).

## Citation

> Shah, Dharit (2026). *NCAA All Sports Rosters 2025-26: An Individual-Level Dataset
> Across All Divisions* (Version 2.0.1) [Data set]. Hugging Face.
> https://doi.org/10.57967/hf/9512

Machine-readable: [`CITATION.cff`](CITATION.cff).

## Governance

- [DATASHEET.md](DATASHEET.md) — Datasheets for Datasets
- [ETHICS_REVIEW.md](ETHICS_REVIEW.md) — ethics self-review / IRB exemption floor
- [DISCLOSURE_RISK.md](DISCLOSURE_RISK.md) — quasi-identifier analysis
- [LEGAL_NOTES.md](LEGAL_NOTES.md) — Feist / CC0, scraping, FERPA/NIL notes
- [OPT_OUT.md](OPT_OUT.md) — per-record removal on request (dharits3@gmail.com)
