# CODEBOOK — Field Hockey (NCAA 2025-26)

Per-sport slice of the NCAA All-Sports 2025-26 public dataset. Same 19-column public (de-identified, no names) schema as the master release. All files here are subsets of `data/ncaa_all_sports_rosters_2025-26.csv` — identical columns, filtered to Field Hockey.

## Files in this folder

| File | Scope | Rows |
|---|---|---:|
| `all.csv` | all divisions, all genders | 6,305 |
| `women/all.csv` | women, all divisions | 6,305 |
| `women/d1.csv` | women, D1 | 1,973 |
| `women/d2.csv` | women, D2 | 789 |
| `women/d3.csv` | women, D3 | 3,543 |

Genders present: women.

## Columns (19)

| Column | Definition |
|---|---|
| `athlete_id` | Stable de-identified row id (per sport). Not a person id across sports. |
| `sport` | Sport key — constant within this folder. |
| `athletic_year` | `2025-26` for every row. |
| `season` | Sport's own season label. |
| `division` | `D1` / `D2` / `D3`. |
| `gender` | `Men` / `Women`. |
| `conference` | Athletic conference (fully populated; `Independent` intentional where applicable). |
| `school` | Institution short name. |
| `position_raw` | Position/event as listed. |
| `position_group` | Standardized position group. |
| `class_year_raw` | Class as listed. |
| `class_standing` | Standardized class standing. |
| `hometown_raw` | Hometown as listed. |
| `hometown_city` | Parsed city. |
| `hometown_state` | USPS state/territory (US athletes incl. PR/VI/GU/AS/MP). |
| `origin` | `domestic` / `international` / `unknown`. US territories are domestic. |
| `high_school` | High school as listed. |
| `high_school_is_academy` | Legacy academy flag. |
| `source_url` | Source roster URL. |


_Generated 2026-07-18 from the v2.0.4 territory-origin fix build._
