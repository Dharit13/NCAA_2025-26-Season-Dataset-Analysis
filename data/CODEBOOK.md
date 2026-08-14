# Codebook & Data Dictionary — NCAA All-Sports Rosters 2025-26

**Version 2.1.0** (named + enriched build, 2026-08-14). One row per athlete per
sport-roster; 2025-26 athletic year; all 28 NCAA championship + emerging sports;
D1/D2/D3; both genders. **513,655 rows, 27 columns.**

The release has two parts:

- **Combined file** — `data/ncaa_all_sports_rosters_2025-26_enriched.{csv,parquet}`
  (27 columns, all sports, **named**: `first_name`/`last_name` are public in v2.1).
- **Stats sidecars** — 10 sports carry a separate per-sport season-stats file
  `by_sport/<sport>/stats.{parquet,csv}` with **sport-specific columns by design**,
  joined on `athlete_id` (see "Stats-sidecar architecture" below).

Each CSV has a Parquet twin with identical data (typed nulls instead of empty strings).
Per-sport slices and per-gender/division CSV splits live under
[../by_sport/](../by_sport/README.md). Machine-readable column list with coverage:
[../samples/data_dictionary.csv](../samples/data_dictionary.csv). Release context:
[../DATASHEET.md](../DATASHEET.md).

---

## Read first

- **Missingness is a publication convention.** Schools choose what to publish; a null
  means the school did not publish the field, not that the scrape missed it. Women's
  weight is ~0% in many sports because schools do not publish women's weights (softball
  0%, field hockey 0%, volleyball ~4%). Columns are never dropped for low coverage;
  per-sport rates are in [../samples/sport_summary.csv](../samples/sport_summary.csv).
- **Raw + parsed pairs ship together by policy.** `height_raw`/`height_in` and
  `weight_raw`/`weight_lbs`: the raw column is the as-published string, the parsed
  column is derived from it and is null where the string could not be parsed.
- **Cross-sport counting:** `track_indoor` and `track_outdoor` share source rows by
  design (distinct `tfi_`/`tfo_` ids). Decide your unit of analysis before deduping.
- **Named release.** Names are school-published facts from public roster pages. Do not
  use the data to evaluate, rank, or make decisions about individual athletes, or for
  any commercial use of athletes' identities (no NIL/publicity rights are granted).

---

## Combined file — 27 columns

Column order is locked: `athlete_id`, the 8 v2.1 identity/bio columns, then the 18
remaining v2.0.5 roster columns unchanged. Coverage % = share of the 513,655 rows
non-null (authoritative values from `samples/data_dictionary.csv`).

### Identity & bio (new in v2.1)

| Column | Type | Cov% | Description |
|---|---|---:|---|
| `athlete_id` | string | 100.0 | Stable surrogate key, sport-prefixed. Globally unique; joins to the per-sport stats files. One deliberate collision-suffix survivor: `ih_1d6230318867`/`_2` at Beloit are two *different* athletes who share a name (goalie Jr / defense Sr) — an id collision, not a duplicate. |
| `first_name` | string | 100.0 | First/given name as school-published (public in v2.1). May be a nickname or initials where that is what the school publishes. |
| `last_name` | string | 99.5 | Last/family name as school-published. v2.1 repaired inherited scrape defects (1,562 comma-swaps, 407 jersey-number leaks, 2 trailing commas, 1 manual fix); all changes ledger-logged. |
| `major` | string | 30.9 | Academic major as listed on the roster/bio page, free text as published. Publication varies by school and platform; null means the school's template does not carry the field, not "undeclared". |
| `previous_school` | string | 19.5 | Previous school as listed — **source-labeled**: some schools list a college, some a junior/club team, some a high school. The value is reproduced as published; do not reinterpret it as a transfer flag or assume a level. |
| `height_raw` | string | 62.2 | Height exactly as published (e.g. `6-2`, `5'11"`). |
| `height_in` | float | 62.2 | Height parsed to inches from `height_raw`. Null where unpublished or unparseable. |
| `weight_raw` | string | 30.1 | Weight exactly as published (e.g. `185`, `185 lbs`). |
| `weight_lbs` | int | 30.0 | Weight parsed to integer pounds from `weight_raw`. Null where unpublished or unparseable (raw > parsed by 0.5 pp). |

### Roster columns (carried from v2.0.5)

| Column | Type | Cov% | Description |
|---|---|---:|---|
| `sport` | string | 100.0 | Sport registry key (`basketball`, `track_indoor`, ...). Indoor/outdoor track share source rows by design. |
| `athletic_year` | string | 100.0 | `2025-26` for every row. |
| `season` | string | 100.0 | Sport's own season label (`2025` fall, `2026` spring, `2025-26` winter/full-year). |
| `division` | string | 100.0 | `D1`/`D2`/`D3` per official NCAA sponsor list. |
| `gender` | string | 100.0 | `Women` / `Men` (team competition gender). |
| `conference` | string | 100.0 | Athletic conference, 2025-26 vintage; fully populated (`Independent` is intentional for true / sport-specific independents). |
| `school` | string | 100.0 | Institution name (roster short name). |
| `position_raw` | string | 81.6 | Position/event/weight-class as listed; semantics vary by sport. |
| `position_group` | string | 100.0 | Sport-specific standardized grouping (`UNK` where unlisted). |
| `class_year_raw` | string | 99.1 | Class as listed (`Fr.`, `R-So.`, `Gr.`). |
| `class_standing` | string | 100.0 | Standardized: `Fr`/`So`/`Jr`/`Sr`/`Gr`/`UNK`. |
| `hometown_raw` | string | 97.9 | Hometown as listed. |
| `hometown_city` | string | 97.9 | Parsed city. |
| `hometown_state` | string | 89.4 | Parsed US state/territory, USPS 2-letter (US athletes incl. PR/VI/GU/AS/MP). |
| `origin` | string | 100.0 | `domestic` (459,068) / `international` (43,853) / `unknown` (10,734). Roster-listed origin, **not** citizenship. US territories are `domestic`. |
| `high_school` | string | 92.5 | High school / prep / academy as listed (institution-published free text). |
| `high_school_is_academy` | 0/1 | 99.8 | Legacy thin prep/academy flag. |
| `source_url` | string | 100.0 | Roster page / API endpoint the row came from. |

---

## Stats-sidecar architecture (new in v2.1)

Ten sports ship a separate per-sport season-stats file. Stat columns are
**sport-specific by design** — a batting line, a goalie line, and a passing line do not
belong in one wide table — so the sidecars are **never merged into a single cross-sport
file**. Each sidecar holds one row per athlete with ≥1 published stat line, joined on
`athlete_id`:

```python
combined = pd.read_parquet("data/ncaa_all_sports_rosters_2025-26_enriched.parquet")
stats    = pd.read_parquet("by_sport/ice_hockey/stats.parquet")

df = combined.merge(stats, on="athlete_id", how="left")
```

(A left merge keeps roster athletes without stats — redshirts, scout team,
non-appearing reserves — with null stat columns. That is a real state, not an error.)

| Sport | Stats rows | Sidecar |
|---|---:|---|
| baseball | 27,470 | `by_sport/baseball/stats.{parquet,csv}` |
| basketball | 26,818 | `by_sport/basketball/stats.{parquet,csv}` |
| field_hockey | 5,439 | `by_sport/field_hockey/stats.{parquet,csv}` |
| football | 34,982 | `by_sport/football/stats.{parquet,csv}` |
| ice_hockey | 6,345 | `by_sport/ice_hockey/stats.{parquet,csv}` |
| lacrosse | 24,789 | `by_sport/lacrosse/stats.{parquet,csv}` |
| soccer | 42,750 | `by_sport/soccer/stats.{parquet,csv}` |
| softball | 17,160 | `by_sport/softball/stats.{parquet,csv}` |
| volleyball | 17,767 | `by_sport/volleyball/stats.{parquet,csv}` |
| water_polo | 1,612 | `by_sport/water_polo/stats.{parquet,csv}` |

Total: **205,132 athletes (39.9%) have ≥1 stat row.**

**Stat vocabularies.** Each sport's stat columns are defined in that sport's own
codebook, `by_sport/<sport>/CODEBOOK.md` (e.g.
[ice_hockey](../by_sport/ice_hockey/CODEBOOK.md): `gp, goals, assists, points,
plus_minus, minutes, goalie_*`; [baseball](../by_sport/baseball/CODEBOOK.md): batting +
pitching lines; [football](../by_sport/football/CODEBOOK.md): position-group passing /
rushing / receiving / defense / kicking columns). Sport-specific caveats (e.g. hockey
goalie `minutes` unreliable; Presto combined "Sacks-YDS" football cells unsplit; Sidearm
JS-rendered goalkeeper/pitching lines not captured everywhere) live in the same
per-sport codebooks and in [../DATASHEET.md](../DATASHEET.md).

---

## Notes

- **Verification.** All 28 sports signed off via internal battery + adversarial
  live-page re-scrape (~3,000 field-level checks, zero wrong-value findings) + external
  record-book cross-checks for stats sports. See [../DATASHEET.md](../DATASHEET.md) §3.
- **v2.1 row delta.** 514,696 → 513,655: −845 platform dual-render duplicate rows,
  −196 header-artifact rows. See [../DATASHEET.md](../DATASHEET.md) §4 and
  [../OFFICIAL_COMPARISON.md](../OFFICIAL_COMPARISON.md).
- **Governance.** Research-tier variables (BISG race predictions, income/SES, tract,
  mobility joins) appear in **no** distributed file. License CC0 1.0 (citation
  requested; no NIL/publicity grant). DOI 10.57967/hf/9512. Opt-out:
  dharits3@gmail.com.
- **Per-sport semantics** live in each `by_sport/<sport>/CODEBOOK.md`.
