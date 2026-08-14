# Disclosure Risk Assessment — NCAA All Sports Rosters 2025-26 (v2.1.0)

**Written disclosure-risk assessment for the v2.1.0 public release: 513,655 athlete
rows, 1,087 schools, 28 sports, NCAA D1/D2/D3, 2025-26 athletic year. Built
2026-08-14.** Men 290,510 / Women 223,145. Companions:
[ETHICS_REVIEW.md](ETHICS_REVIEW.md), [OPT_OUT.md](OPT_OUT.md),
[LEGAL_NOTES.md](LEGAL_NOTES.md). Ground truth: the staged files under this
directory, [samples/data_dictionary.csv](samples/data_dictionary.csv), and the
PII audit run over every staged file (clean — see §3).

**Headline, stated up front:** v2.1.0 distributes **identified records by
design**. `first_name` and `last_name` are public columns. The prior release
(v2.0.5) was de-identified, and its disclosure assessment asked whether rows
could be re-identified through quasi-identifiers. That question is now moot.
The questions this document must answer instead are:

1. exactly what a named row does and does not reveal (§2);
2. whether the sensitive research tier can be reconstructed from released
   fields (§3 — it cannot);
3. what third-party data can be joined to a named row and what the join yields
   beyond what school sites already show (§4);
4. which populations carry elevated stakes (§5); and
5. what controls apply and what risk remains (§6–§7).

## 1. What changed in v2.1.0 and why

- **Names are public.** `first_name` / `last_name` ship in every public file.
  This is a deliberate, locked decision, not a leak: names are school-published
  facts on public athletics rosters, and named records enable legitimate
  external linkage — transfer tracking, honors and awards, draft outcomes —
  that de-identified rows structurally blocked.
- **Eight new columns** over the 19 in v2.0.5: `first_name`, `last_name`,
  `major`, `previous_school`, `height_raw`, `height_in`, `weight_raw`,
  `weight_lbs` — 27 public columns total. Every addition is a roster/bio-page
  fact or a deterministic parse of one.
- **Per-sport season-stats sidecars** for 10 sports (205,132 athletes, 39.9%
  of rows), joined on `athlete_id`. Stats are institution/NCAA-published
  season performance facts.
- **Row count:** 513,655 (v2.0.5: 514,696; −1,041 junk rows removed — 845
  duplicate rows, 196 header artifacts).
- **What v2.0.5's assessment still buys us:** its central finding — that the
  released fields carry no sensitive payload to unlock — is unchanged and now
  load-bearing in the opposite direction: naming a row attaches a name to
  facts the name's owner's school already published beside that name.

The honest cost of the change is stated in §4 and §7: naming removes the
per-row friction that de-identification imposed on bulk misuse. That friction
was under a minute per row (v2.0.5's own assessment: linkage "succeeds
essentially always"), but it was not zero, and this release trades it away.

## 2. Inventory of released fields and their sensitivity

Every distributed field is an institution-published fact from a public
athletics roster, bio, or stats page. Nothing inferred, imputed, or
third-party-joined ships in any public file. Parsed variants
(`height_in`, `weight_lbs`, `hometown_city`, `hometown_state`,
`position_group`, `class_standing`, `origin`, `high_school_is_academy`) are
deterministic transformations of the published string, not inferences.

| Group | Columns | Sensitivity assessment |
|---|---|---|
| Identity | `first_name`, `last_name` | **New in v2.1.0.** The name as the school published it. Highest-consequence fields in the release: they convert every other field from "attribute of an unnamed row" to "attribute of a named person." No middle name, no DOB, no photo, no contact vector accompanies them. |
| Surrogate key | `athlete_id` | Sport-prefixed random key; carries no external information. Joins the combined file to the stats sidecars, and is the removal key for opt-out. |
| Institutional | `school`, `sport`, `division`, `conference`, `gender`, `season`, `athletic_year` | Facts about teams and institutions, not persons. |
| Athletic | `position_raw`, `position_group`, `class_year_raw`, `class_standing`, `height_raw`, `height_in`, `weight_raw`, `weight_lbs` | Roster-card facts. Height present for 62.2% of rows, weight for ~30% — publication is a per-school convention, not a per-athlete signal. Height/weight of athletic-team members are expressly FERPA directory information ([LEGAL_NOTES.md](LEGAL_NOTES.md) §1). Physically descriptive but published precisely for public consumption. |
| Origin / pipeline | `hometown_raw`, `hometown_city`, `hometown_state`, `origin`, `high_school`, `high_school_is_academy`, `previous_school` | The fields that place a named person geographically. All appear on the source roster page beside the same name. `origin` is a 3-bin roster-listed derivation, **not citizenship**. |
| Academic | `major` | Directory information under FERPA ("major field of study"); present for 30.9% of rows, again by school convention. |
| Provenance | `source_url` | Points at the named source page. In v2.0.5 this was "the linkage vector"; in v2.1.0 it is plain provenance — the page it points to shows a superset of the row. |
| Stats sidecars | per-sport season statistics, 10 sports | Public performance facts (games, goals, times, etc.) joined by `athlete_id`. Named performance data is the standard published product of NCAA stats portals and Sports-Reference; the sidecar adds no field those sources lack. |

**Deliberately absent from every public file:** photographs, bio free text,
contact information of any kind, date of birth or age, jersey numbers, and
every research-tier field enumerated in §3.

## 3. What is withheld, and why it cannot be reconstructed

The following exist only in a research tier that is **never distributed**
(locked policy):

- **BISG race/ethnicity predictions** (per-athlete);
- **home-community income / SES measures** attached to hometowns;
- **census tract identifiers**;
- **College Scorecard / Opportunity Insights mobility joins.**

Non-reconstructability, demonstrated:

1. **No released column is derived from any withheld field.** The 27 public
   columns and every sidecar column trace to a school-published string or a
   deterministic parse of one. There is no residue — no band, no flag, no
   ordering — computed from BISG, income, tract, or mobility data anywhere in
   the staged files.
2. **The PII audit ran clean over every staged file**: a forbidden-column
   check enumerating the research-tier fields, plus a contact-info pattern
   scan (email/phone) over all free-text columns, found zero occurrences in
   any distributed file.
3. **The join key is one-way in practice.** `athlete_id` joins public files to
   each other. The research tier lives outside the release; no distributed
   artifact carries its values or its keys' payloads.

Two honest caveats, because "cannot be reconstructed" invites them:

- **A third party can run their own surname-based inference on the released
  names.** True, and unpreventable for any named dataset — it is a property
  of names, not of this release. What is protected is narrower and matters:
  this project's predictions do not ship, are not endorsed, and cannot be
  recovered from released fields; the project is not the distributor of any
  individual-level demographic inference. Use guidance requests that users not
  perform individual-level sensitive inference; under CC0 this is a stated
  norm, not an enforceable condition, and it is labeled as such.
- **A third party can join public place statistics to `hometown_city`.** Also
  true and also generic: the same keys are on the source roster page. Place
  medians are facts about places; the research tier's SES attachments confer
  no individual-level attribute even internally, and none of them ships.

## 4. Linkage and mosaic analysis

What can be joined to a named row, and what the join yields:

| Auxiliary source | Join key | Marginal yield over the school's own site |
|---|---|---|
| The source roster/bio page (`source_url`) | direct | **Negative** — the source shows more (photo, bio, often jersey number) than the row does. |
| NCAA stats portals, Sports-Reference/College Basketball Reference, ESPN, 247Sports | name + school + sport | None per athlete — these already publish the same named roster and performance facts. The sidecars are a convenience copy of the same class of data. |
| Social media | name + school + sport | Whatever the athlete publishes there; the dataset adds hometown/high-school confirmation an adversary would also find on the roster page. |
| Transfer portal coverage, draft databases, honors lists | name + school | This is the **intended legitimate linkage** — outcome tracking. It works precisely because rows are named. |
| People-search / data-broker products | name + hometown | The genuinely uncomfortable join: name + `hometown_city` + `high_school` narrows a broker query toward family and home address. **The identical pairing appears on the source page**; the dataset's contribution is not the pairing but the enumeration capability below. |

**The mosaic finding.** Per individual, the release reveals nothing a school
site does not already show — the row is a strict subset of the source page.
What the release genuinely adds is **bulk accessibility**: one file that can
enumerate, in one query, every listed athlete from a given town, high school,
or roster demographic slice across 1,087 schools. Capabilities that follow:

- mass solicitation (NIL/recruiting/commercial spam directed at named
  athletes) — mitigated by carrying **no contact vectors** of any kind;
- gambling-adjacent harassment target lists (names joined to performance
  sidecars) — the same join is already the front page of every stats portal;
  the dataset is a season snapshot, not a live feed, and carries no channel
  to reach anyone;
- cross-school profiling of individuals over time — currently a single
  athletic year (2025-26); multi-year concatenation is a future-release
  decision that re-triggers this assessment (§6).

No mitigation makes bulk enumeration impossible without un-naming or
un-publishing the data; §6–§7 record what bounds it.

## 5. Special populations

- **Minors.** Some first-year athletes are 17 at fall enrollment. No date of
  birth is collected or carried in any tier, so they cannot be distinguished
  — and in v2.1.0 they are **named**. Their rows carry the same
  school-published facts as everyone else's; the elevated stake is that the
  standing mitigations (no DOB, no contact info, no photos, guardian-operable
  opt-out) are now protecting identified children.
- **Team IMPACT honorees.** Some rosters include Team IMPACT children —
  chronically ill kids formally honored as team members — and schools list
  them as athletes, occasionally with distinctive entries (e.g., very low
  listed weights such as 90 lbs). They are **kept as published**: they are
  genuine roster listings, and silently deleting them would misrepresent the
  rosters. The disclosure consequences are noted explicitly: these are
  **named minors**, and the honoring context carries an implicit health
  signal. The dataset deliberately carries **no flag** identifying them —
  constructing one would create a health-adjacent attribute the source never
  published. A parent or guardian can have an honoree's row removed via
  [OPT_OUT.md](OPT_OUT.md). Users doing anthropometric analysis should note
  that these listings can appear as weight outliers.
- **International athletes.** `origin` = international is a roster-listed
  fact, not citizenship or visa status. A named row locating an international
  athlete's foreign hometown replicates the source page; no immigration,
  nationality, or documentation attribute exists in any tier. Coverage
  analysis should treat `origin` as the publication convention it is.

## 6. Mitigations and controls

| Control | Content |
|---|---|
| Opt-out | Any listed athlete — or a parent/guardian, including for Team IMPACT honorees — can request row-level removal by `athlete_id` or name + school + sport. Removal covers the combined file **and** the stats sidecars and sample files, propagates to the next version on all three platforms (Hugging Face canonical, Kaggle mirror, GitHub), target turnaround 14 days. Full process: [OPT_OUT.md](OPT_OUT.md). |
| Public removals ledger | [RELEASE_NOTES.md](RELEASE_NOTES.md) carries a Removals section recording version and count of removed rows — never requester identity. The ledger starts empty at v2.1.0. |
| Excluded field classes | No photos, no bio text, no contact information, no birthdates or ages, no jersey numbers. These are absence-by-policy, enforced by the release battery's locked 27-column schema check plus the PII forbidden-column audit, not incidental gaps. |
| Research-tier firewall | BISG predictions, SES/income measures, tract identifiers, and mobility joins never leave the research tier (§3); the line is absolute and re-certified in [ETHICS_REVIEW.md](ETHICS_REVIEW.md). |
| License note | CC0 1.0 covers the **database compilation only**; it grants no name/image/likeness or publicity rights, and downstream users remain responsible for lawful use of names ([LICENSE](LICENSE), [LEGAL_NOTES.md](LEGAL_NOTES.md) §2). |
| Reassessment | This assessment is re-run at each annual release and out-of-cycle on any trigger: a new field, a new sidecar, multi-year concatenation, an opt-out dispute, or evidence of misuse. |

## 7. Residual-risk statement

Accepted residual risks, named plainly:

1. **Bulk enumeration of named individuals** (§4) is inherent to a named
   release and is bounded, not eliminated, by the absence of contact vectors
   and sensitive attributes.
2. **Named minors are present and not individually distinguishable**,
   including Team IMPACT honorees whose listing context implies a health
   condition. The guardian opt-out is the operative control; it is reactive
   by nature.
3. **Persistence beyond the source page.** Schools prune rosters; this
   dataset preserves 2025-26 listings after the source lapses. The payload
   preserved is fixed at roster facts plus season statistics — no sensitive
   attribute accrues to a stale row.
4. **Downstream re-processing of names** (including demographic inference by
   third parties) cannot be prevented under CC0. The project's controls end at
   what it distributes; that boundary is stated rather than obscured.
5. **Removal is forward-only.** CC0 copies made before a removal cannot be
   recalled; removal guarantees absence from every subsequent version on
   every platform.

These are accepted because the alternative controls — withholding names,
coarsening hometowns, or not releasing — either restore a de-identification
that v2.0.5's own analysis showed took under a minute to defeat, or destroy the linkage
value that motivates the release. The proportionality argument is made in
full in [ETHICS_REVIEW.md](ETHICS_REVIEW.md) (Named-release addendum).

---
*Assessed 2026-08-14 against the v2.1.0 staged build (513,655 rows / 1,087
schools / 28 sports). Supersedes the v2.0.5 quasi-identifier assessment,
whose treatment table remains the record for the de-identified releases.*
