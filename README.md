# NCAA All Sports 2025-26 — Cross-Sport Master Dataset

The **individual-level roster dataset for every NCAA championship sport** — all 28 sports,
all three divisions, both genders, one coherent athletic year (2025-26). **515,393 athlete
rows across 1,089 schools** (build 2026-07-06; all counts below refresh at the final
build), concatenated from the 28 validated per-sport releases
with a `sport` key and `athletic_year` added to every row. Two recovery jobs (~500
pipeline-dropped athletes; missing-sponsor-cell sweep) are in flight — **final counts land
after the recovery merge**.

> **Status:** built + battery-verified + PII-audited. Release docs drafted 2026-07-07
> (datasheet, ethics review, disclosure risk, legal notes — see
> [Release governance](#release-governance)). **Not yet published — release is blocked on
> Gate 0** (recovery merge + count refresh) of
> [RELEASE_CHECKLIST.md](RELEASE_CHECKLIST.md).

## Contents

```
all/
├── README.md              ← you are here
├── metadata.json          ← build sidecar: git head, per-sport rows/seasons, checks
├── pii_audit.txt          ← PII audit result for the shipped build (clean)
├── sanitize_report.txt    ← per-row log of the release-tier PII sanitization
├── data/                  ← PUBLIC tier (de-identified — no names)
│   ├── ncaa_all_sports_rosters_2025-26.csv       515,393 athletes × 19 cols
│   ├── ncaa_all_sports_rosters_2025-26.parquet   same data, typed nulls
│   └── CODEBOOK.md
└── analysis/              ← enriched analysis tier (de-identified + SES/geography)
    ├── ncaa_all_sports_analysis_dataset.csv      515,393 × 54 cols
    └── ncaa_all_sports_analysis_dataset.parquet
```

**v2.0 (this build)** adds two enrichments to the analysis tier: a high-school
**public / private-religious / private-secular / prep-academy** classification (from the NCES
CCD + PSS federal censuses), and **Census-validated town income & population** (ACS 2022) that
replaces the previously mis-scaled `hometown_population`. See [data/CODEBOOK.md](data/CODEBOOK.md)
for the full 54-column data dictionary and [DATASET_RELEASE_NOTES.md](DATASET_RELEASE_NOTES.md)
for what changed.

**Only `data/` (+ the governance docs) is published.** The `analysis/` tier is
research-only (owner decision 2026-07-07) — it stays in this private repo; its joined
variables are all re-derivable from public sources (IPEDS, Scorecard, Census, MRC).

There is **no cross-sport restricted (named) tier** — named rosters stay in the per-sport
`dataset_release/<sport>/restricted/` folders under their data-use terms.

**vs the official NCAA numbers:** this dataset holds 91.9% of the NCAA-reported athlete
participations (2024-25 report: 561,290 incl. emerging sports), ≈96.9% of sponsor-list
teams, ≈98% of Division I — and is the only public individual-level count for 2025-26
until the NCAA's own report lands (~Sept 2026). The gap is mostly *reported-squad-size vs
published-roster* (walk-ons/practice players who never appear on school websites), not
scraping misses — full decomposition in [OFFICIAL_COMPARISON.md](OFFICIAL_COMPARISON.md).

## Shape

| | |
|---|---|
| Athletes | 515,393 (Men 291,389 / Women 224,004) |
| Divisions | D1 186,459 · D2 124,444 · D3 204,490 |
| Origin | domestic 452,839 (87.9%) · international 51,411 (10.0%) · unknown 11,143 (2.2%) |
| Sports | 28 (every NCAA championship + emerging sport) |
| Schools | 1,089 |
| Athletic year | 2025-26 for every row (`season` carries each sport's label: `2025`, `2026`, or `2025-26`) |

## The one thing to know before counting across sports

**`track_indoor` and `track_outdoor` share source rows by design.** Schools publish a single
track & field roster; the indoor sport is materialized from the shared scrape and
independently reconciled against the NCAA indoor sponsor lists. The two sports carry
different athlete_ids (`tfi_`/`tfo_`) for the same person-season — 1,571 school-gender teams
overlap. **Never dedupe across sports**; when counting unique athletes across all sports,
decide explicitly how to treat indoor/outdoor track (e.g. keep one of the two).

An athlete on two sports' rosters (e.g. cross country + track) appears once per sport, with
per-sport athlete_ids. `athlete_id` joins to the per-sport restricted and analysis tiers.

## Provenance & quality

- Source: official school athletics-site rosters (Sidearm / Presto / WMT / school roster
  APIs / Wayback snapshots), scraped in-season against the 2025-26 athletic year.
- Every per-sport release passed the 11-check verification battery (id uniqueness, tier
  parity, PII-clean public tiers, split parity, sourcing, label-integrity audits, coverage
  vs the official NCAA sponsor lists). Coverage per sport is in each
  `dataset_release/<sport>/README.md` (D1 typically 94-100%).
- Release tiers were PII-sanitized (2026-07-06): 14 staff-directory rows dropped, ~380
  scrape-debris field values scrubbed (leaked names / emails / phones / social-media text);
  see `sanitize_report.txt`. The shipped build's audit is clean (`pii_audit.txt`).
- De-identified, not anonymized: no names, no per-athlete demographic predictions;
  `hometown_*` and `high_school` are quasi-identifiers retained for research value. Do not
  attempt re-identification.
- Rebuild: `python 2_scripts/analysis/build_cross_sport_master.py` (idempotent; re-run after
  any per-sport rebuild).

## Release governance

The gate-ordered path to publication, and the documents behind it:

- [RELEASE_CHECKLIST.md](RELEASE_CHECKLIST.md) — **the master gate document**; ordered blocking
  gates from recovery merge through DOI minting and post-release policy.
- [DATASHEET.md](DATASHEET.md) — Datasheets for Datasets (Gebru et al.): composition,
  collection, and honest identifiability/consent/notification answers.
- [ETHICS_REVIEW.md](ETHICS_REVIEW.md) — independent-researcher ethics self-review; IRB
  exemption analysis (45 CFR 46.104(d)(4)(i)) as a floor, not clearance.
- [DISCLOSURE_RISK.md](DISCLOSURE_RISK.md) — quasi-identifier analysis for `hometown_*` +
  `high_school`; why the public tier is de-identified, not anonymized.
- [LEGAL_NOTES.md](LEGAL_NOTES.md) — scraping case law (hiQ, Bright Data), Feist/CC0
  rationale, FERPA/NIL open questions.
- [OFFICIAL_COMPARISON.md](OFFICIAL_COMPARISON.md) — coverage vs the NCAA's own
  participation report, gap fully decomposed.
- Data license: **CC0 1.0** ([LICENSE](LICENSE) — public-domain dedication with a
  citation request and an explicit no-NIL/publicity-grant note). Code license: MIT
  (repo root `LICENSE`). Citation metadata: repo-root `CITATION.cff` (DOI filled at
  Gate 4).
- Opt-out: removal-request process ([OPT_OUT.md](OPT_OUT.md)) — honored without
  question; per-record removal stays possible after DOI minting via file commits.
