# NCAA All-Sports Rosters 2025-26 — Release Notes

## v2.1.1 — 2026-08-14 (bio-sidecar split; same day as v2.1.0)

Structural change only — **no data values changed**. The six sparse bio
columns (`major`, `previous_school`, `height_raw`, `height_in`, `weight_raw`,
`weight_lbs`) moved out of the combined roster file into a compact sidecar:

- **Roster file** `data/ncaa_all_sports_rosters_2025-26_enriched.{parquet,csv}`
  — now **21 columns** (identity + team + hometown), 513,655 rows.
- **Bio sidecar** `data/ncaa_athlete_bio_2025-26.{parquet,csv}` — `athlete_id`
  + the six bio columns; **373,817 rows** (athletes with ≥1 published bio
  field; 72.8%). Within the sidecar the fields are much denser than they were
  in the wide file (height 85.5%, major 42.4%, weight 41.3%).
- Join on `athlete_id`; per-sport `by_sport/` slices carry the 21-column
  roster schema. Rationale: the bio fields are sparse by publication
  convention — splitting them keeps the roster file lean and makes the
  missingness structure explicit.

**Docs errata (2026-08-14, post-release):** the reconciliation notes atop the
28 `by_sport/*/CODEBOOK.md` files now state the post-split reality (21-column
slices; bio fields in the sidecar) — their sign-off bodies still describe the
pre-split 27-column layout. `OFFICIAL_COMPARISON.md` headline and per-sport
dataset counts refreshed to the shipped v2.1.1 files. No data files changed.

## v2.1.0 — 2026-08-14 (named + enriched)

The dataset is now **named and enriched**. Schema change: **19 → 27 columns**
(column order locked; the 19 v2.0.x columns are unchanged and keep their
semantics; as of v2.1.1 the six bio columns live in the bio sidecar and the
roster file carries 21). New per-sport **season-stats sidecar files** for 10 sports. The
consolidated file path **changes** to
`data/ncaa_all_sports_rosters_2025-26_enriched.parquet` (+ `.csv`); per-sport
slices remain `by_sport/<sport>/all.parquet`.

| | v2.0.5 | v2.1.0 |
|---|---:|---:|
| Athletes | 514,696 | **513,655** |
| Schools | 1,087 | **1,087** |
| Sports | 28 | **28** |
| Columns | 19 | **27** |
| Names | withheld | **public** |
| Season-stats files | — | **10 sports · 205,132 rows** |

v2.1.0 splits: Men 290,510 / Women 223,145 · D1 185,086 / D2 124,660 /
D3 203,909 · origin domestic 459,068 / international 43,853 / unknown 10,734.

### What's new

- **Names public.** `first_name` / `last_name` ship on every row. Rationale:
  names are school-published facts on public roster pages — publishing them
  makes every record verifiable against its `source_url` and linkable, while
  adding nothing beyond what the cited page prints.
- **8 new columns:** `first_name`, `last_name`, `major`, `previous_school`,
  `height_raw`/`height_in`, `weight_raw`/`weight_lbs`. Raw + parsed pairs ship
  together so parsing is auditable.
- **Season-stats sidecars** — `by_sport/<sport>/stats.{parquet,csv}`, columns
  sport-specific **by design**, joined to rosters on `athlete_id`. 205,132
  athletes (39.9%) have a stat row. Per-sport rows: soccer 42,750 · football
  34,982 · baseball 27,470 · basketball 26,818 · lacrosse 24,789 · volleyball
  17,767 · softball 17,160 · ice_hockey 6,345 · field_hockey 5,439 ·
  water_polo 1,612.
- **Coverage of the new columns:** first_name 100%, last_name 99.5% (2,542
  mononym athletes carry no last name), height 62.2%, weight 30.0%,
  major 30.9%, previous_school 19.5% (high_school carries over at 92.5%).
  **Missingness is a publication convention** — schools choose what their
  rosters print (women's weight is ~0% in many sports by convention: softball
  0%, field hockey 0%; football publishes it at 89.3%). Columns are never
  dropped for low coverage.

### Row-count change: 514,696 → 513,655 (−1,041 junk rows)

All inherited from the v2.0.x source data, found during enrichment:

| Class | Rows | What it was |
|---|---:|---|
| Dual-render duplicates | 845 | One platform ("firehawk" template) rendered rosters twice; the second render produced duplicate rows with `_N`-suffixed `athlete_id`s |
| Header artifacts | 196 | Table headers scraped as athletes (e.g. a `Ht.` column header as a name) |

### Fixed-rows ledger

All defects inherited from v2.0.x source data; every change is logged.
Name repairs total **1,972 rows** (comma-swaps + jersey-number fixes +
trailing commas + 1 manual); the null-literal class repairs non-name fields.
(`metadata.json` reports `fix_jersey_number: 487` because it counts the 80
recovered height/weight values as separate ledger entries alongside the 407
name fixes.)

| Fix class | Rows |
|---|---:|
| `"Last," / "First"` comma-swaps rejoined in correct order | 1,562 |
| Jersey number mashed into the name field, removed | 407 |
| — of which height/weight recovered from mashed UCLA baseball strings | +80 values |
| Junk null-literal strings (`<NA>`, `nan`, `N/A`, `null`) replaced with real nulls (mostly `weight_raw` in acro & tumbling / STUNT) | 2,142 |
| Trailing-comma strips | 2 |
| Manual correction (Alleda Hawron, WNE track) | 1 |

Additionally, 4 stats-sidecar rows whose `athlete_id` belonged to removed
duplicate rows were dropped from the sidecars (they have no roster row to
join to).

**One deliberate keep:** `ih_1d6230318867` / `ih_1d6230318867_2` (Beloit ice
hockey) are two **different** athletes named Taylor — an id-hash collision, not
a duplicate. Both rows stay.

### Verification

All 28 sports individually signed off before release:

- **Internal battery** per sport (id uniqueness, parity, coverage, label
  integrity).
- **Adversarial live-page workflow** per sport: 9–11 agents re-checking ~25
  stratified athletes per sport against live roster pages — ~3,000 field checks
  total, **zero wrong-value findings** (the only misses were under-capture:
  fields the pipeline left blank, never fields it got wrong).
- **External record-book cross-checks** for the stats sports.

The per-sport verification log is maintained in the build repository (not
shipped with the dataset).

### Known limitations

- WMT-platform schools (Stanford, UCLA, Penn State): partial bio-column coverage.
- Some Sidearm goalkeeper/pitching stat lines are JS-only and missing from the
  stats sidecars.
- Presto's combined "Sacks-YDS" football stat column ships unsplit.
- Track-family enrichment rejoin 83.3–83.5% (bio columns blank for the
  unmatched remainder in cross country / indoor / outdoor track).
- Ice-hockey goalie minutes unreliable.
- `track_indoor` / `track_outdoor` share source rows by design — never blindly
  dedupe across sports.

### Removals (opt-out ledger)

Per-record removal is honored on request, without question: email
**dharits3@gmail.com** (see `OPT_OUT.md`), target turnaround **14 days**.
Removals are committed to the data files and recorded here, per version.

*Empty as of 2026-08-14 — no removal requests received.*

### Files in this release

300 files (301 including `MANIFEST.json` itself), ~586 MB (v2.1.1 split trimmed
the redundant bio columns out of every roster file). Full per-file row counts
and sha256 checksums: `MANIFEST.json`.

### Citation

Shah, Dharit (2026). *NCAA All Sports Rosters 2025-26: An Individual-Level
Dataset Across All Divisions* (Version 2.1.0) [Data set]. Hugging Face.
https://doi.org/10.57967/hf/9512

Top level:

| Path | Rows |
|---|---:|
| `data/ncaa_all_sports_rosters_2025-26_enriched.parquet` | 513,655 |
| `data/ncaa_all_sports_rosters_2025-26_enriched.csv` | 513,655 |
| `samples/ncaa_rosters_sample_10000.csv` | 10,000 |
| `samples/school_sport_summary.csv` | 19,603 |
| `samples/sport_summary.csv` | 28 |
| `samples/data_dictionary.csv` | 27 |
| `metadata.json` · `MANIFEST.json` · `LICENSE` · `CITATION.cff` · `README.md` · `RELEASE_NOTES.md` · governance docs | — |

`by_sport/<sport>/` — every sport ships `CODEBOOK.md`, `all.csv`,
`all.parquet`, and per-gender division slices (`<gender>/all.csv`, `d1.csv`,
`d2.csv`, `d3.csv`; a division file exists only where the division fields teams
— e.g. no men's gymnastics `d2.csv`). The 10 stats sports add `stats.csv` +
`stats.parquet`. Per-sport roster rows sum to the consolidated 513,655.

| Sport | Genders | Roster rows | Stats rows |
|---|---|---:|---:|
| acro_tumbling | W | 1,099 | — |
| baseball | M | 38,033 | 27,470 |
| basketball | M+W | 32,614 | 26,818 |
| beach_volleyball | W | 1,678 | — |
| bowling | W | 804 | — |
| cross_country | M+W | 28,178 | — |
| equestrian | W | 1,017 | — |
| fencing | M+W | 1,212 | — |
| field_hockey | W | 6,300 | 5,439 |
| football | M | 72,512 | 34,982 |
| golf | M+W | 13,447 | — |
| gymnastics | M+W | 2,031 | — |
| ice_hockey | M+W | 7,529 | 6,345 |
| lacrosse | M+W | 29,221 | 24,789 |
| rowing | W | 5,763 | — |
| rugby | W | 824 | — |
| skiing | M+W | 500 | — |
| soccer | M+W | 54,537 | 42,750 |
| softball | W | 20,661 | 17,160 |
| stunt | W | 1,046 | — |
| swimming | M+W | 20,808 | — |
| tennis | M+W | 14,019 | — |
| track_indoor | M+W | 60,816 | — |
| track_outdoor | M+W | 64,231 | — |
| triathlon | W | 321 | — |
| volleyball | M+W | 21,394 | 17,767 |
| water_polo | M+W | 2,333 | 1,612 |
| wrestling | M+W | 10,727 | — |
| **Total** | | **513,655** | **205,132** |

### Unchanged

- School set (1,087) and sport set (28).
- The 19 v2.0.x columns and their semantics.
- Governance posture: the research tier (BISG race, income/SES, tract, mobility
  joins) is research-only, in no distributed file, never will be, and is not
  reconstructable from released fields. PII audit clean.
- License CC0 1.0 (citation requested; no NIL/publicity grant).
- DOI **10.57967/hf/9512** (same HF repo — cite with version **2.1.0**).

---

## v2.0.5 — 2026-08-06 (city-only roster origin fix)

Data-quality fix on the public **19-column** tier. **No rows added or removed**
(514,696), **no schema change**.

| | v2.0.4 | v2.0.5 |
|---|---:|---:|
| Athletes | 514,696 | **514,696** |
| Schools | 1,087 | **1,087** |
| Domestic | 452,348 | **459,317** |
| International | 51,210 | **44,241** |
| Unknown | 11,138 | **11,138** |

### What changed

- **False-international fix (6,969 public-tier rows):** the same `classify_origin`
  flaw behind the v2.0.4 territory fix also mislabeled US athletes at schools whose
  rosters print hometowns **city-only** ("Columbus" with no state), island-style
  ("Honolulu, O'ahu"), or as a "City, St. / High School" mash. Concentrated at ~15
  schools (SUNY Cortland, the five Ohio Athletic Conference schools, UW-Oshkosh,
  UW-La Crosse, Olivet, Eastern Connecticut State, Cal State LA / Dominguez Hills,
  Hawaii, East Carolina, Coastal Carolina, UNC Greensboro, and a long tail). Rows now
  `origin=domestic` with `hometown_state` resolved via Census gazetteer match against
  the school's state (falls back to unique national match / major-city map; 51 rows
  are domestic with state left blank where the state is not determinable).
- **Method:** gazetteer-driven detection over all 51,210 international rows; per-rule
  dispositions with country-name and foreign-capital exclusions; a 26-row stratified
  sample was verified against live roster pages (25 confirmed US towns, 1 refuted —
  which reverted the whole "bare country name at schools that print domestic hometowns
  with states" class, e.g. "Mexico" at Dickinson = the country, not Mexico, PA).
  Genuinely international clusters (Simon Fraser (BC), the Lincoln Missouri Jamaica
  track pipeline, District of Columbia's Caribbean rosters, etc.) are untouched.
  Full audit trail: `patch_false_international_manifest.json` /
  `false_intl_dispositions.csv` in the build tree.
- **`by_sport/`** slices and per-sport release CSVs patched from the same dispositions.

### Unchanged

- Row count, schema, de-identification posture.
- DOI **10.57967/hf/9512** (cite with version **2.0.5**).

---

## v2.0.4 — 2026-07-18 (territory origin + docs)

Data-quality fix on the public **19-column** tier. **No rows added or removed**
(514,696), **no schema change**.

| | v2.0.3 | v2.0.4 |
|---|---:|---:|
| Athletes | 514,696 | **514,696** |
| Schools | 1,087 | **1,087** |
| Domestic | 452,198 | **452,348** |
| International | 51,360 | **51,210** |

### What changed

- **US territory origin/state:** athletes whose `hometown_raw` clearly indicates
  American Samoa, Guam, U.S. Virgin Islands, Northern Mariana Islands, or Puerto Rico
  but shipped as `origin=international` with empty `hometown_state` are now
  `origin=domestic` with the matching USPS territory code (`AS`/`GU`/`VI`/`MP`/`PR`).
  **150** public-tier rows corrected. British Virgin Islands (incl. Tortola / Road Town /
  Anegada / Virgin Gorda) stay `international`.
- **`by_sport/`** convenience slices rebuilt from the corrected master.
- **CODEBOOK:** `conference` documentation updated to reflect full population (removed
  stale "~68% / weakest field" language left over from pre-v2.0.3 builds). Version header
  bumped to 2.0.4.

### Unchanged

- De-identification posture (no names).
- Public column set and semantics (territory codes were already in the USPS set; Puerto
  Rico was already mostly correct).
- DOI **10.57967/hf/9512** (cite with version **2.0.4**).

---

## v2.0.3 — 2026-07-14 (audit correction; data tip 2026-07-17)

Comprehensive dataset + join audit on the public **19-column** tier. Schools
cross-checked against the official NCAA directory and web-verified for the 2025-26
season. **Schema unchanged.**

| | v2.0.2 | v2.0.3 |
|---|---:|---:|
| Athletes | 515,085 | **514,696** |
| Schools | 1,089 | **1,087** |
| Public columns | 19 | 19 |

### Why −389 athletes / −2 schools

Removed out-of-scope institutions that were **not NCAA members competing under NCAA
sponsorship in 2025-26**:

| School | Reason | Rows |
|---|---|---:|
| Shawnee State | Competing as NAIA that year | 172 |
| Johnson & Wales University Charlotte | NCAA exploratory / USCAA | 217 |

**Kept** six NCAA provisional members (Jamestown, Roosevelt, Point Park, Carlow, Regent,
Middle Georgia State). Johnson & Wales **Providence** remains (NCAA).

### What else changed

- **Conference labels** web-verified for 2025-26, including Notre Dame (per-sport
  ACC / Big Ten hockey / Independent football), SUNY Brockport → Empire 8, SUNY Canton →
  SUNYAC, Upper Iowa → GLVC, Penn College → United East, Vermont State Lyndon → North
  Atlantic, Salve Regina → NEWMAC, Norwich → GNAC, Utica → Empire 8, Marywood → Atlantic
  East.
- **High-school recovery** on public `high_school` (source-page restores where blank);
  analysis-tier `hs_category` patches applied in the private research build (not shipped
  here).
- Prior v2.0.2 conference spelling normalizations retained.
- **`by_sport/`** convenience slices rebuilt from the corrected master (203 CSVs;
  gender×division leaves sum to 514,696).
- Conference is **fully populated** (Shawnee State's intentional blank rows are gone with
  the school).

### Unchanged

- De-identification posture (no names).
- Public column set and semantics.
- DOI **10.57967/hf/9512** (cite with version **2.0.3**).

---

## v2.0.2 — 2026-07-14 (conference label normalization)

Cosmetic conference-label normalization on the public **19-column** tier. **No rows added
or removed** (515,085), **no schema change**, **no other field changed** — only six
conference spellings from different source feeds were normalized to their canonical
official names:

| Was | Now | Rows |
|---|---|---:|
| `Old Dominion Athletic Conf.` | `Old Dominion Athletic Conference` | 6,112 |
| `Southern California Intercollegiate Athletic Conf.` | `Southern California Intercollegiate Athletic Conference` | 4,893 |
| `Southern Intercol. Ath. Conf.` | `Southern Intercollegiate Athletic Conference` | 4,083 |
| `Atlantic 10 conference` | `Atlantic 10 Conference` | 597 |
| `Sunbelt Conference` | `Sun Belt Conference` | 416 |
| `New England Wrestling Assn` | `New England Wrestling Association` | 28 |

`Southern Conference` (D1) and `Southern Athletic Association` (D3) are **left distinct** —
they are genuinely different leagues. DOI **10.57967/hf/9512** (cite with version **2.0.2**).

## v2.0.1 — 2026-07-13 (label correction)

Conference/division label corrections on the public **19-column** tier after a full
**2025-26** re-verify. **Schema unchanged.**

| | v2.0 | v2.0.1 |
|---|---:|---:|
| Athletes | 515,393 | **515,085** |
| Schools | 1,089 | 1,089 |
| Public columns | 19 | 19 |

### Why −308 athletes

**Louisiana Christian University** (NAIA) roster rows had been mixed into the Louisiana /
UL Lafayette scrape path via `lcwildcats.net`. Those **308** rows are removed. No other
schools were dropped for this release.

### What else changed

- **Conference labels** corrected for 2025-26 vintage: stale realignment names and
  core-collision / join errors filled or fixed. Conference is now populated for
  **≈99.97%** of rows (empty only where intentional — see below).
- **Division labels** corrected where mislabeled; public `division` is the authoritative
  D1/D2/D3 field.
- **`by_sport/`** convenience slices rebuilt from the corrected master (203 CSVs;
  gender×division leaves still sum to the full master).

### Known intentional exceptions (not bugs)

| Case | Notes |
|---|---|
| **Shawnee State** | Empty `conference` (172 rows). NCAA D2 member competing as NAIA in 2025-26 — no NCAA conference affiliation that year. *(Removed entirely in v2.0.3.)* |
| **Independents** | `Independent` is correct for full independents including **Notre Dame**, **Maranatha Baptist**, and **Salem WV**, and for athletes on sport-specific independent schedules. |

### Unchanged

- De-identification posture (no names).
- Public column set and semantics.
- DOI **10.57967/hf/9512** (cite with version **2.0.1**).

---

## v2.0 — 2026-07-08

Individual-level roster data for **all 28 NCAA championship/emerging sports**, all three
divisions (D1/D2/D3), both genders, athletic year **2025-26** — the first year under the
House v. NCAA settlement (roster limits, revenue sharing, NIL).

- **515,393 athletes** across **1,089 schools** (~91.8% of officially reported NCAA
  participations; 96.9% of sponsor-list teams; 98.5% of D1).
- **De-identified** (no names, no contact info). Every field is an institution-published
  roster fact from official college athletics sites, validated against NCAA sponsor lists.
- **Public tier:** 19 columns. **Analysis tier:** 54 columns (adds Census town income,
  high-school public/private/religious classification, College Scorecard / mobility joins) —
  research-only; not distributed in this GitHub tree.

### Canonical record & DOI

- DOI: **10.57967/hf/9512** — https://doi.org/10.57967/hf/9512
- Hugging Face (record of origin): https://huggingface.co/datasets/dharits3/ncaa-college-athlete-rosters-2025-26
- Kaggle (mirror): https://www.kaggle.com/datasets/shahdha/ncaa-all-sports-rosters-2025-26

### What's in v2.0

- High-school public/private/religious classification (NCES CCD + PSS; fuzzy (name, state)
  match, threshold ≥85, ~98% precision; resolved 394,447 / 76.5%).
- Census-validated town income & population (ACS 2022; r=0.97 with the original field;
  correctly re-scaled small-town population).

### License

CC0-1.0 (public domain). Roster facts are not copyrightable (Feist v. Rural). Please do not
attempt re-identification. Per-record removal honored on request: dharits3@gmail.com.

### Citation

Shah, Dharit (2026). *NCAA All Sports Rosters 2025-26: An Individual-Level Dataset Across All
Divisions* (Version 2.0) [Data set]. Hugging Face. https://doi.org/10.57967/hf/9512
