# Disclosure Risk Assessment — NCAA All Sports 2025-26

**Written indirect-identifier risk assessment** (ICPSR frame) for the public and analysis
tiers of the cross-sport master. **Current tip: v2.0.4 — 514,696 rows, 1,087 schools**
(2026-07-18). Assessment written against the Gate-0 final build of **2026-07-08
(515,393 rows / 1,089 schools)**; post-review removals changed counts, not the
quasi-identifier inventory or treatment decisions. Companion:
[ETHICS_REVIEW.md](ETHICS_REVIEW.md). Ground truth used here: `data/CODEBOOK.md`,
`pii_audit.txt` (clean: forbidden-column check PASS across all 56 tier files; 0 regex hits
for email/phone/SSN in free-text columns), `sanitize_report.txt` (per-row log:
staff-directory rows dropped, leaked `high_school` values scrubbed).

**Headline, stated up front:** the public tier is **de-identified, not anonymized**.
Re-identification by linkage to the source roster pages is trivial — by design, since every
field is an institution-published fact and `source_url` points at the page. The assessment
therefore turns on what a successful linkage *yields*, not on whether it is possible.

## 1. Quasi-identifier inventory

Direct identifiers (names, contact info, photos, bio links) are absent from all distributed
tiers — removed at sanitization and enforced by the forbidden-column audit. What remains:

| Field | Uniqueness contribution | Note |
|---|---|---|
| `school` | High (partitions to ~15–120 athletes) | 1,087 values |
| `sport` + `gender` | High (partitions to one team, ~10–120 rows) | with school: the roster itself |
| `position_raw` / `position_group` | Medium–high within team | often 1–5 per team |
| `class_year_raw` / `class_standing` | Medium within team | 6 bins |
| `hometown_city` / `hometown_state` / `hometown_raw` | High | frequently unique within a team |
| `high_school` (+ `high_school_is_academy`) | High | frequently unique within a team |
| `origin` | Low | 3 bins |
| `source_url` | **Total** — direct pointer to the named source page | provenance field |
| `athlete_id` | None externally (surrogate) | joins to restricted tier **inside this repo only** |

school × sport × gender × class × position × hometown is unique for the large majority of
rows. This is not disputed; it is the premise of the analysis below.

## 2. The linkage attack

Take any row → open `source_url` (or search "{school} {sport} roster 2025-26", or the
Wayback capture) → match on position + class + hometown + high school → read the name.
Cost: under a minute, no skill, no auxiliary dataset. **The attack succeeds essentially
always** while the source page is live or archived.

## 3. Why the attack yields no new information

The linkage target — the school's own roster page — is the very page that **already
displays the name next to every quasi-identifier in the row**. An attacker who completes
the linkage learns nothing about the individual they did not have on screen at the moment
of linkage. The dataset's marginal content over its sources is:

- cross-school **aggregation** (the population, not any person), and
- **place-level context** in the analysis tier — IPEDS/Scorecard/Opportunity Insights
  facts about the *school* and census-style medians about the *hometown city*
  (`school_pct_pell`, `mrc_par_median`, `hometown_median_household_income`, `dist_mi`,
  `income_band`, ...). All are public statistics about places and institutions,
  attachable by anyone from the same public keys; none is an individual-level attribute
  or inference.

No distributed tier contains a sensitive attribute to *unlock* by re-identifying: no race
(BISG outputs are research-only, never distributed), no income, no contact info, no DOB,
no injury/eligibility status. Linkage recovers a name the source already published,
attached to facts the source already published.

## 4. The genuine residual risk

**A future reader, after the source rosters go offline.** Schools prune old rosters
(though Presto-era season pages persist for years, and Wayback holds captures); a reader
in 2036 could hold a quasi-unique row for a person whose roster entry is no longer
casually findable. Even then the row contains only 2025-26 roster facts — but the dataset
would at that point be *preserving* publicity the school had let lapse. Mitigations:

- **Opt-out**, honored per-record indefinitely (repository-level removal path established
  before DOI minting, since the DOI locks the record against deletion).
- **No sensitive attributes** in any distributed tier — the payoff of future linkage stays
  fixed at "was on a roster, played this position, from this town."
- Documented warning in CODEBOOK/DATASHEET: do not attempt re-identification.

Accepted as residual: it cannot be eliminated without destroying the quasi-identifiers
that are the dataset's research value (§6).

## 5. Aggregate-tier rules

Published aggregates (including any research-only BISG summaries that surface in papers)
follow: **suppress cells with N<10**. One suppressed cell in a margined table is
recoverable by subtraction — a school × sport × race table with published school × sport
totals leaks every lone suppressed cell. Therefore, prescriptively:

- **Complementary suppression:** whenever a cell is suppressed, suppress at least one
  complementary cell in each margin that would otherwise recover it; or
- **Coarsen instead:** collapse race bins (or class/position bins) until every cell
  clears N≥10, so no suppression is needed. Preferred for school × sport × race tables,
  where single-suppression is near-certain at team sizes.

No aggregate release ships with exactly one suppressed cell in any margined dimension.

## 6. Treatment decisions (ICPSR menu: retain / recode / suppress / remove)

| Field | Decision | Justification |
|---|---|---|
| Names, contact info, photos, bio text | **Removed** (sanitization) | Direct identifiers. 14 staff-directory rows dropped; leaked emails/phones/social text scrubbed from free-text fields (`sanitize_report.txt`); absence enforced by `pii_audit.txt` forbidden-column check. |
| BISG per-athlete race predictions | **Never distributed** (research-only) | The only sensitive inferred attribute in the project; attaching it to a linkable row is the one outcome every other control exists to prevent. |
| `hometown_city` / `hometown_state` / `hometown_raw` | **Retain as published** | Core research value (geographic origin, access). Institution-published. Coarsening to state would gut origin analysis while linkage remains trivial via the remaining fields — cost without benefit. |
| `high_school` (+ `_is_academy`) | **Retain as published** | Core research value (pipelines, academy routes). Institution-published. Same cost/benefit as hometown. |
| `class_year_raw` / `class_standing` | **Retain as published** | Needed for cohort/eligibility analysis; institution-published; low marginal uniqueness given hometown is already carried. |
| `position_raw` / `position_group` | **Retain as published** | Sport-structural variable; institution-published. |
| `school`, `sport`, `division`, `conference`, `gender`, `season` | **Retain — no treatment needed** | Institutional/structural, not personal. School-naming per se does not destroy anonymity (claim tested and refuted 0-3 — ETHICS_REVIEW). |
| `origin` | **Retain — no treatment needed** | 3-bin derivation of hometown; adds no uniqueness beyond it. |
| `source_url` | **Retain, documented as the linkage vector** | Provenance is a FAIR requirement (R1.2) and the honest disclosure of the linkage path. Removing it would not impede the attack (a web search reproduces it) — it would only obscure provenance. |
| `athlete_id` | **Retain (surrogate)** | Carries no external information; sport-prefixed random key. Join to named restricted tier exists only inside this repo; restricted tiers are never deposited externally. |
| Analysis-tier place joins (`school_*`, `mrc_*`, `hometown_median_household_income`, `hometown_population`, `dist_mi`, `in_state`, `income`/`income_band`) | **Retain — no treatment needed** | Public place-level statistics keyed on already-carried fields; reproducible by any user from IPEDS/Scorecard/MRC; no individual-level content. |

**Summary decision:** the public tier ships as-is under the de-identified-not-anonymized
label; the analysis tier adds place-level context only; names and inferred sensitive
attributes never leave the repo; aggregates follow §5. Reassessed at each annual release
alongside ETHICS_REVIEW.md.

---
*Assessment written against Gate-0 final build 2026-07-08 (515,393 rows / 1,089 schools).
Assessed 2026-07-07. Current tip: v2.0.4 (514,696 rows / 1,087 schools, 2026-07-18).*
