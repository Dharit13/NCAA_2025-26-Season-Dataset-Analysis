# Codebook & Data Dictionary — NCAA All-Sports Rosters 2025-26

**Version 2.0.4** (enriched build). One row per athlete per sport-roster; 2025-26 athletic year; all 28 NCAA championship sports; D1/D2/D3; both genders. **514,696 rows.**

The release has two tiers:
- **Public tier** — `data/ncaa_all_sports_rosters_2025-26.csv` (19 columns, de-identified, no names).
- **Analysis tier** — `analysis/ncaa_all_sports_analysis_dataset.csv` (54 columns; adds Census income/population, College Scorecard, Opportunity Insights mobility, high-school typology, and validated town income). Joins to the public tier on `athlete_id`.

Each CSV has a Parquet twin with identical data (typed nulls instead of empty strings).

---

## Level-of-analysis rule (read first)
Income, population, school-type, and mobility fields describe the athlete's **hometown or high school or destination school — never the athlete's own family.** A town median is not a household income; a school type is a property of the school. All socioeconomic analysis must stay at the place/school level. Do **not** join income to individual athletes as if it were their family income (ecological fallacy).

---

## Tier 1 — Public (19 columns)

| Column | Type | Description |
|---|---|---|
| `athlete_id` | string | Stable surrogate key, sport-prefixed. Globally unique; joins to analysis tier. |
| `sport` | string | Sport registry key (`basketball`, `track_indoor`, ...). |
| `athletic_year` | string | `2025-26` for every row. |
| `season` | string | Sport's own season label (`2025` fall, `2026` spring, `2025-26` winter/full-year). |
| `division` | string | `D1`/`D2`/`D3` per official NCAA sponsor list. |
| `gender` | string | `Women` / `Men` (team competition gender). |
| `conference` | string | Athletic conference (fully populated on the public tier; `Independent` is intentional for true independents / sport-specific independents). |
| `school` | string | Institution name (roster short name). |
| `position_raw` | string | Position/event/weight-class as listed; semantics vary by sport. |
| `position_group` | string | Sport-specific standardized grouping. |
| `class_year_raw` | string | Class as listed (`Fr.`, `R-So.`, `Gr.`). |
| `class_standing` | string | Standardized: `Fr`/`So`/`Jr`/`Sr`/`Gr`/`UNK`. |
| `hometown_raw` | string | Hometown as listed. |
| `hometown_city` | string | Parsed city (97.8%). |
| `hometown_state` | string | Parsed US state/territory, USPS 2-letter (US athletes incl. PR/VI/GU/AS/MP; ~88%). |
| `origin` | string | `domestic` (87.9% / 452,348) / `international` (9.9% / 51,210) / `unknown` (2.2% / 11,138). US territories are `domestic`. |
| `high_school` | string | High school / prep / academy as listed (92.6%). |
| `high_school_is_academy` | 0/1 | Legacy thin prep/academy flag. Superseded by analysis-tier `hs_category`. |
| `source_url` | string | Roster page / API endpoint the row came from. |

---

## Tier 2 — Analysis (54 columns)

Columns 1-43 carry the public roster fields plus the original enrichment; columns 44-54 (**marked NEW**) were added in this version. Coverage % is share of the 514,696 rows populated.

| Column | Type | Cov% | Source | Description |
|---|---|---:|---|---|
| `athlete_id` | string | 100 | internal | Stable surrogate key. Joins across all tiers. |
| `sport_key` | string | 100 | internal | Sport registry key (matches dataset_release/<sport>/). |
| `athletic_year` | string | 100 | internal | `2025-26` for every row. |
| `league` | string | 100 | internal | `NCAA` for every row. |
| `gender` | string | 100 | roster | `Women` / `Men` (team competition gender). |
| `d2` | string | 98 | internal | Legacy division tag; use `division` instead. |
| `conference` | string | 100 | roster | Athletic conference (fully populated; `Independent` is intentional where applicable). |
| `school_name` | string | 100 | roster | Institution name (roster short name). |
| `sport` | string | 100 | roster | Full sport name incl. gender (e.g. "Women's Basketball"). |
| `position` | string | 82 | roster | Position/event/weight-class as listed; semantics vary by sport. |
| `class_year` | string | 99 | roster | Class as listed (`Fr.`, `R-So.`, `Gr.`). |
| `hometown` | string | 98 | roster | Hometown as listed (raw). |
| `hometown_city` | string | 98 | roster (parsed) | Parsed city. |
| `hometown_state` | string | 96 | roster (parsed) | Parsed US state as published (mixed-format). |
| `high_school` | string | 93 | roster | High school / prep / academy as listed. |
| `source_url` | string | 100 | internal | Roster page / API endpoint the row came from. |
| `origin` | string | 100 | derived | `US` / `INTL` / `UNK` (aligned with public domestic/international/unknown: 87.9% / 9.9% / 2.2%). US territories are `US`. |
| `us_state` | string | 88 | derived | USPS 2-letter state/territory (incl. PR/VI/GU/AS/MP), US athletes only. |
| `intl_region` | string | 10 | derived | Country/region for international athletes (10%). |
| `position_std` | string | 100 | derived | Standardized position grouping. |
| `class_std` | string | 100 | derived | Standardized class: `Fr`/`So`/`Jr`/`Sr`/`Gr`/`UNK`. |
| `is_academy` | 0/1 | 100 | derived (legacy) | Legacy thin prep/academy flag. **SUPERSEDED** by `hs_category` — misfiled 44,767 religious-school athletes as non-academy. Kept for backward compatibility only. |
| `division` | string | 100 | derived | `D1`/`D2`/`D3` per official NCAA sponsor list. |
| `hometown_median_household_income` | float | 79 | Census (original) | Median household income of hometown (original enrichment; 78.6%). Retained; `town_census_income` is the validated replacement. |
| `hometown_population` | float | 79 | Census (original, flawed) | Hometown population (original). **MIS-SCALED for small towns** — use `town_census_population` instead. |
| `school_pct_pell` | float | 87 | College Scorecard | Share of school undergrads receiving Pell grants (0-1). |
| `school_net_price` | float | 87 | College Scorecard | Average annual net price ($). |
| `school_cost` | float | 87 | College Scorecard | Average annual cost of attendance ($). |
| `school_adm_rate` | float | 85 | College Scorecard | Undergraduate admission rate (0-1). |
| `school_tuition_out` | float | 87 | College Scorecard | Out-of-state tuition ($). |
| `school_med_faminc` | float | 87 | College Scorecard | Median family income of the school's students ($). |
| `school_med_earnings` | float | 87 | College Scorecard | Median earnings of alumni ($). |
| `mrc_par_median` | float | 78 | Opportunity Insights MRC | Median parent income of student body ($). |
| `mrc_par_q1` | float | 78 | Opportunity Insights MRC | Share of students from bottom income quintile (0-1). |
| `mrc_par_top10` | float | 78 | Opportunity Insights MRC | Share of students from top 10% (0-1). |
| `mrc_mobility_kq5pq1` | float | 78 | Opportunity Insights MRC | Bottom-to-top mobility rate P(child top quintile | parent bottom quintile). |
| `mrc_tier` | string | 78 | Opportunity Insights MRC | School selectivity/type tier (e.g. "Selective private"). |
| `conf_high_major` | float | 37 | derived | 1 = high-major conference (36.6%). |
| `school_state` | string | 88 | derived | State of the institution. |
| `in_state` | float | 78 | derived | 1 = athlete's state == school state. |
| `dist_mi` | float | 77 | derived | Recruiting distance hometown→school (miles). |
| `income` | float | 79 | Census (original) | Alias of hometown_median_household_income (legacy). |
| `income_band` | string | 79 | derived | `low`/`mid`/`high` income band (legacy). |
| `hs_category` | string | 100 | NCES CCD+PSS (this version) | **NEW.** High-school type: `public` / `private_religious` / `private_secular` / `private_prep_academy` / `homeschool` / `international` / `no_hs_listed` / `unknown`. 100% populated (buckets included). |
| `hs_category_detailed` | string | 84 | NCES CCD+PSS | **NEW.** Finer type incl. `private_catholic` / `private_other_religious` / `private_nonsectarian` / name-pattern variants (83.7%). |
| `hs_religious_affiliation` | string | 10 | NCES PSS | **NEW.** `catholic` / `other_religious` / `nonsectarian` where matched to PSS (10.5%). |
| `hs_type_confidence` | string | 84 | derived | **NEW.** `high` (score≥100) / `medium` (85-99) / `name_pattern` / `none`. |
| `hs_match_source` | string | 84 | derived | **NEW.** `CCD` (public census) / `PSS` (private census) / name-pattern. |
| `hs_match_score` | float | 84 | derived | **NEW.** Fuzzy match score 0-100 to NCES record. |
| `town_census_income` | float | 79 | Census ACS 2022 | **NEW.** Validated town median household income ($; 79.1%). Replaces hometown_median_household_income. r=0.97 with original. |
| `town_census_population` | float | 79 | Census ACS 2022 | **NEW.** Correctly-scaled town population (79.1%). Replaces hometown_population. |
| `town_census_place` | string | 79 | Census ACS 2022 | **NEW.** Matched Census place/county-subdivision name. |
| `town_income_match_type` | string | 79 | derived | **NEW.** `exact` / `fuzzy` town-name match to Census. |
| `town_athletes_per_10k` | float | 79 | derived | **NEW.** NCAA athletes per 10,000 town residents (production rate). |

---

## High-school classification (new)
Built from two federal censuses: **NCES CCD** (public-school universe, 2021) and **NCES PSS** (private-school universe 2021-22, carries religious affiliation). Athlete high schools were fuzzy-matched on (name, state) with a distinctive-token guard; threshold combined-score ≥ 85 (validated 49/50 ≈ 98% precision). Athlete-level distribution:

| hs_category | Athletes | % |
|---|---:|---:|
| public | 321,362 | 62.4% |
| private_religious | 53,177 | 10.3% |
| international | 43,252 | 8.4% |
| no_hs_listed | 38,148 | 7.4% |
| unknown | 38,017 | 7.4% |
| private_secular | 11,842 | 2.3% |
| private_prep_academy | 8,066 | 1.6% |
| homeschool | 1,529 | 0.3% |

Resolved to a concrete school type (public/private_*): **394,447 (76.5%)**. The remainder is international (8.4%), no high school listed (7.4%), unmatched US (7.4%), homeschool (0.3%). `is_academy` (legacy) is retained but superseded — it classified 44,767 religious-school athletes as non-academy. Full method: `HS_CLASSIFICATION_METHODS.md`.

## Validated town income (new)
`town_census_income` / `town_census_population` come from Census ACS 2022 5-year (B19013 income, B01003 population) at place level, with county-subdivision level for CT/ME/MA/NH/RI/VT where towns are the real unit. This **replaces** the original `hometown_population`, which was mis-scaled for small towns (e.g. Darien CT showed 1,242 vs actual 21,571). New income correlates r=0.97 with the original income field. A variance decomposition (`INCOME_PROXY_VALIDATION.md`) shows the town median captures 98-100% of ZIP-level income variation for towns under 50k. Match: 16,430 hometowns (mapping to 10,053 Census places) / 407,664 athletes; `town_income_match_type` flags exact vs fuzzy.

## Notes
- **Cross-sport counting:** `track_indoor` ∩ `track_outdoor` share source rows by design (distinct `tfi_`/`tfo_` ids). Decide your unit of analysis before deduping.
- **De-identified, not anonymized.** No names; `hometown_*`/`high_school` are quasi-identifiers retained for research value. Do not attempt re-identification.
- **Vintages:** rosters 2025-26; Census ACS 2022; NCES CCD 2021 / PSS 2021-22; College Scorecard & Opportunity Insights as in the original enrichment. Charter schools are counted public.
- **Per-sport semantics** live in each `dataset_release/<sport>/data/CODEBOOK.md`.