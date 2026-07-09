# Datasheet — NCAA All Sports Rosters 2025-26

A datasheet (after Gebru et al., *Datasheets for Datasets*) for the cross-sport master
release: individual-level rosters for **all 28 NCAA championship + emerging sports**, all
three divisions, both genders, one athletic year (2025-26). Companion documents:
[README.md](README.md), [data/CODEBOOK.md](data/CODEBOOK.md),
[OFFICIAL_COMPARISON.md](OFFICIAL_COMPARISON.md), [ETHICS_REVIEW.md](ETHICS_REVIEW.md),
[DISCLOSURE_RISK.md](DISCLOSURE_RISK.md), [LEGAL_NOTES.md](LEGAL_NOTES.md),
[OPT_OUT.md](OPT_OUT.md).

All counts below are from the **build of 2026-07-06** (`metadata.json`) and will be
**refreshed at the final build** — two recovery passes (pipeline-dropped athletes; missing
sponsor-cell sweep) are still landing small upward revisions.

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

**How many instances are there in total?** **515,393 rows** (Men 291,389 / Women 224,004)
across **1,087 schools** and 28 sports (final build 2026-07-08).
Per-sport row counts are in `metadata.json`. One deliberate redundancy:
**`track_indoor` and `track_outdoor` share source rows** — schools publish one track &
field roster; the indoor sport is materialized from the shared scrape and independently
reconciled against the indoor sponsor lists (1,571 overlapping school-gender teams,
distinct `tfi_`/`tfo_` ids). Never dedupe across sports blindly.

**Does the dataset contain all possible instances or is it a sample?** It attempts the
census of published rosters and holds **91.9% of the NCAA's reported athlete
participations, 96.9% of sponsor-list teams, 98.5% of Division I** (final build 2026-07-08;
counts final). The shortfall is decomposed, not hand-waved
([OFFICIAL_COMPARISON.md](OFFICIAL_COMPARISON.md)): (1) ~3.1% of sponsor-list team cells
missing (small D2/D3 and emerging-sport programs that publish no scrapeable roster);
(2) published-roster vs reported-squad-size — official figures are season-cumulative
compliance counts including walk-ons/scout-team/mid-season cuts who never appear on the
public website (at D1 football, ~1 in 5 reported participants is never published);
(3) vintage — the official baseline is 2024-25. **Selection skews:** missing teams
concentrate in small non-D1 programs, so D1 is overrepresented relative to the true
universe; unpublished squad members (disproportionately walk-ons) are absent by
construction.

**What data does each instance consist of?** 19 columns (public tier): `athlete_id`,
`sport`, `athletic_year`, `season`, `division`, `gender`, `conference`, `school`,
`position_raw`, `position_group`, `class_year_raw`, `class_standing`, `hometown_raw`,
`hometown_city`, `hometown_state`, `origin`, `high_school`, `high_school_is_academy`,
`source_url`. Full semantics in [data/CODEBOOK.md](data/CODEBOOK.md). An enriched
analysis tier (`analysis/`, 54 columns; **research-only, not distributed** — see §6)
adds hometown- and school-level SES/geography joined from public sources (Census ACS,
IPEDS, College Scorecard, Opportunity Insights MRC) — all merged variables are area- or
institution-level, none are per-athlete attributes. **v2.0** adds a high-school
public/private/religious/prep-academy classification (NCES CCD + PSS federal censuses;
`hs_category` and companions) and Census-validated town income & population (ACS 2022;
`town_census_income`/`town_census_population`), the latter replacing a previously
mis-scaled `hometown_population`. These too are place/school-level, never per-athlete.
The town-income proxy was validated by variance decomposition (captures 98-100% of
ZIP-level income variation for towns under 50k), and a coverage-bias audit confirmed the
athletes lacking usable geography are only ~3.7% poorer than those retained (bias
conservative). See `DATASET_RELEASE_NOTES.md`.

**Is there a label or target associated with each instance?** No. This is an
observational roster dataset, not a prediction task.

**Is any information missing from individual instances?** Yes, where the source roster
omitted it: field completeness varies by sport (hometown typically >95%, high school
~90-95%, position/class near-complete; per-sport figures in each
`dataset_release/<sport>/` datasheet). `hometown_state` is populated only for domestic
athletes. Missing values are empty strings in CSV, typed nulls in Parquet.

**Are relationships between individual instances made explicit?** Partially. `athlete_id`
joins each row to the per-sport restricted and analysis tiers. Cross-sport identity of
multi-sport athletes is **not** linked (ids are per-sport), except that the
indoor/outdoor track overlap is documented at the team level.

**Are there recommended data splits?** No task splits. Natural analysis strata
(`sport`, `division`, `gender`) are explicit columns; per-sport releases carry
per-division/gender splits.

**Are there any errors, sources of noise, or redundancies?** Yes: (1) the track
indoor/outdoor shared-row design above; (2) roster-page noise — schools vary in position
vocabularies (a few list height in `position_raw`), hometown formatting, and class
notation; raw fields are preserved alongside standardized ones; (3) residual
misparses survive at low rates despite an 11-check verification battery and a
label-integrity audit per sport; (4) `conference` is best-effort. Known sport-specific
limitations (skiing's unsplittable coed tail, STUNT's unpublished tail) are documented in
the per-sport codebooks.

**Is the dataset self-contained, or does it link to external resources?** Self-contained.
`source_url` points to the originating roster page (live, Wayback snapshot, or school
roster API) for provenance; those pages may change or disappear, which does not affect
the data. The analysis tier's external joins are baked in at build time.

**Does the dataset contain data that might be considered confidential?** No. Every field
was published by the athlete's institution on its public, logged-out athletics website.

**Does the dataset contain data that might be offensive, insulting, threatening, or
otherwise cause anxiety?** No.

**Does the dataset identify any subpopulations?** By `gender` (the team's competition
gender, as published) and `origin` (domestic/international, derived from the published
hometown). Age is not present; `class_standing` is academic, not demographic.

**Is it possible to identify individuals from the dataset?** **Yes, by linkage — this
dataset is de-identified, not anonymized.** Distributed files contain no names, contact
information, jersey numbers, or profile URLs. But a row's quasi-identifiers
(school + sport + position + class + hometown + high school) will often match exactly one
athlete on the corresponding public source roster, which remains online and is pointed to
by `source_url`. Re-identification recovers only what the school already published; it is
nonetheless a documented residual risk, `hometown_*`/`high_school` are retained
deliberately as research-critical quasi-identifiers, and users are asked not to attempt
re-identification. See [ETHICS_REVIEW.md](ETHICS_REVIEW.md).

**Does the dataset contain data that might be considered sensitive in any way?** **Not in
the distributed files.** No race/ethnicity, religion, health, financial, biometric, or
contact data. Two things exist upstream and are never distributed: (1) **named rosters**
live only in per-sport `restricted/` folders in the research repo, shareable solely under
a data-use agreement (the ICPSR two-tier model: public file without sensitive variables +
DUA-gated restricted file); there is no cross-sport named tier at all. (2) **BISG
race/ethnicity predictions** were generated for the research analysis; they are inferred
probabilistic attributes about individuals, so they are **research-only and are not
distributed in any tier**. Published race/ethnicity results are aggregates with **N<10
suppression**.

---

## 3. Collection process

**How was the data associated with each instance acquired?** Scraped from each school's
official athletics-site roster page — directly observable, institution-published data;
nothing reported by or inferred about the athletes themselves. Sources were
server-rendered CMS pages (predominantly Sidearm Sports and PrestoSports), school roster
APIs, and, for pages that had rolled to the next season or resisted scraping,
**season-verified Wayback Machine snapshots**. Every row carries its `source_url`.

**What mechanisms or procedures were used to collect the data, and how were they
validated?** A reproducible pipeline (`2_scripts/`): (1) athletics-domain resolution,
reused across sports; (2) a season-pinned static scraper (`scrape_sport.py`) with
landed-URL/title season gates, school-identity checks, gender disambiguation for shared
slugs, and WAF-aware fetching; (3) a **recovery ladder** for the tail — headless Chromium
rendering, academic-year URL variants, homepage-nav slug discovery, Wayback snapshots —
each result re-verified before keeping; (4) **reconciliation against the official NCAA
directory** (`reconcile_official.py`): every sport's captured teams are matched name- and
domain-level to the NCAA's own per-sport sponsor lists (web3.ncaa.org memberList), which
also supplies authoritative division labels and flags never-attempted schools and
mislabel risks. Each per-sport release passed an 11-check battery (id uniqueness, tier
parity, PII-clean public tiers, split parity, sourcing, label-integrity, coverage). The
whole build was benchmarked against the NCAA's published participation report
([OFFICIAL_COMPARISON.md](OFFICIAL_COMPARISON.md)).

**If the dataset is a sample, what was the sampling strategy?** Not a sample by design —
attempted census of the official sponsor lists. Non-capture is availability-driven (no
published roster / undiscoverable URL), not sampled; the missing-cell list ships as
`coverage_missing.csv`.

**Who was involved in the data collection process and how were they compensated?** The
author alone, unpaid, using LLM-assisted tooling (Claude Code) to write and run the
pipeline.

**Over what timeframe was the data collected?** In-season across the 2025-26 athletic
year: fall-2025 sports scraped/verified against `2025` season pages, winter sports
`2025-26`, spring sports `2026`, with rolled pages recovered from fall-dated archives.
Collection and recovery ran through July 2026. The data are point-in-time roster
snapshots, not season-cumulative.

**Were any ethical review processes conducted?** **No institutional IRB.** The author is
an independent researcher not covered by the Common Rule; research on public data of this
kind would in any case be exempt under 45 CFR 46.104(d)(4)(i). Treating that exemption as
a floor rather than clearance, a documented **self-review against Zimmer's (2010) four
concerns** (consent, identifiability, harm, "already public") was conducted and shapes
the release design — see [ETHICS_REVIEW.md](ETHICS_REVIEW.md).

**Did you collect the data from the individuals directly, or obtain it via third
parties?** Via third parties: the athletes' own institutions, which publish rosters on
their official public websites. Scraping was logged-out access to public pages (the
posture US courts have held outside the CFAA — *hiQ v. LinkedIn* (9th Cir. 2022), *Meta
v. Bright Data* (2024); caveats and full analysis in [LEGAL_NOTES.md](LEGAL_NOTES.md)).

**Were the individuals notified about the data collection?** **No.** The ~510,000
athletes were not notified. The data are facts their institutions had already published
about them in a directory-like form.

**Did the individuals consent to the collection and use of their data?** **No consent was
sought or obtained.** Athletes typically consent to their school's publicity practices,
not to third-party aggregation. This is stated plainly rather than excused; the
compensating controls are the de-identified public tier, the non-distribution of names
and inferred attributes, and a standing removal process.

**If consent was obtained, was a mechanism provided to revoke it?** N/A for consent, but
an **opt-out/removal mechanism exists regardless**: any athlete (or their representative)
can request removal of their row(s), honored in the next published version —
see [OPT_OUT.md](OPT_OUT.md).

**Has an analysis of the potential impact of the dataset on data subjects been
conducted?** Yes, informally: the identifiability and harm analysis in
[ETHICS_REVIEW.md](ETHICS_REVIEW.md) (linkage re-identification, aggregation beyond the
source context, inferred-attribute risk) drove the tier design, the BISG
non-distribution rule, and N<10 suppression. No formal DPIA (no GDPR nexus assessed;
US-published data about US college rosters).

---

## 4. Preprocessing / cleaning / labeling

**Was any preprocessing/cleaning/labeling of the data done?** Yes:

- **Contamination filtering** — coaches, staff-directory entries, and non-athlete rows
  removed (`is_non_athlete()`); a final cross-sport pass (2026-07-06) dropped 14
  residual staff rows (`sanitize_report.txt`).
- **Deduplication** — exact and (school, name, hometown) within-roster duplicates
  removed; label-integrity audits removed rosters duplicated under wrong school names.
- **Standardization** — `class_standing` (Fr/So/Jr/Sr/Gr/UNK), sport-specific
  `position_group`, `hometown_city`/`hometown_state` parsed (USPS), `origin`
  domestic/international, `high_school_is_academy` flagged. Raw fields (`*_raw`)
  retained alongside every standardized field.
- **Official-list reconciliation** — division labels and team inclusion reconciled to the
  NCAA sponsor lists; club programs excluded; overcaptures quarantined.
- **PII sanitization for release tiers** — names/contact/profile URLs stripped from
  public and analysis tiers; ~380 scrape-debris field values scrubbed (leaked
  names/emails/phones/social text) with a per-row log (`sanitize_report.txt`); the
  shipped build's audit is clean — forbidden-column check PASS across all 56 tier files,
  0 regex hits for email/phone/SSN (`pii_audit.txt`).

**Was the "raw" data saved in addition to the preprocessed data?** Yes — raw scrapes,
per-school CSVs, and run logs are preserved in the research repo (`outputs/`), enabling
full reprocessing. They are not distributed (they contain names).

**Is the software used to preprocess/clean/label the data available?** Yes — the pipeline
(`2_scripts/`: scraper, recovery ladder, reconciliation, deliverable builder,
`build_cross_sport_master.py`) is released with the project.

---

## 5. Uses

**Has the dataset been used for any tasks already?** Yes, by the author: talent-geography
and SES analyses per sport (each `dataset_release/<sport>/analysis/FINDINGS.md`) — e.g.
divisional SES stratification, recruiting-distance gradients, prep-academy pipelines,
state-level talent belts — and the official-vs-scraped participation benchmark
([OFFICIAL_COMPARISON.md](OFFICIAL_COMPARISON.md)).

**Is there a repository that links to papers or systems that use the dataset?** Not yet;
citations will be tracked on the dataset's Hugging Face page.

**What (other) tasks could the dataset be used for?** Sports-economics and
sociology-of-sport research (access, stratification, Title IX-adjacent participation
analysis), geographic/migration analysis via `hometown_*` joins to Census data,
institutional research via `school` joins to IPEDS/Scorecard/MRC (public sources,
joinable by key), a 2025-26 baseline for longitudinal roster studies, and record-linkage
methods research.

**Is there anything about the composition or collection that might impact future uses?**
Yes: (1) **roster snapshot ≠ squad size** — do not treat counts as compliance-report
participation figures (91.9%; relationship decomposed in OFFICIAL_COMPARISON.md);
(2) **track indoor/outdoor share source rows** — pick a unit of analysis before
cross-sport dedup; (3) missing teams skew small/non-D1 — weight or restrict accordingly;
(4) `hometown_*`/`high_school` are quasi-identifiers — any derived release should
preserve the de-identification posture and N<10 suppression for small cells;
(5) position vocabularies are sport-specific — use `position_group` with the per-sport
codebooks.

**Are there tasks for which the dataset should not be used?** **Yes:**
re-identification of individuals or linkage back to named rosters beyond what the source
institutions publish; per-athlete demographic inference (e.g. name- or geography-based
race prediction — the reason BISG outputs are not distributed); evaluation, ranking,
recruiting, or any decision-making about individual athletes; and any commercial use of
athletes' identities (NIL-adjacent uses). These are stated as conditions of use in the
release documentation.

---

## 6. Distribution

**Will the dataset be distributed to third parties?** Yes — the **public de-identified
rosters tier only** (19 columns). The enriched analysis tier is **research-only and is
not distributed** (owner decision 2026-07-07); its joined variables all come from public
sources (IPEDS, Scorecard, Census ACS, Opportunity Insights) that any user can re-join
by `school`/`hometown_*` key. Named restricted tiers stay in the research repo under
per-sport data-use terms and are never posted to any archive. BISG outputs are never
distributed at all. (Zenodo is explicitly ruled out for anything restricted — its terms
bar non-anonymized personal data.)

**How will the dataset be distributed?** On the **Hugging Face Hub** (CSV + Parquet, this
documentation alongside), with the build pipeline in the accompanying code repository. A
**DataCite DOI** is minted via Hugging Face **at release** — minted last, because a DOI
locks the repo against deletion/rename; per-record removal remains possible via file
commits (see OPT_OUT.md).

**When will the dataset be distributed?** After the final recovery merge and count
refresh (expected July 2026). Status at this writing: built, battery-verified,
PII-audited, unpublished.

**Will the dataset be distributed under a copyright or other IP license?** **CC0 1.0**,
with a **citation request** (not a legal condition). Rationale: the contents are facts,
which are uncopyrightable in the US (*Feist v. Rural*), so attribution licenses over the
data would likely be unenforceable; CC0 is the norm at Dryad/Figshare/Dataverse and
satisfies FAIR R1.1. Citation: Shah, Dharit (2026). *NCAA All Sports Rosters 2025-26: An
Individual-Level Dataset Across All Divisions.* [Data set].

**Have any third parties imposed IP-based or other restrictions on the data?** No. The
underlying facts are not copyrightable; collection was logged-out scraping of public
pages. Some athletics sites publish terms of use; no agreement was entered (no login, no
clickwrap).

**Do any export controls or other regulatory restrictions apply?** None known. FERPA
binds institutions, not third-party researchers, and the fields here mirror
directory-type information the institutions chose to publish; this is documented as an
open policy question, not a settled defense, in [LEGAL_NOTES.md](LEGAL_NOTES.md).

---

## 7. Maintenance

**Who will be supporting/hosting/maintaining the dataset?** The author (Dharit Shah),
via the Hugging Face repo and the project code repository.

**How can the owner/curator/manager be contacted?** dharits3@gmail.com, or issues on the
dataset's Hugging Face page.

**Is there an erratum?** An `ERRATA.md` will be maintained in the dataset repo from
v1.0: confirmed data errors, affected rows/versions, and the fixing version.

**Will the dataset be updated?** Yes. (1) **Correction releases** for errata and removal
requests, version-bumped with changelogs. (2) A planned **2026-27 reproduction** using
the released pipeline, published as a separate season dataset — the DOI is
version-scoped, so each season/version remains citable as consumed.

**If the dataset relates to people, are there applicable limits on the retention of the
data?** No statutory limit, but a standing **per-record removal process**: any athlete
can request deletion of their rows ([OPT_OUT.md](OPT_OUT.md)); removals are applied in
the next version via file commits (DOI-compatible) and logged in the errata without
naming the requester.

**Will older versions continue to be supported/hosted/maintained?** Hugging Face
preserves version history via git revisions; superseded versions remain accessible for
reproducibility, except that rows removed by opt-out are removed from the tip and noted
in errata (prior git revisions are the platform's retention behavior, documented in
OPT_OUT.md).

**If others want to extend/augment/build on the dataset, is there a mechanism?** Yes —
CC0 permits anything; the full pipeline is released for reproduction or extension to
future seasons; derived-work authors are asked to keep the de-identification posture and
suppression rules of §5. Contributions/corrections via Hugging Face discussions or
email.
