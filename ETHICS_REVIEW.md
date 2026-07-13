# Ethics Self-Review — NCAA All Sports 2025-26

**Ethics review** for the cross-sport master dataset (515,085 athlete rows,
1,089 schools — final build 2026-07-08). Reviewer: Dharit Shah
(dharits3@gmail.com), independent researcher, no institutional affiliation. This document
**is** the ethics review — see §4 for why that is the honest framing rather than a gap.

Structured around the four concerns Zimmer (2010) raised against the T3 "Tastes, Ties,
and Time" Facebook release — the canonical case of an IRB-approved, "anonymized" roster-like
dataset that was re-identified and withdrawn. Each section states the concern, how it applies
here, and the disposition. Companion document: [DISCLOSURE_RISK.md](DISCLOSURE_RISK.md)
(field-level indirect-identifier assessment).

## 1. Consent

**None was obtained.** No athlete was asked, and no athlete was notified. That is stated
plainly, as the Gebru et al. datasheet standard expects for scraped-web data — the honest
answer is "No," not a rationalization dressed as consent.

Release proceeds anyway, on three grounds:

- **Institution-published facts.** Every field in the public tier is a fact the athlete's
  own school published on its official athletics site, roster-by-roster, as a condition of
  the athlete's public-facing participation in NCAA sport. The dataset collects what schools
  chose to publish; it adds no fact about any individual (see DISCLOSURE_RISK §3).
- **De-identified public tier.** Names never appear in any distributed file. Named rosters
  stay in per-sport `restricted/` folders in this repo and are never deposited anywhere.
  Per-athlete BISG race predictions are research-only and are never distributed in any tier.
- **Opt-out.** Any athlete (or school) may request removal of their row(s); the process is
  documented in [OPT_OUT.md](OPT_OUT.md) (linked from the release README) and honored
  per-record at the repository level. Consent
  is not retroactively manufactured by this — opt-out is a mitigation, not a substitute —
  but it gives affected individuals a working lever, which T3's subjects never had.

## 2. Contextual expectations of privacy

Nissenbaum's test: does moving the information violate the **norms of the context in which
it was shared**? College athletes expect their roster entry — name, position, class,
hometown, high school — on the public athletics site, in game programs, in media guides,
and in press coverage. Public listing is the norm of this context, not a breach of it.

What this release changes about the context, honestly assessed:

| Contextual shift | Assessment |
|---|---|
| **Aggregation** (one school's page → 515,085 rows) | Real shift: cross-school queries become trivial. But the aggregate reveals population patterns, not new individual facts — each row remains exactly what one school published. |
| **SES/geography enrichment** (analysis tier — research-only, not distributed as of 2026-07-07) | Joins are **place-level aggregates only** — IPEDS/Scorecard facts about the *school*, census-style medians about the *hometown city*. An athlete's row gains "the median household income of the city they told their school they are from," not their family's income. No individual-level attribute is inferred or attached in any distributed tier. |
| **Research framing** (fan context → demographic research) | The purpose shifts from following a team to studying access and stratification in college sport. Mitigations answer this: analysis operates on de-identified rows; findings are reported as aggregates with N<10 suppression; the one genuinely sensitive derived attribute (BISG race prediction) is confined to research-only files precisely because attaching an inferred race to a locatable individual *would* breach contextual integrity. |

**Disposition:** the context shift is acknowledged and bounded. The public tier contains
nothing an athlete's own school did not publish about them; the enrichment adds context
about *places and institutions*, not people.

## 3. Anonymization strategy

**There is none, and the dataset does not claim one.** The public tier is
**de-identified, not anonymized** — the distinction is stated in the CODEBOOK, README,
and datasheet. school + sport + position + class + hometown + high_school will often be
unique; joining a row to the (public, live or archived) source roster page recovers the
name trivially — indeed `source_url` points straight at it. T3's fatal error was claiming
anonymity while shipping quasi-identifiers; this release ships the quasi-identifiers and
claims only de-identification.

Why the quasi-identifiers are **retained rather than coarsened**: hometown, high school,
and class year *are* the research value — geographic origin, recruiting pipelines, and
SES-linked access are the questions the dataset exists to answer — and every one of them
is an institution-published fact. Coarsening them would destroy the dataset's purpose to
defend against an "attack" that recovers only information the attacker's own linkage
target already displays. Full field-by-field reasoning, the honest linkage-attack
walkthrough, and the one genuine residual risk are in
[DISCLOSURE_RISK.md](DISCLOSURE_RISK.md).

## 4. Competence and process of ethics review

- **No IRB.** The author is an independent researcher with no institutional affiliation.
  Under the Common Rule, an unaffiliated individual has no IRB obligation, and research on
  public data is in any case exempt under 45 CFR 46.104(d)(4)(i). **Exemption is a floor,
  not ethical clearance** — T3 *had* IRB approval and still failed. Regulatory exemption is
  therefore treated here as answering nothing.
- **This document is the review.** Its competence rests on the named frameworks it applies
  — Zimmer (2010), Nissenbaum contextual integrity, Gebru et al. datasheets, Google Data
  Cards (Human & Sensitive Attributes; Intentionality, covering inferred attributes like
  BISG), and the ICPSR two-tier disclosure model — rather than on institutional sign-off
  it cannot obtain and will not simulate.
- **Standing decisions this review certifies:** public tier de-identified (no names, no
  contact info, no per-athlete demographic predictions, ever); BISG outputs research-only,
  never distributed; enrichment place-level only; aggregates N<10-suppressed; named tiers
  never deposited on Zenodo (its terms bar non-anonymized personal data) or any external
  host; DOI minted last, with a per-record removal path in place first.
- **Revisit:** this review is re-run at each annual release, and out-of-cycle on any
  trigger — a new field, a new enrichment source, a change in tier boundaries, an opt-out
  dispute, or evidence of misuse.

## Minors

Some first-year athletes are **17 at fall enrollment**. Schools publish their roster
entries regardless of age — the NCAA public-roster norm does not carve out 17-year-olds,
and neither the scrape nor the release can distinguish them: **no date of birth is
collected or carried in any tier**. The fields carried for a possibly-17-year-old are the
same institution-published facts as for everyone else, in the same de-identified form.
Age is therefore not a field-level risk, but it raises the stakes of the standing
mitigations (no names, no inferred attributes, opt-out honored without question), and it
is one reason the sensitive-attribute line — no per-athlete race predictions distributed,
ever — is absolute rather than negotiable.

## The T3 lesson, applied

- **"Already public" does not discharge obligations.** Zimmer's core holding, adopted here
  in full. The public-facts argument in §1 carries weight only *because* it travels with
  de-identification, place-level-only enrichment, suppression rules, opt-out, and this
  review. Publicness is the starting point of the analysis, not the end of it.
- **The refuted overreach, also on record.** Zimmer's further claim — that naming the
  source school destroys anonymity by itself — was tested in this project's verification
  run and **refuted 0-3**. School names are retained. This review does not overstate risk
  to appear cautious: the honest position is that re-identification here is trivial *via
  linkage to the public source* (acknowledged, §3) and that school-naming per se adds
  nothing to it.

---
*Public tip v2.0.1 (2026-07-13): 515,085 rows after label correction. Ethics review dated 2026-07-07 (v2.0 baseline).
Next scheduled review: 2026-27 release.*
