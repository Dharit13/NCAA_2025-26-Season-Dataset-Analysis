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
        path: data/ncaa_all_sports_rosters_2025-26_enriched.parquet
  - config_name: bio
    data_files:
      - split: full
        path: data/ncaa_athlete_bio_2025-26.parquet
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
  - config_name: baseball_stats
    data_files:
      - split: full
        path: by_sport/baseball/stats.parquet
  - config_name: basketball_stats
    data_files:
      - split: full
        path: by_sport/basketball/stats.parquet
  - config_name: field_hockey_stats
    data_files:
      - split: full
        path: by_sport/field_hockey/stats.parquet
  - config_name: football_stats
    data_files:
      - split: full
        path: by_sport/football/stats.parquet
  - config_name: ice_hockey_stats
    data_files:
      - split: full
        path: by_sport/ice_hockey/stats.parquet
  - config_name: lacrosse_stats
    data_files:
      - split: full
        path: by_sport/lacrosse/stats.parquet
  - config_name: soccer_stats
    data_files:
      - split: full
        path: by_sport/soccer/stats.parquet
  - config_name: softball_stats
    data_files:
      - split: full
        path: by_sport/softball/stats.parquet
  - config_name: volleyball_stats
    data_files:
      - split: full
        path: by_sport/volleyball/stats.parquet
  - config_name: water_polo_stats
    data_files:
      - split: full
        path: by_sport/water_polo/stats.parquet
---

# NCAA All Sports Rosters 2025-26

A near-census of a full NCAA athletic year — now **named and enriched**.
**513,655 athlete roster records** across **all 28 championship and emerging
sports**, **1,087 schools**, all three divisions (D1/D2/D3), men's and women's
teams, one coherent year (2025-26, the first season under the House v. NCAA
settlement). Every field is an institution-published roster fact from official
school athletics sites, validated against the NCAA's official sponsor lists.

**New in v2.1** (2026-08-14; current: v2.1.1):

- **Names are public.** `first_name` / `last_name` are columns on every row —
  they are school-published facts on public roster pages, and publishing them
  makes every record verifiable against its `source_url` and linkable to other
  data. (`last_name` is null for the ~0.5% of listings that publish a single
  name.)
- **8 new columns:** 2 name columns (`first_name`/`last_name`, in the roster
  file) + 6 bio fields (`major`, `previous_school`, `height_raw`/`height_in`,
  `weight_raw`/`weight_lbs`) in the compact **bio sidecar**
  `data/ncaa_athlete_bio_2025-26` (373,817 rows — athletes with ≥1 bio field —
  joined on `athlete_id`; raw + parsed pairs ship together). As of the v2.1.1
  split the roster file is 21 columns (the 19 v2.0.5 columns unchanged + names;
  column order locked) — **21 roster columns + 7 bio-sidecar columns, 27
  distinct fields across two files**.
- **Season-stats sidecars for 10 sports** — `by_sport/<sport>/stats.parquet`,
  205,132 stat rows (39.9% of athletes), joinable on `athlete_id`.
- **1,041 junk rows removed** and **1,972 defective names repaired** (details in
  [RELEASE_NOTES.md](RELEASE_NOTES.md)).

**What it's for:** recruiting geography, international participation, division
and gender structure, high-school → college pathways, walk-on vs contributor
analysis (roster × stats), academic-major composition, transfer pathways
(`previous_school`), anthropometrics by sport — at individual-record resolution.

## Three things it shows out of the box

1. **Tennis (40.1%) and ice hockey (28.8%) have the highest international roster
   shares**; golf (20.2%) and water polo (19.7%) follow. Football and softball
   (1.5% each) are the most domestic. (Sports with ≥1,000 athletes.)
2. **Division III, not D1, is the largest slice of college sports:** 203,909
   roster records vs 185,086 in D1 and 124,660 in D2.
3. **What a roster omits is a publication convention, and the data shows it:**
   football rosters list weight for 89.3% of athletes; softball and field hockey
   list it for 0%. Height/weight/major gaps track school and sport publishing
   norms, not scraping failures.

## Quick start

Load the roster file (Parquet, 513,655 × 21; bio and stats join in below — see
**Joining the files**):

```python
import pandas as pd
df = pd.read_parquet("hf://datasets/dharits3/ncaa-college-athlete-rosters-2025-26/data/ncaa_all_sports_rosters_2025-26_enriched.parquet")
```

Load a single sport without downloading the rest — every sport is a config, the
bio sidecar is the `bio` config, and each of the 10 stats sidecars is a
`<sport>_stats` config:

```python
from datasets import load_dataset
soccer = load_dataset("dharits3/ncaa-college-athlete-rosters-2025-26", "soccer", split="full")
```

Just want a look? [`samples/ncaa_rosters_sample_10000.csv`](samples/ncaa_rosters_sample_10000.csv)
is a 10,000-row sample; [`samples/sport_summary.csv`](samples/sport_summary.csv)
has per-sport counts and coverage.

## Joining the files

Every file in the release joins on **`athlete_id`**, with two scopes:

- The **bio sidecar** (`data/ncaa_athlete_bio_2025-26`) is one file covering
  every sport — it joins to the full roster file, any sport slice, or any
  gender/division CSV.
- **Stats joins are within ONE sport only.** Stat columns are sport-specific
  (a goalie's GAA has no football equivalent), so a stats join starts from that
  sport's roster slice under `by_sport/`, never the cross-sport file.

Every stats file also carries `first_name`/`last_name` so it reads standalone;
drop them before joining to a roster slice, which already has both.

**(a) Roster + bio** — works across all sports:

```python
import pandas as pd
base   = "hf://datasets/dharits3/ncaa-college-athlete-rosters-2025-26"
roster = pd.read_parquet(f"{base}/data/ncaa_all_sports_rosters_2025-26_enriched.parquet")  # 513,655 × 21
bio    = pd.read_parquet(f"{base}/data/ncaa_athlete_bio_2025-26.parquet")                  # 373,817 × 7
df = roster.merge(bio, on="athlete_id", how="left")  # 513,655 rows, 27 columns
# athletes with no published bio field have no sidecar row -> NaN bio columns
```

**(b) One sport's roster slice + its stats** (basketball):

```python
import pandas as pd
base   = "hf://datasets/dharits3/ncaa-college-athlete-rosters-2025-26"
roster = pd.read_parquet(f"{base}/by_sport/basketball/all.parquet")    # 32,614 roster rows
stats  = pd.read_parquet(f"{base}/by_sport/basketball/stats.parquet")  # 26,818 stat rows
# stats files carry first_name/last_name for standalone readability;
# drop them here — the roster slice already has both.
bb = roster.merge(stats.drop(columns=["first_name", "last_name"]), on="athlete_id", how="left")
# left join keeps all 32,614 athletes; no published stat line -> NaN stats
```

**(c) The full three-file pattern for one sport** (roster slice + bio + stats):

```python
import pandas as pd
base   = "hf://datasets/dharits3/ncaa-college-athlete-rosters-2025-26"
roster = pd.read_parquet(f"{base}/by_sport/basketball/all.parquet")       # one sport's roster slice
bio    = pd.read_parquet(f"{base}/data/ncaa_athlete_bio_2025-26.parquet") # bio sidecar (all sports)
stats  = pd.read_parquet(f"{base}/by_sport/basketball/stats.parquet")     # THIS sport's stat columns

full = (roster
        .merge(bio, on="athlete_id", how="left")
        .merge(stats.drop(columns=["first_name", "last_name"]), on="athlete_id", how="left"))
```

**(d) With the `datasets` library** — the sidecars are configs too:

```python
from datasets import load_dataset
bio      = load_dataset("dharits3/ncaa-college-athlete-rosters-2025-26", "bio", split="full")
bb_stats = load_dataset("dharits3/ncaa-college-athlete-rosters-2025-26", "basketball_stats", split="full")
```

DuckDB / Polars read (and join) the same paths:

```sql
SELECT r.sport, count(*) AS athletes,
       round(100.0 * avg(CASE WHEN b.height_in IS NOT NULL THEN 1 ELSE 0 END), 1) AS height_pct,
       round(100.0 * avg(CASE WHEN r.origin = 'international' THEN 1 ELSE 0 END), 1) AS intl_pct
FROM read_parquet('hf://datasets/dharits3/ncaa-college-athlete-rosters-2025-26/data/ncaa_all_sports_rosters_2025-26_enriched.parquet') r
LEFT JOIN read_parquet('hf://datasets/dharits3/ncaa-college-athlete-rosters-2025-26/data/ncaa_athlete_bio_2025-26.parquet') b
       USING (athlete_id)
GROUP BY 1 ORDER BY athletes DESC;
```

```python
import polars as pl
df = pl.read_parquet("hf://datasets/dharits3/ncaa-college-athlete-rosters-2025-26/data/ncaa_all_sports_rosters_2025-26_enriched.parquet")
```

Left-joins keep every rostered athlete; missing bio/stats values are real
roster states (redshirts, fields a school didn't publish), not join failures.
As of v2.1.1 the bio columns live **only** in the sidecar — the roster file
stays lean and the sparse fields join in when you need them. The gender ×
division CSVs under `by_sport/<sport>/<gender>/` are row-slices of the same
21-column schema; anything you compute on them joins back the same way.

## Shape (v2.1.1)

| | |
|---|---|
| Athletes | **513,655** (Men 290,510 / Women 223,145) |
| Divisions | D1 185,086 · D2 124,660 · D3 203,909 |
| Origin | domestic 459,068 (89.4%) · international 43,853 (8.5%) · unknown 10,734 (2.1%) |
| Sports | 28 (every NCAA championship + emerging sport) |
| Schools | 1,087 |
| Columns | 21 roster + 7 bio-sidecar (27 distinct fields across two files, joined on `athlete_id`) + per-sport stats |
| Season stats | 10 sports · 205,132 rows · 39.9% of athletes |
| Athletic year | 2025-26 for every row |

## Schema & completeness

Two joinable files (v2.1.1 split). **Bold** = new in v2.1.x. Machine-readable
version: [`samples/data_dictionary.csv`](samples/data_dictionary.csv); per-sport
notes in each `by_sport/<sport>/CODEBOOK.md`.

**Roster file** (`data/ncaa_all_sports_rosters_2025-26_enriched`, 513,655 × 21,
in file order):

| Column | Meaning | Known |
|---|---|---:|
| `athlete_id` | Stable sport-prefixed surrogate key; joins to the `stats` files and the bio sidecar | 100% |
| **`first_name`** | First/given name as school-published | 100% |
| **`last_name`** | Last/family name as school-published | 99.5% |
| `sport` | Sport registry key — indoor/outdoor track share source rows, see below | 100% |
| `athletic_year` | 2025-26 for every row | 100% |
| `season` | Sport's season label (`2025`, `2026`, `2025-26`) | 100% |
| `division` | D1 / D2 / D3 per official NCAA sponsor list | 100% |
| `gender` | Team competition gender | 100% |
| `conference` | Athletic conference, 2025-26 vintage; fully populated | 100% |
| `school` | Institution name | 100% |
| `position_raw` | Position / event / weight class as listed | 81.6% |
| `position_group` | Standardized position grouping (`UNK` where unlisted) | 100% |
| `class_year_raw` | Class as listed (`Fr.`, `R-So.`, `Gr.`) | 99.1% |
| `class_standing` | Standardized Fr/So/Jr/Sr/Gr/UNK | 100% |
| `hometown_raw` | Hometown as listed | 97.9% |
| `hometown_city` | Parsed city | 97.9% |
| `hometown_state` | Parsed USPS state/territory (incl. PR/VI/GU/AS/MP) | 89.4% |
| `origin` | domestic / international / unknown — roster-listed, **not citizenship** | 100% |
| `high_school` | High school / prep / academy as listed | 92.5% |
| `high_school_is_academy` | Legacy prep/academy flag | 99.8% |
| `source_url` | Roster page / API endpoint the row came from | 100% |

**Bio sidecar** (`data/ncaa_athlete_bio_2025-26`, 373,817 × 7 — only athletes
with ≥1 published bio field; "Known" shown within-file / of all 513,655
athletes):

| Column | Meaning | Known |
|---|---|---:|
| `athlete_id` | Join key (subset of the roster file's ids) | 100% / 72.8% |
| **`major`** | Academic major as listed on roster/bio page | 42.4% / 30.9% |
| **`previous_school`** | Previous school as listed (college, junior/club, or HS — source-labeled) | 26.8% / 19.5% |
| **`height_raw`** | Height as published (`6-2`, `5'11"`) | 85.5% / 62.2% |
| **`height_in`** | Parsed height, inches | 85.4% / 62.2% |
| **`weight_raw`** | Weight as published | 41.4% / 30.1% |
| **`weight_lbs`** | Parsed weight, pounds | 41.3% / 30.0% |

> **Missingness is a publication convention, not a defect.** Schools choose what
> their rosters publish, and conventions differ sharply by sport and gender:
> women's weight is ~0% in many sports by convention (softball 0%, field hockey
> 0%) while football publishes it at 89.3%; cross-country programs rarely list
> height. Columns are never dropped for low coverage — a blank means the school
> did not publish it. Compare coverage rates across sports in
> [`samples/sport_summary.csv`](samples/sport_summary.csv) before treating a
> gap as data loss.

## Season-stats sidecars (new)

`by_sport/<sport>/stats.parquet` (+ `.csv`) for 10 sports. Columns are
**sport-specific by design** (a baseball stat file has no reason to share a
schema with soccer's); every file joins to its sport's roster slice on
`athlete_id`. Every stats file also carries `first_name`/`last_name` so it
reads standalone — drop them when joining to a roster slice (see **Joining the
files**). 205,132 athletes (39.9%) have a stat row.

| Sport | Stat rows | Sport | Stat rows |
|---|---:|---|---:|
| soccer | 42,750 | ice_hockey | 6,345 |
| football | 34,982 | field_hockey | 5,439 |
| baseball | 27,470 | water_polo | 1,612 |
| basketball | 26,818 | | |
| lacrosse | 24,789 | | |
| volleyball | 17,767 | | |
| softball | 17,160 | | |

## Before you count across sports

**`track_indoor` and `track_outdoor` share source rows by design.** Schools
publish one track & field roster; indoor is materialized from the shared scrape
and reconciled against indoor sponsor lists. Distinct `athlete_id`s
(`tfi_` / `tfo_`) for the same person-season. Never blindly dedupe across
sports; when counting unique athletes, decide explicitly how to treat
indoor/outdoor track.

## Access: what ships and what never will

The public tier is everything in this repo: **21 roster columns + 7 bio-sidecar
columns** (27 distinct fields across two files) — all school-published roster
facts — **plus the per-sport season-stats sidecars**.
There is also a **research tier** (BISG race/ethnicity predictions,
home-community income/SES, Census-tract and mobility joins) that is
**research-only, is in no distributed file, and never will be**; it is not
reconstructable from the released fields. No photos, no contact information, no
birthdates, no social-media handles. The shipped build passed a PII audit.

Names are published because they are facts the schools themselves publish on
public roster pages; every row carries the `source_url` it came from. Per-record
removal is honored on request — see [OPT_OUT.md](OPT_OUT.md).

## Research using this dataset

| Output | One-line finding | Dataset version | Links |
|---|---|---|---|
| *The Beautiful Game's Class Ceiling: Who Reaches NCAA College Soccer on the Eve of the 2026 World Cup* (SocArXiv, 2026) | Selective-college access among NCAA soccer players runs 15.3% for the richest home-community quintile vs 3.7% for the poorest; men's rosters are 2.7× more international than women's. | v2.0.4 (soccer slice) | [DOI](https://doi.org/10.31235/osf.io/9x4kg_v1) · [replication](https://osf.io/mu3wt/) |
| *Who Reaches the NBA Draft? Dual Concentration in College and High-School Pathways (2016–2026)* (SocArXiv, 2026) | Duke + Kentucky alone supply 9.5% of all 656 draft picks; two-thirds of draftees did not finish at a US public high school. | Separate derived panel (656 picks) | [DOI](https://doi.org/10.31235/osf.io/exarh_v1) · [replication](https://osf.io/yud8x/) |
| **Starter notebook** (Kaggle) | Load, two-file stats join, coverage heatmap, heights, the weight-publication convention, pathways, manifest reproducibility check. | v2.1.0 | [notebook](https://www.kaggle.com/code/shahdha/ncaa-rosters-starter) |
| All-sports analysis (Kaggle notebook) | Headline findings: sport/division/gender structure, international shares, state production, class pyramid. | v2.1.0 | [notebook](https://www.kaggle.com/code/shahdha/ncaa-all-sports-analysis) |

## What's in this repo

```
├── data/
│   ├── ncaa_all_sports_rosters_2025-26_enriched.parquet   513,655 × 21 (identity + team + hometown)
│   ├── ncaa_all_sports_rosters_2025-26_enriched.csv       same data
│   ├── ncaa_athlete_bio_2025-26.parquet + .csv            bio sidecar: athlete_id + major/prev_school/ht/wt, 373,817 rows (athletes with ≥1 bio field)
│   └── CODEBOOK.md                                        full column dictionary (roster + bio sidecar + stats architecture)
├── by_sport/<sport>/
│   ├── all.parquet + all.csv           per-sport slice (same 21 cols; parquet = HF config)
│   ├── <gender>/all.csv, d1/d2/d3.csv  gender × division CSV slices
│   ├── stats.parquet + stats.csv       season stats (10 sports; join on athlete_id)
│   └── CODEBOOK.md
├── samples/                            10k sample · data dictionary · sport & school summaries
├── metadata.json · MANIFEST.json       build sidecars (row counts, sha256 per file)
├── DATASHEET.md · ETHICS_REVIEW.md · DISCLOSURE_RISK.md · LEGAL_NOTES.md · OPT_OUT.md · OFFICIAL_COMPARISON.md
├── LICENSE · CITATION.cff
└── RELEASE_NOTES.md                    changelog + opt-out removals ledger
```

## Release status

**Current: v2.1.1** (2026-08-14) — the **bio-sidecar split**, same day as v2.1.0.

- The six sparse bio columns (`major`, `previous_school`, `height_raw`,
  `height_in`, `weight_raw`, `weight_lbs`) moved out of the roster file into
  `data/ncaa_athlete_bio_2025-26.{parquet,csv}` (373,817 rows — athletes with
  ≥1 published bio field), joined on `athlete_id`. Same values, same ids; no
  data changed. The roster file is now 21 columns.

**v2.1.0** (2026-08-14) — the **named + enriched** release.

- Names public; 8 new columns; stats sidecars for 10 sports; consolidated file
  renamed to `data/ncaa_all_sports_rosters_2025-26_enriched.parquet`.
- **513,655 rows** (v2.0.5: 514,696). The −1,041 are junk rows inherited from
  v2.0.x source data: 845 dual-render duplicate rows (`_N`-suffixed
  `athlete_id`s from a platform that rendered rosters twice) and 196
  header-artifact rows (e.g. a `Ht.` column header scraped as a name).
- **Name repairs** (all inherited defects, each change logged): 1,562
  `"Last," / "First"` comma-swaps, 407 jersey-number-in-name fixes (recovering
  80 height/weight values from mashed strings), 2 trailing-comma strips, 1
  manual correction.
- Verification: all 28 sports individually signed off — internal battery plus an
  adversarial live-page workflow (~3,000 field checks against live roster pages,
  zero wrong-value findings) plus external record-book cross-checks for the
  stats sports. See [RELEASE_NOTES.md](RELEASE_NOTES.md).

Prior versions (condensed; full changelog in [RELEASE_NOTES.md](RELEASE_NOTES.md)):

| Version | Date | Change |
|---|---|---|
| v2.0.5 | 2026-08-06 | False-international origin fix (6,969 rows re-labeled domestic, gazetteer-resolved states) |
| v2.0.4 | 2026-07-18 | US-territory origin fix (150 rows → domestic with `AS`/`GU`/`VI`/`MP`/`PR`) |
| v2.0.3 | 2026-07-14 | Directory audit: −389 rows / −2 non-NCAA schools; conference labels web-verified |
| v2.0.2 / v2.0.1 | 2026-07-14/13 | Conference-label normalization; Louisiana Christian (NAIA) removal (−308) |
| v2.0 | 2026-07-08 | Initial public release (19 de-identified columns) |

| | |
|---|---|
| **Dataset (HF, canonical)** | https://huggingface.co/datasets/dharits3/ncaa-college-athlete-rosters-2025-26 |
| **DOI** | [10.57967/hf/9512](https://doi.org/10.57967/hf/9512) (unchanged — same repo) |
| **Kaggle mirror** | https://www.kaggle.com/datasets/shahdha/ncaa-all-sports-rosters-2025-26 |
| **GitHub** | https://github.com/Dharit13/NCAA_2025-26-Season-Dataset-Analysis ([releases](https://github.com/Dharit13/NCAA_2025-26-Season-Dataset-Analysis/releases)) |
| **License** | CC0 1.0 — citation requested; CC0 covers the compilation and grants **no NIL / publicity rights** |
| **Version** | **2.1.1** |

**Known intentional exceptions**

| Case | Behavior |
|---|---|
| **Independents** | `conference = Independent` is correct for full independents such as **Notre Dame** (football), **Maranatha Baptist**, and **Salem WV**, and for athletes on sport-specific independent schedules. |
| **Beloit ice hockey** | Two distinct athletes named Taylor share a base `athlete_id` hash (`_2`-suffixed second id) — a collision deliberately kept, not a duplicate. |

## Known limitations

- **WMT-platform schools** (Stanford, UCLA, Penn State) have partial bio-column
  coverage.
- Some Sidearm goalkeeper/pitching stat lines are rendered JS-only and are
  missing from the stats sidecars.
- Presto's combined **"Sacks-YDS"** football stat column ships unsplit.
- Track-family enrichment rejoin rate is 83.3–83.5% (the unmatched remainder in
  cross country / indoor / outdoor track has no row in the bio sidecar).
- Ice-hockey goalie minutes are unreliable; treat with caution.
- `track_indoor` / `track_outdoor` share source rows by design (see above).

## Provenance & quality

- Source: official school athletics-site rosters (Sidearm / Presto / WMT /
  school roster APIs / Wayback snapshots), scraped in-season for the 2025-26
  athletic year and validated against NCAA sport-sponsorship lists.
- v2.1.0 verification: every sport passed its internal integrity battery, then
  an adversarial live-page verification (multi-agent, ~25 stratified athletes
  per sport, ~3,000 field checks total — zero wrong-value findings; the only
  misses were under-capture, i.e. fields the pipeline left blank). Stats sports
  were additionally cross-checked against external record books. The per-sport
  verification log is maintained in the build repository.
- Raw and parsed fields ship together (`height_raw` + `height_in`,
  `weight_raw` + `weight_lbs`, `*_raw` + standardized) so parsing is auditable.
- PII audit clean for the shipped build.

## Citation

> Shah, Dharit (2026). *NCAA All Sports Rosters 2025-26: An Individual-Level
> Dataset Across All Divisions* (Version 2.1.1) [Data set]. Hugging Face.
> https://doi.org/10.57967/hf/9512

Machine-readable: [`CITATION.cff`](CITATION.cff).

## Governance

- [DATASHEET.md](DATASHEET.md) — Datasheets for Datasets
- [ETHICS_REVIEW.md](ETHICS_REVIEW.md) — ethics self-review / IRB exemption floor
- [DISCLOSURE_RISK.md](DISCLOSURE_RISK.md) — identifiability analysis, updated for the named release
- [LEGAL_NOTES.md](LEGAL_NOTES.md) — facts/CC0 rationale, scraping case law, FERPA/NIL notes
- [OPT_OUT.md](OPT_OUT.md) — per-record removal on request (dharits3@gmail.com,
  14-day target); removals ledger in [RELEASE_NOTES.md](RELEASE_NOTES.md)
