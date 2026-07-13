# CODEBOOK — Ice Hockey (NCAA 2025-26)

Per-sport slice of the NCAA All-Sports 2025-26 public dataset. Same 19-column public (de-identified, no names) schema as the master release. All files here are subsets of `data/ncaa_all_sports_rosters_2025-26.csv` — identical columns, filtered to Ice Hockey.

## Files in this folder

| File | Scope | Rows |
|---|---|---|
| `all.csv` | all divisions, all genders | 7,535 |
| `men/all.csv` | men, all divisions | 4,483 |
| `men/d1.csv` | men, D1 | 1,729 |
| `men/d2.csv` | men, D2 | 206 |
| `men/d3.csv` | men, D3 | 2,548 |
| `women/all.csv` | women, all divisions | 3,052 |
| `women/d1.csv` | women, D1 | 987 |
| `women/d2.csv` | women, D2 | 124 |
| `women/d3.csv` | women, D3 | 1,941 |

Genders present: men, women.

## Columns (19)

| Column | Definition |
|---|---|
| `athlete_id` | Stable de-identified row id (per sport). Not a person id across sports. |
| `sport` | Sport key — constant within this folder (e.g. "basketball"). |
| `athletic_year` | Athletic year — constant "2025-26" on every row. |
| `season` | Sport's published season label (2025 fall / 2026 spring / 2025-26). |
| `division` | NCAA division: D1 / D2 / D3. |
| `gender` | Men / Women. |
| `conference` | Athletic conference (as published; ~68% populated). |
| `school` | Institution name (join key to IPEDS/Scorecard). |
| `position_raw` | Position string as published by the school. |
| `position_group` | Standardized position bucket. |
| `class_year_raw` | Class/eligibility string as published. |
| `class_standing` | Standardized class: Fr/So/Jr/Sr/Gr/UNK. |
| `hometown_raw` | Hometown string as published. |
| `hometown_city` | Parsed hometown city (~97.8%). |
| `hometown_state` | Parsed US state (domestic only; ~87.9%). |
| `origin` | domestic / international / unknown. |
| `high_school` | High school as published (~92.6%). |
| `high_school_is_academy` | Legacy binary academy flag (SUPERSEDED by the analysis-tier hs_category; see top-level CODEBOOK). |
| `source_url` | URL of the roster page scraped. |

## Notes
- **De-identified.** No athlete names or contact info. `hometown_*` and `high_school` are quasi-identifiers retained for research value — do not attempt re-identification.
- `high_school_is_academy` is the legacy flag; the richer public/private/religious classification lives in the research-only analysis tier (not distributed).
- **Do not sum `track_indoor` + `track_outdoor`** — they share source rows by design.
- Full project-level data dictionary and provenance: top-level `data/CODEBOOK.md`.

_Generated 2026-07-13 from the conference/division-corrected build (515,085 rows). Row counts above are exact for this build._
