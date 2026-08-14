# Datasheet — NCAA All Sports Rosters 2025-26 (v2.1.0)

A datasheet (after Gebru et al., *Datasheets for Datasets*) for the cross-sport master
release: individual-level rosters for **all 28 NCAA championship + emerging sports**, all
three divisions, both genders, one athletic year (2025-26). Companion documents in this
release: [data/CODEBOOK.md](data/CODEBOOK.md), [by_sport/README.md](by_sport/README.md),
[OFFICIAL_COMPARISON.md](OFFICIAL_COMPARISON.md), [LICENSE](LICENSE),
[metadata.json](metadata.json), [MANIFEST.json](MANIFEST.json). The v2.0.5 governance
documents (ethics review, disclosure-risk analysis, legal notes, opt-out process) carry
forward unchanged and are distributed alongside the dataset repository.

All counts below are from the **build of 2026-08-14** (v2.1.0, `metadata.json`).

**What changed in v2.1.0.** The dataset is now **named**: `first_name` and `last_name`
are public columns. Six enrichment columns were added (`major`, `previous_school`,
`height_raw`/`height_in`, `weight_raw`/`weight_lbs`), and **10 sports gained per-sport
season-stats sidecar files** joined on `athlete_id`. Row count moved 514,696 → **513,655**
(−1,041 junk rows removed; see §4). The schema is now **27 columns** (the 19 v2.0.5
public columns + 8 new).

---

## 1. Motivation

**For what purpose was the dataset created?** No public individual-level dataset covers
NCAA athletes across every sport and division with **origin** information (hometown, high
school). Existing data are scattered, Division-I-biased, and name-centric. This dataset was
built to enable research on the **geography and socioeconomics of access to college
athletics** — talent pipelines, recruiting reach, prep-school channels, divisional
stratification — for the whole NCAA, in a single coherent year. It is also the only public
individual-level count of 2025-26 NCAA participation until the NCAA's own report lands
(~Sept 2026).

**Why is v2.1.0 a named release?** Two reasons. (1) Every value in the file is a
**school-published fact**: names appear on the same public, logged-out roster pages as
every other field, published by the athlete's own institution in a directory-like form.
The v2.0.x de-identified posture withheld names while shipping quasi-identifiers
(school + sport + position + class + hometown + high school) that already resolved most
rows to a unique athlete on the public source page pointed to by `source_url` — a
protection that was largely formal. (2) Names enable **external linkage** — to transfer
portals, professional drafts, record books, later seasons, and other public sports data —
which is where much of the research value of an athlete-level census lies. The
compensating controls are unchanged: no inferred attributes are distributed (§2, §6), use
conditions prohibit evaluation/ranking of individuals and NIL-adjacent commercial use of
identities (§5), and a standing per-record opt-out applies (§7).

**Who created the dataset and on behalf of which entity?** Dharit Shah
(dharits3@gmail.com), independent researcher. No institution, no team.

**Who funded the creation of the dataset?** No funding. Personal project; compute and
scraping ran on personal hardware.

**Any other comments?** The unit of release is the athletic year: every sport is the
2025-26 season, scraped in-season, so cross-sport comparisons are not confounded by
vintage.

---

## 2. Composition

**What do the instances represent?** One row = one athlete on one sport's 2025-26 roster,
as published on the athlete's own school's official athletics website. An athlete on two
sports' rosters (e.g. cross country + track) appears once per sport, with per-sport
`athlete_id`s — the same convention the NCAA uses in its participation reports.

**How many instances are there in total?** **513,655 rows** (Men 290,510 / Women 223,145)
across **1,087 schools** and 28 sports; D1 185,086 / D2 124,660 / D3 203,909. Origin:
domestic 459,068 / international 43,853 / unknown 10,734. Per-sport row counts are in
`metadata.json` and the coverage table below. One deliberate redundancy: **`track_indoor`
and `track_outdoor` share source rows** — schools publish one track & field roster; the
indoor sport is materialized from the shared scrape and independently reconciled against
the indoor sponsor lists (distinct `tfi_`/`tfo_` ids). Never dedupe across sports blindly.

**Does the dataset contain all possible instances or is it a sample?** It attempts the
census of published rosters. Coverage against the NCAA's own participation figures is
decomposed, not hand-waved, in [OFFICIAL_COMPARISON.md](OFFICIAL_COMPARISON.md): the gap
is missing small non-D1 team cells, published-roster vs reported-squad-size (compliance
counts include walk-ons and mid-season cuts never published on the website), and vintage.
Selection skews: missing teams concentrate in small non-D1 programs; unpublished squad
members (disproportionately walk-ons) are absent by construction.

**What data does each instance consist of?** **27 columns**: the 19 v2.0.5 public
columns (`athlete_id`, `sport`, `athletic_year`, `season`, `division`, `gender`,
`conference`, `school`, `position_raw`, `position_group`, `class_year_raw`,
`class_standing`, `hometown_raw`, `hometown_city`, `hometown_state`, `origin`,
`high_school`, `high_school_is_academy`, `source_url`) plus 8 new: `first_name`,
`last_name`, `major`, `previous_school`, `height_raw`, `height_in`, `weight_raw`,
`weight_lbs`. The physical column order is locked with the identity/bio columns
following `athlete_id` (see [samples/data_dictionary.csv](samples/data_dictionary.csv)
for the exact order); raw + parsed pairs ship together by policy
(`height_raw`/`height_in`, `weight_raw`/`weight_lbs`). Full semantics in
[data/CODEBOOK.md](data/CODEBOOK.md).

**Stats sidecars (new in v2.1.0).** 10 sports additionally ship a per-sport season-stats
file `by_sport/<sport>/stats.{parquet,csv}` with **sport-specific columns by design** —
stat vocabularies differ per sport and are never merged into one wide file. Sidecars join
to the main file on `athlete_id`. **205,132 athletes (39.9%) have ≥1 stat row.** See
[by_sport/README.md](by_sport/README.md) and each sport's `by_sport/<sport>/CODEBOOK.md`.

**Per-sport coverage** (share of rows non-null; from
[samples/sport_summary.csv](samples/sport_summary.csv)):

| Sport | Athletes | height % | weight % | major % | prev. school % | Stats rows |
|---|---:|---:|---:|---:|---:|---:|
| acro_tumbling | 1,099 | 55.4 | 0.0 | 33.2 | 10.2 | — |
| baseball | 38,033 | 90.1 | 76.8 | 32.3 | 33.8 | 27,470 |
| basketball | 32,614 | 89.0 | 36.8 | 31.3 | 30.8 | 26,818 |
| beach_volleyball | 1,678 | 81.3 | 0.0 | 27.2 | 31.6 | — |
| bowling | 804 | 39.1 | 0.0 | 34.0 | 15.0 | — |
| cross_country | 28,178 | 30.2 | 5.5 | 30.9 | 13.4 | — |
| equestrian | 1,017 | 26.3 | 0.0 | 36.2 | 14.6 | — |
| fencing | 1,212 | 42.6 | 9.2 | 28.5 | 0.4 | — |
| field_hockey | 6,300 | 79.8 | 0.0 | 44.4 | 11.7 | 5,439 |
| football | 72,512 | 90.3 | 89.3 | 25.7 | 23.7 | 34,982 |
| golf | 13,447 | 38.8 | 8.7 | 28.7 | 19.3 | — |
| gymnastics | 2,031 | 53.7 | 1.4 | 21.8 | 17.6 | — |
| ice_hockey | 7,529 | 87.2 | 44.8 | 14.7 | 35.8 | 6,345 |
| lacrosse | 29,221 | 86.3 | 41.4 | 39.6 | 13.5 | 24,789 |
| rowing | 5,763 | 42.7 | 0.6 | 26.8 | 4.8 | — |
| rugby | 824 | 57.4 | 0.0 | 17.6 | 4.5 | — |
| skiing | 500 | 21.2 | 2.8 | 25.8 | 14.6 | — |
| soccer | 54,537 | 84.9 | 27.4 | 36.8 | 24.7 | 42,750 |
| softball | 20,661 | 80.6 | 0.0 | 34.9 | 22.4 | 17,160 |
| stunt | 1,046 | 33.6 | 0.0 | 25.4 | 25.5 | — |
| swimming | 20,808 | 33.6 | 5.0 | 30.3 | 9.5 | — |
| tennis | 14,019 | 51.1 | 8.5 | 32.6 | 17.2 | — |
| track_indoor | 60,816 | 24.1 | 4.7 | 27.2 | 11.7 | — |
| track_outdoor | 64,231 | 24.9 | 4.9 | 27.9 | 12.3 | — |
| triathlon | 321 | 38.3 | 0.0 | 29.6 | 7.5 | — |
| volleyball | 21,394 | 90.2 | 4.4 | 37.3 | 22.5 | 17,767 |
| water_polo | 2,333 | 71.6 | 23.0 | 40.3 | 17.6 | 1,612 |
| wrestling | 10,727 | 33.5 | 49.6 | 33.4 | 15.1 | — |

Overall: `first_name` 100%, `last_name` 99.5% (2,542 mononym athletes),
`height_in` 62.2%, `weight_lbs` 30.0%, `major` 30.9%,
`previous_school` 19.5%, `high_school` 92.5%.

**Is there a label or target associated with each instance?** No. This is an
observational roster dataset, not a prediction task.

**Is any information missing from individual instances?** Yes, wherever the source page
omitted it — see the missingness section below, which governs interpretation of every
coverage figure in this document. Missing values are empty strings in CSV, typed nulls in
Parquet.

**Are relationships between individual instances made explicit?** `athlete_id` joins each
row to its sport's stats sidecar. Cross-sport identity of multi-sport athletes is **not**
linked (ids are per-sport); with names now public, users can construct such links
themselves, subject to the use conditions in §5.

**Are there recommended data splits?** No task splits. Natural analysis strata (`sport`,
`division`, `gender`) are explicit columns; `by_sport/` carries per-gender and
per-division CSV splits.

**Are there any errors, sources of noise, or redundancies?** Yes: (1) the track
indoor/outdoor shared-row design above; (2) roster-page noise — schools vary in position
vocabularies, hometown formatting, and class notation; raw fields are preserved alongside
standardized ones; (3) `previous_school` is source-labeled and heterogeneous (college /
junior or club team / high school, depending on the school — see the codebook); (4) one
deliberate id collision is retained: `ih_1d6230318867` and its `_2` suffix at Beloit are
two **different** ice-hockey athletes who share a name (goalie Jr / defense Sr) — an id
collision, not a duplicate; (5) known limitations: WMT-platform schools partially covered
(Stanford/UCLA/Penn State bio pages); Sidearm goalkeeper/pitching stat lines that render
only via JS are not captured everywhere; Presto combined "Sacks-YDS" football columns are
left unsplit; the track-family
(indoor/outdoor/XC) name-bridge rejoin rate is 83.3–83.5% (adjudicated; remainder
enrichment left null); ice-hockey goalie `minutes` is unreliable.

**Is the dataset self-contained, or does it link to external resources?** Self-contained.
`source_url` points to the originating roster page (live, Wayback snapshot, or school
roster API) for provenance; those pages may change or disappear, which does not affect
the data.

**Does the dataset contain data that might be considered confidential?** No. Every field
was published by the athlete's institution on its public, logged-out athletics website.

**Does the dataset contain data that might be offensive, insulting, threatening, or
otherwise cause anxiety?** No.

**Does the dataset identify any subpopulations?** By `gender` (the team's competition
gender, as published) and `origin` (domestic/international, derived from the published
hometown). Age is not present; `class_standing` is academic, not demographic.

**Is it possible to identify individuals from the dataset?** **Yes — directly. v2.1.0 is
a named release.** `first_name`/`last_name` are public columns, as published by each
athlete's institution. This replaces the v2.0.x de-identified posture, whose
quasi-identifiers already resolved most rows to a unique athlete on the public source
roster (see §1 for the rationale). What the dataset adds is aggregation and linkability,
not new facts about any individual; the use conditions in §5 and the opt-out in §7 are
the corresponding controls.

**Does the dataset contain data that might be considered sensitive in any way?** **Not in
the distributed files.** No race/ethnicity, religion, health, financial, biometric, or
contact data. Locked governance policy: **BISG race/ethnicity predictions, income/SES,
Census-tract, and mobility-report-card joins are research-only and appear in no
distributed file.** Published race/ethnicity results are aggregates with N<10
suppression. A PII audit of every staged file in this release is clean.

---

## 2a. Missingness is a publication convention

**Schools choose what to publish.** A null in this dataset ordinarily means the school
did not publish the field, not that the scrape missed it. The clearest case is women's
weight: **many schools do not publish weights for women's teams** — softball 0%, field
hockey 0%, volleyball ~4% — so near-zero weight coverage in those sports is a fact about
publication norms, not scrape failure. The same logic runs the other way: football
publishes weight almost universally (89.3%), and wrestling publishes weight (49.6%) more
readily than height (33.5%) because athletes compete by weight class. Roster-card sports
(football, baseball, basketball, hockey) publish height routinely; time-based sports
(track, swimming, cross country) mostly do not. `major` and `previous_school` appear only
where a school's roster template includes them.

Consequences for users: **columns are never dropped for low coverage** — per-sport rates
are documented instead ([samples/sport_summary.csv](samples/sport_summary.csv)); coverage
comparisons across sports or genders measure publication conventions before they measure
anything about athletes; and modeling missingness as random is wrong — it is structured
by sport, gender, division, and website platform.

---

## 3. Collection process

**How was the data associated with each instance acquired?** Scraped from each school's
official athletics website — directly observable, institution-published data; nothing
reported by or inferred about the athletes themselves. v2.1.0 layers a **re-scrape of
school-published roster, bio, and stats pages (June–August 2026)** on top of the v2.0
roster scrape: the v2.0 row universe was retained (minus the junk rows of §4) and each
sport's rosters were re-visited to capture names, bio fields (height, weight, major,
previous school), and season statistics. Sources were server-rendered CMS pages
(predominantly Sidearm Sports and PrestoSports), school roster APIs, and season-verified
Wayback Machine snapshots. Every row carries its `source_url`.

**What mechanisms or procedures were used to collect the data, and how were they
validated?** The v2.0 pipeline (athletics-domain resolution; season-pinned scraper with
season gates, school-identity checks, and gender disambiguation; recovery ladder;
reconciliation against the official NCAA per-sport sponsor lists) is described in the
v2.0.5 datasheet and remains the foundation. The v2.1.0 enrichment was verified per
sport, and **every one of the 28 sports was signed off** with three independent layers:

1. **Internal battery** — structural, range, and cross-field checks (id uniqueness and
   join integrity, height/weight plausibility ranges, raw↔parsed agreement, per-sport
   stat-column constraints).
2. **Adversarial live-page verification** — for each sport, 9–11 independent agents
   re-scraped ~25 stratified athletes against the live school pages and compared
   field-by-field; **~3,000 field-level checks total across sports, with zero
   wrong-value findings in signed-off sports** — all discrepancies found were
   under-capture (a published value the pipeline missed), never a wrong captured value.
3. **External record-book cross-checks** — for each stats sport, 5–6 athletes checked
   against independent sources (e.g. collegehockeyinc.com, NCAA record books).

Full per-sport log: `docs/enrichment/VERIFICATION_LOG.md` in the research repository.
The roster universe itself was benchmarked against the NCAA's published participation
report ([OFFICIAL_COMPARISON.md](OFFICIAL_COMPARISON.md)).

**If the dataset is a sample, what was the sampling strategy?** Not a sample by design —
attempted census of the official sponsor lists. Non-capture is availability-driven, not
sampled.

**Who was involved in the data collection process and how were they compensated?** The
author alone, unpaid, using LLM-assisted tooling (Claude Code) to write and run the
pipeline.

**Over what timeframe was the data collected?** Rosters in-season across the 2025-26
athletic year (fall 2025 – spring 2026, recovery through July 2026); enrichment and stats
re-scrape June–August 2026. The data are point-in-time roster snapshots plus
season-final statistics, not season-cumulative compliance counts.

**Were any ethical review processes conducted?** No institutional IRB (independent
researcher; public-data research of this kind is in any case exempt under 45 CFR
46.104(d)(4)(i)). A documented self-review against Zimmer's (2010) four concerns
(consent, identifiability, harm, "already public") was conducted for v2.0 and revisited
for the named release; it shapes the governance policy in §2 and §6 and the opt-out in
§7.

**Did you collect the data from the individuals directly, or obtain it via third
parties?** Via third parties: the athletes' own institutions, which publish rosters on
their official public websites. Scraping was logged-out access to public pages (the
posture US courts have held outside the CFAA — *hiQ v. LinkedIn* (9th Cir. 2022), *Meta
v. Bright Data* (2024)).

**Were the individuals notified about the data collection?** No. The athletes were not
notified. The data are facts their institutions had already published about them in a
directory-like form.

**Did the individuals consent to the collection and use of their data?** No consent was
sought or obtained. Athletes typically consent to their school's publicity practices,
not to third-party aggregation. This is stated plainly rather than excused; the
compensating controls are the non-distribution of inferred attributes, the use
conditions of §5, and the standing removal process of §7.

**If consent was obtained, was a mechanism provided to revoke it?** N/A for consent, but
an **opt-out/removal mechanism exists regardless**: any athlete (or their representative)
can request removal of their row(s) via dharits3@gmail.com, honored in the next published
version.

---

## 4. Preprocessing / cleaning / labeling

**Was any preprocessing/cleaning/labeling of the data done?** The v2.0 pipeline's
contamination filtering, deduplication, standardization, official-list reconciliation,
and PII sanitization are described in the v2.0.5 datasheet and carry forward. New in
v2.1.0:

- **Junk-row removal (−1,041 rows; 514,696 → 513,655).** 845 platform dual-render
  duplicate rows (identical athletes emitted twice by the "firehawk" template, carrying
  `_N`-suffixed `athlete_id`s) and 196 header-artifact rows (a column header such as
  `Ht.`/`Cl.`/`Wt.` scraped as an athlete name). No real athletes were removed.
- **Name repairs** (all inherited from the v2.0.x source data; every change logged in an
  internal ledger): **1,562** `'Last,'`/`'First'` comma-swap fixes; **407**
  jersey-number-in-`first_name` fixes (real name parsed out of `last_name`; 80
  height/weight values recovered from mashed UCLA baseball bio strings in the same
  pass); **2** trailing-comma strips; **1** manual fix (Alleda Hawron, Western New
  England track).
- **One deliberate keep:** `ih_1d6230318867`/`_2` at Beloit are two different athletes
  who share a name (goalie Jr / defense Sr) — an id collision retained, not a duplicate.
- **Null-literal cleanup** — **2,142** junk placeholder strings (`<NA>`, `nan`, `N/A`,
  `null`, `None`) replaced with true nulls: 2,129 `weight_raw` values (concentrated in
  acro & tumbling and STUNT), 12 `high_school`, 1 `hometown_city`. This is why those
  two sports' weight coverage reads 0.0%. Alongside the removed duplicate rows, **4**
  orphaned stats-sidecar rows (ids with no surviving roster row) were dropped.
- **Parsing** — `height_in` (float inches) and `weight_lbs` (integer pounds) parsed from
  the as-published `height_raw`/`weight_raw` strings, which are retained unchanged.

**Was the "raw" data saved in addition to the preprocessed data?** Yes — raw scrapes,
per-school CSVs, run logs, and the v2.1 fix ledger are preserved in the research repo,
enabling full reprocessing.

**Is the software used to preprocess/clean/label the data available?** Yes — the
pipeline is released with the project.

---

## 5. Uses

**Has the dataset been used for any tasks already?** Yes, by the author: talent-geography
and SES analyses per sport, and the official-vs-scraped participation benchmark
([OFFICIAL_COMPARISON.md](OFFICIAL_COMPARISON.md)).

**What (other) tasks could the dataset be used for?** Sports-economics and
sociology-of-sport research (access, stratification, Title IX-adjacent participation
analysis); geographic/migration analysis via `hometown_*`; institutional research via
`school` joins to IPEDS/Scorecard; a 2025-26 baseline for longitudinal roster studies;
and — newly practical with names — **record linkage** to transfer portals, drafts,
record books, and future seasons, plus roster-construction and performance analysis via
the stats sidecars.

**Is there anything about the composition or collection that might impact future uses?**
Yes: (1) roster snapshot ≠ compliance squad size ([OFFICIAL_COMPARISON.md](OFFICIAL_COMPARISON.md));
(2) track indoor/outdoor share source rows — pick a unit of analysis before cross-sport
dedup; (3) missing teams skew small/non-D1; (4) **missingness is a publication
convention** (§2a) — coverage differences are publication norms first; (5)
`previous_school` is source-labeled, not standardized; (6) position vocabularies are
sport-specific — use `position_group`; (7) stats sidecars are sport-specific by design —
do not union them.

**Are there tasks for which the dataset should not be used?** **Yes:** per-athlete
demographic inference (e.g. name- or geography-based race prediction — the reason BISG
outputs are never distributed); evaluation, ranking, recruiting, or any decision-making
about individual athletes; harassment or contact of athletes; and any commercial use of
athletes' identities (NIL-adjacent uses — the CC0 dedication covers the data compilation
and grants **no** name/image/likeness or publicity rights). These are stated as
conditions of use in the release documentation.

---

## 6. Distribution

**Will the dataset be distributed to third parties?** Yes — the 27-column named public
tier and the 10 per-sport stats sidecars, exactly as staged here. Research-tier
variables (BISG race predictions, income/SES, tract, mobility joins) are **never
distributed in any file** (locked policy). The PII audit of every staged file is clean.

**How will the dataset be distributed?** On the **Hugging Face Hub** (canonical; CSV +
Parquet, this documentation alongside), DOI **10.57967/hf/9512**, with a Kaggle mirror
and the build pipeline in the accompanying code repository.

**When will the dataset be distributed?** v2.1.0, built 2026-08-14.

**Will the dataset be distributed under a copyright or other IP license?** **CC0 1.0**,
with a **citation request** (not a legal condition). The contents are facts,
uncopyrightable in the US (*Feist v. Rural*). CC0 covers the data compilation only; it
grants no NIL/publicity rights in any athlete's identity. Citation: Shah, Dharit (2026).
*NCAA All Sports Rosters 2025-26: An Individual-Level Dataset Across All Divisions.*
[Data set]. doi:10.57967/hf/9512.

**Have any third parties imposed IP-based or other restrictions on the data?** No. The
underlying facts are not copyrightable; collection was logged-out scraping of public
pages; no terms were agreed to (no login, no clickwrap).

**Do any export controls or other regulatory restrictions apply?** None known. FERPA
binds institutions, not third-party researchers, and the fields mirror directory-type
information the institutions chose to publish; documented as an open policy question,
not a settled defense, in the v2.0.5 legal notes.

---

## 7. Maintenance

**Who will be supporting/hosting/maintaining the dataset?** The author (Dharit Shah),
via the Hugging Face repo and the project code repository.

**How can the owner/curator/manager be contacted?** dharits3@gmail.com, or issues on the
dataset's Hugging Face page.

**Is there an erratum?** Confirmed data errors, affected rows/versions, and the fixing
version are logged in the dataset repo's errata.

**Will the dataset be updated?** Yes. (1) Correction releases for errata and removal
requests, version-bumped with changelogs (this release, v2.1.0, is itself the named +
enriched successor to v2.0.5). (2) A planned 2026-27 reproduction using the released
pipeline, published as a separate season dataset — the DOI is version-scoped, so each
season/version remains citable as consumed.

**If the dataset relates to people, are there applicable limits on the retention of the
data?** No statutory limit, but a standing **per-record removal process**: any athlete
(or their representative) can request deletion of their rows (dharits3@gmail.com);
removals are applied in the next version via file commits (DOI-compatible) and logged in
the errata without naming the requester. This matters more, not less, in a named
release.

**Will older versions continue to be supported/hosted/maintained?** Hugging Face
preserves version history via git revisions; superseded versions remain accessible for
reproducibility, except that rows removed by opt-out are removed from the tip and noted
in errata.

**If others want to extend/augment/build on the dataset, is there a mechanism?** Yes —
CC0 permits anything; the full pipeline is released for reproduction or extension to
future seasons; derived-work authors are asked to honor the §5 use conditions and N<10
suppression for small demographic cells. Contributions/corrections via Hugging Face
discussions or email.
