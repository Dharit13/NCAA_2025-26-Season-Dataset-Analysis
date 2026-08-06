---
license: cc0-1.0
language:
  - en
pretty_name: NCAA All-Sports Rosters 2025-26
size_categories:
  - 100K<n<1M
tags:
  - tabular
  - sports
  - sports-analytics
  - ncaa
  - college-athletics
  - rosters
  - student-athletes
  - recruiting
  - higher-education
  - social-science
  - demographics
  - united-states
configs:
  - config_name: default
    data_files:
      - split: full
        path: data/ncaa_all_sports_rosters_2025-26.parquet
  - config_name: acro_tumbling
    data_files:
      - split: full
        path: by_sport/acro_tumbling/all.parquet
  - config_name: baseball
    data_files:
      - split: full
        path: by_sport/baseball/all.parquet
  - config_name: basketball
    data_files:
      - split: full
        path: by_sport/basketball/all.parquet
  - config_name: beach_volleyball
    data_files:
      - split: full
        path: by_sport/beach_volleyball/all.parquet
  - config_name: bowling
    data_files:
      - split: full
        path: by_sport/bowling/all.parquet
  - config_name: cross_country
    data_files:
      - split: full
        path: by_sport/cross_country/all.parquet
  - config_name: equestrian
    data_files:
      - split: full
        path: by_sport/equestrian/all.parquet
  - config_name: fencing
    data_files:
      - split: full
        path: by_sport/fencing/all.parquet
  - config_name: field_hockey
    data_files:
      - split: full
        path: by_sport/field_hockey/all.parquet
  - config_name: football
    data_files:
      - split: full
        path: by_sport/football/all.parquet
  - config_name: golf
    data_files:
      - split: full
        path: by_sport/golf/all.parquet
  - config_name: gymnastics
    data_files:
      - split: full
        path: by_sport/gymnastics/all.parquet
  - config_name: ice_hockey
    data_files:
      - split: full
        path: by_sport/ice_hockey/all.parquet
  - config_name: lacrosse
    data_files:
      - split: full
        path: by_sport/lacrosse/all.parquet
  - config_name: rowing
    data_files:
      - split: full
        path: by_sport/rowing/all.parquet
  - config_name: rugby
    data_files:
      - split: full
        path: by_sport/rugby/all.parquet
  - config_name: skiing
    data_files:
      - split: full
        path: by_sport/skiing/all.parquet
  - config_name: soccer
    data_files:
      - split: full
        path: by_sport/soccer/all.parquet
  - config_name: softball
    data_files:
      - split: full
        path: by_sport/softball/all.parquet
  - config_name: stunt
    data_files:
      - split: full
        path: by_sport/stunt/all.parquet
  - config_name: swimming
    data_files:
      - split: full
        path: by_sport/swimming/all.parquet
  - config_name: tennis
    data_files:
      - split: full
        path: by_sport/tennis/all.parquet
  - config_name: track_indoor
    data_files:
      - split: full
        path: by_sport/track_indoor/all.parquet
  - config_name: track_outdoor
    data_files:
      - split: full
        path: by_sport/track_outdoor/all.parquet
  - config_name: triathlon
    data_files:
      - split: full
        path: by_sport/triathlon/all.parquet
  - config_name: volleyball
    data_files:
      - split: full
        path: by_sport/volleyball/all.parquet
  - config_name: water_polo
    data_files:
      - split: full
        path: by_sport/water_polo/all.parquet
  - config_name: wrestling
    data_files:
      - split: full
        path: by_sport/wrestling/all.parquet
---

# NCAA All Sports Rosters 2025-26

The first near-census of a full NCAA athletic year: **514,696 athlete roster records**
across **all 28 championship and emerging sports**, **1,087 schools**, all three
divisions (D1/D2/D3), men's and women's teams — one coherent year (2025-26), the first
season under the House v. NCAA settlement. Covers **~91.7%** of officially reported
NCAA participations (~96.9% of sponsor-list teams). **De-identified** (no names, no
photos, no bios); every field is an institution-published roster fact validated against
the NCAA's official sponsor lists.

**What it's for:** recruiting geography, international participation, division and
gender structure, high-school → college pathways, conference composition, roster-size
policy (House-settlement roster caps) — at individual-record resolution, without PII.

## Three things it shows out of the box

1. **Tennis (40.1%) and ice hockey (28.8%) have the highest international roster
   shares**; golf (20.2%) and water polo (19.7%) follow. Football and softball (1.5%
   each) are the most domestic. (Sports with ≥1,000 athletes; share of rows with
   `origin = international`.)
2. **Division III, not D1, is the largest slice of college sports:** 204,054 roster
   records vs 185,912 in D1 and 124,730 in D2.
3. **Prep-academy pipelines concentrate in specific sports:** among domestic athletes,
   rowing (12.2%), gymnastics (10.4%), and tennis (10.2%) list prep/academy high
   schools at the highest rates (`high_school_is_academy`).

## Quick start

Load everything (Parquet, 19 columns):

```python
import pandas as pd
df = pd.read_parquet("hf://datasets/dharits3/ncaa-college-athlete-rosters-2025-26/data/ncaa_all_sports_rosters_2025-26.parquet")
```

Load a single sport without downloading the rest — every sport is a config:

```python
from datasets import load_dataset
soccer = load_dataset("dharits3/ncaa-college-athlete-rosters-2025-26", "soccer", split="full")
```

Polars / DuckDB:

```python
import polars as pl
df = pl.read_parquet("hf://datasets/dharits3/ncaa-college-athlete-rosters-2025-26/data/ncaa_all_sports_rosters_2025-26.parquet")
```

```sql
SELECT sport, count(*) AS athletes,
       round(avg(CASE WHEN origin='international' THEN 1 ELSE 0 END)*100, 1) AS intl_pct
FROM read_parquet('hf://datasets/dharits3/ncaa-college-athlete-rosters-2025-26/data/ncaa_all_sports_rosters_2025-26.parquet')
GROUP BY 1 ORDER BY athletes DESC;
```

## Shape (v2.0.5)

| | |
|---|---|
| Athletes | **514,696** (Men 290,969 / Women 223,727) |
| Divisions | D1 185,912 · D2 124,730 · D3 204,054 |
| Origin | domestic 459,317 (89.2%) · international 44,241 (8.6%) · unknown 11,138 (2.2%) |
| Sports | 28 (every NCAA championship + emerging sport) |
| Schools | 1,087 |
| Athletic year | 2025-26 for every row |

## Schema & completeness

19 public columns (full definitions: [`data/CODEBOOK.md`](data/CODEBOOK.md)).

| Column | Meaning | Known | Read this first |
|---|---|---:|---|
| `sport` | Sport registry key | 100% | Indoor/outdoor track share source rows — see warning below |
| `division` / `gender` / `conference` / `school` | Team identity (2025-26 vintage) | 100% | Per-sport conferences and legit Independents exist |
| `origin` | domestic / international / unknown | 97.8% | Roster-listed origin, **not citizenship** |
| `hometown_city` / `hometown_state` | Parsed hometown | 97.8% / >99.9% of domestic | State is USPS incl. territories (PR/VI/GU/AS/MP) |
| `high_school` | High school / prep / academy as listed | 92.4% | Institution-published free text |
| `class_standing` | Fr/So/Jr/Sr/Gr | 98.0% | 2.0% UNK |
| `position_group` | Standardized position/event | 90.2% | 9.8% UNK; meaning varies by sport |

## Before you count across sports

**`track_indoor` and `track_outdoor` share source rows by design.** Schools publish one
track & field roster; indoor is materialized from the shared scrape and reconciled
against indoor sponsor lists. Distinct `athlete_id`s (`tfi_` / `tfo_`); **1,639**
overlapping school-gender teams. Never blindly dedupe across sports.

## Research using this dataset

| Output | One-line finding | Dataset version | Links |
|---|---|---|---|
| *The Beautiful Game's Class Ceiling: Who Reaches NCAA College Soccer on the Eve of the 2026 World Cup* (SocArXiv, 2026) | Selective-college access among NCAA soccer players runs 15.3% for the richest home-community quintile vs 3.7% for the poorest; men's rosters are 2.7× more international than women's. | v2.0.4 (54,559 soccer rows of 514,696) | [DOI](https://doi.org/10.31235/osf.io/9x4kg_v1) · [replication](https://osf.io/mu3wt/) |
| *Who Reaches the NBA Draft? Dual Concentration in College and High-School Pathways (2016–2026)* (SocArXiv, 2026) | Duke + Kentucky alone supply 9.5% of all 656 draft picks; two-thirds of draftees did not finish at a US public high school. | Separate derived panel (656 picks) | [DOI](https://doi.org/10.31235/osf.io/exarh_v1) · [replication](https://osf.io/yud8x/) |
| All-sports starter analysis (Kaggle notebook) | Coverage profile, sport/division/gender structure, state + international recruiting patterns. | v2.0.4 | [notebook](https://www.kaggle.com/code/shahdha/ncaa-all-sports-rosters-2025-26-analysis) |

## What's in this repo

```
├── data/
│   ├── ncaa_all_sports_rosters_2025-26.parquet   514,696 × 19 (public, de-identified)
│   └── CODEBOOK.md
├── by_sport/<sport>/all.parquet + all.csv         per-sport slices (identical 19-col schema; parquet loadable as configs)
├── metadata.json                                  build sidecar (counts, checksums, version)
├── index.html                                     Dataset Search / landing page
├── DATASHEET.md · ETHICS_REVIEW.md · DISCLOSURE_RISK.md · …
└── RELEASE_NOTES.md
```

**Public tier only:** 19 de-identified columns — no names, jersey numbers, photos, or
bios. An enriched 54-column analysis tier (Census town income, high-school typology,
Scorecard / mobility joins) is **research-only and is not publicly distributed**; those
joins are rebuildable from this public tier plus public sources (Census ACS, NCES,
College Scorecard, Opportunity Insights), or available by documented research request.

## Release status

**Released.** Current tip is **v2.0.5** (2026-08-06) — false-international origin fix:
6,969 US athletes at city-only-style rosters (bare-city / island-style / slash-mash
hometowns) corrected from `international` to `domestic` with gazetteer-resolved states,
verified against live roster pages. Prior: v2.0.4 territory fix (2026-07-18); v2.0.3
audit correction (2026-07-14); original v2.0 published 2026-07-08.

| | |
|---|---|
| **Dataset (HF, DOI)** | https://huggingface.co/datasets/dharits3/ncaa-college-athlete-rosters-2025-26 |
| **DOI** | [10.57967/hf/9512](https://doi.org/10.57967/hf/9512) |
| **Kaggle mirror** | https://www.kaggle.com/datasets/shahdha/ncaa-all-sports-rosters-2025-26 |
| **License** | CC0 1.0 (citation requested; no NIL / publicity grant) |
| **Version** | **2.0.5** ([GitHub releases](https://github.com/Dharit13/NCAA_2025-26-Season-Dataset-Analysis/releases)) |

### v2.0.5 corrections (summary)

- **False-international fix:** the `classify_origin` flaw behind the v2.0.4 territory
  fix also mislabeled US athletes at ~15 schools whose rosters print hometowns
  city-only ("Columbus"), island-style ("Honolulu, O'ahu"), or as "City, St. / High
  School" mashes (SUNY Cortland, the five OAC schools, UW-Oshkosh/La Crosse, Olivet,
  Eastern Connecticut, Cal State LA/DH, Hawaii, East Carolina, Coastal Carolina, UNCG,
  and a tail). **6,969 rows** now `origin=domestic` with `hometown_state` resolved via
  Census-gazetteer match; a 26-row stratified sample was verified against live roster
  pages (25 confirmed, 1 refuted — the refuting class, bare country names at schools
  that print domestic hometowns with states, was reverted). Genuinely international
  clusters (Simon Fraser, Lincoln Missouri's Jamaica pipeline, etc.) untouched.
- **Schema and row count unchanged** (514,696 athletes / 1,087 schools).

### Prior (v2.0.4)

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

## Provenance & quality

- Source: official school athletics-site rosters, scraped for the 2025-26 athletic year
  and validated against NCAA sport-sponsorship lists.
- Public tiers are PII-sanitized and audited clean (`pii_audit.txt`, `sanitize_report.txt`).
- Coverage vs official NCAA participation figures:
  [OFFICIAL_COMPARISON.md](OFFICIAL_COMPARISON.md).

## Citation

> Shah, Dharit (2026). *NCAA All Sports Rosters 2025-26: An Individual-Level Dataset
> Across All Divisions* (Version 2.0.5) [Data set]. Hugging Face.
> https://doi.org/10.57967/hf/9512

Machine-readable: [`CITATION.cff`](CITATION.cff).

## Governance

- [DATASHEET.md](DATASHEET.md) — Datasheets for Datasets
- [ETHICS_REVIEW.md](ETHICS_REVIEW.md) — ethics self-review / IRB exemption floor
- [DISCLOSURE_RISK.md](DISCLOSURE_RISK.md) — quasi-identifier analysis
- [LEGAL_NOTES.md](LEGAL_NOTES.md) — legal notes (facts/CC0, scraping, FERPA/NIL)
- [OPT_OUT.md](OPT_OUT.md) — per-record removal on request (dharits3@gmail.com)
