# NCAA All Sports Rosters 2025-26

Individual-level roster data for **every NCAA championship sport** — all 28 sports,
all three divisions, both genders, one athletic year (2025-26):
**514,696 athlete rows across 1,087 schools** (~91.7% of officially reported NCAA
participations; ≈96.9% of sponsor-list teams).

## Release status

**Released.** Current tip is **v2.0.4** (2026-07-18) — US territory origin/state
fix plus CODEBOOK conference-coverage correction on the public 19-column tier.
Prior: v2.0.3 audit correction (2026-07-14); original v2.0 published 2026-07-08.

| | |
|---|---|
| **Dataset (HF, DOI)** | https://huggingface.co/datasets/dharits3/ncaa-college-athlete-rosters-2025-26 |
| **DOI** | [10.57967/hf/9512](https://doi.org/10.57967/hf/9512) |
| **Kaggle mirror** | https://www.kaggle.com/datasets/shahdha/ncaa-all-sports-rosters-2025-26 |
| **License** | CC0 1.0 (citation requested; no NIL / publicity grant) |
| **Version** | **2.0.4** |

## What's in this repo

```
├── data/
│   ├── ncaa_all_sports_rosters_2025-26.parquet   514,696 × 19 (public, de-identified)
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

## Shape (v2.0.4)

| | |
|---|---|
| Athletes | **514,696** (Men 290,969 / Women 223,727) |
| Divisions | D1 185,912 · D2 124,730 · D3 204,054 |
| Origin | domestic 452,348 (87.9%) · international 51,210 (9.9%) · unknown 11,138 (2.2%) |
| Sports | 28 (every NCAA championship + emerging sport) |
| Schools | 1,087 |
| Athletic year | 2025-26 for every row |

## v2.0.4 corrections (summary)

- **US territories:** 150 athletes from American Samoa, Guam, U.S. Virgin Islands,
  Northern Mariana Islands, and residual Puerto Rico rows were mislabeled
  `origin=international` with empty `hometown_state`. Now `domestic` with USPS codes
  `AS`/`GU`/`VI`/`MP`/`PR`. British Virgin Islands remain international.
- **CODEBOOK:** removed stale `conference` "~68%" coverage claim (public tier is fully
  populated since v2.0.3).
- **Schema and row count unchanged** (514,696 athletes / 1,087 schools).

### Prior (v2.0.3)

- **−389 rows / −2 schools:** removed Shawnee State (NAIA) and JWU Charlotte
  (exploratory/USCAA). Kept six NCAA provisional members.
- Conference labels web-verified; high-school recovery on public `high_school`.

**Known intentional exceptions**

| Case | Behavior |
|---|---|
| **Independents** | `conference = Independent` is correct for full independents such as **Notre Dame** (football), **Maranatha Baptist**, and **Salem WV** (and for sport-specific independents elsewhere). |

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
> Across All Divisions* (Version 2.0.4) [Data set]. Hugging Face.
> https://doi.org/10.57967/hf/9512

Machine-readable: [`CITATION.cff`](CITATION.cff).

## Governance

- [DATASHEET.md](DATASHEET.md) — Datasheets for Datasets
- [ETHICS_REVIEW.md](ETHICS_REVIEW.md) — ethics self-review / IRB exemption floor
- [DISCLOSURE_RISK.md](DISCLOSURE_RISK.md) — quasi-identifier analysis
- [LEGAL_NOTES.md](LEGAL_NOTES.md) — Feist / CC0, scraping, FERPA/NIL notes
- [OPT_OUT.md](OPT_OUT.md) — per-record removal on request (dharits3@gmail.com)
