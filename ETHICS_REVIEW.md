# Ethics Self-Review — NCAA All Sports Rosters 2025-26

**Ethics review for the cross-sport master dataset. Current release: v2.1.0 —
513,655 athlete rows, 1,087 schools, 28 sports, built 2026-08-14.** Reviewer:
Dharit Shah (dharits3@gmail.com), independent researcher, no institutional
affiliation. This document **is** the ethics review — see Part I §4 for why
that is the honest framing rather than a gap.

Structure: **Part I** carries forward the v2.0.5 review (written 2026-07-07
against the de-identified release) with its dispositions intact; statements
that v2.1.0 supersedes are marked inline rather than silently rewritten.
**Part II** is the dated named-release addendum: v2.1.0 makes
`first_name`/`last_name` public columns, and that decision is reviewed on its
own terms there. Companion: [DISCLOSURE_RISK.md](DISCLOSURE_RISK.md).

---

# Part I — v2.0.5 review, carried forward

Structured around the four concerns Zimmer (2010) raised against the T3
"Tastes, Ties, and Time" Facebook release — the canonical case of an
IRB-approved, "anonymized" roster-like dataset that was re-identified and
withdrawn. Each section states the concern, how it applies here, and the
disposition.

## 1. Consent

**None was obtained.** No athlete was asked, and no athlete was notified. That
is stated plainly, as the Gebru et al. datasheet standard expects for
scraped-web data — the honest answer is "No," not a rationalization dressed as
consent.

Release proceeds anyway, on three grounds:

- **Institution-published facts.** Every field in the public release is a fact
  the athlete's own school published on its official athletics site,
  roster-by-roster, as a condition of the athlete's public-facing
  participation in NCAA sport. The dataset collects what schools chose to
  publish; it adds no fact about any individual.
- **De-identified public tier.** *[Superseded in v2.1.0 — names are now
  public columns; see Part II. The remainder of this ground survives: per-
  athlete BISG race predictions are research-only and are never distributed
  in any tier.]*
- **Opt-out.** Any athlete (or school) may request removal of their row(s);
  the process is documented in [OPT_OUT.md](OPT_OUT.md) and honored
  per-record. Consent is not retroactively manufactured by this — opt-out is
  a mitigation, not a substitute — but it gives affected individuals a
  working lever, which T3's subjects never had.

## 2. Contextual expectations of privacy

Nissenbaum's test: does moving the information violate the **norms of the
context in which it was shared**? College athletes expect their roster entry —
name, position, class, hometown, high school, height/weight where the school
lists them — on the public athletics site, in game programs, in media guides,
and in press coverage. Public listing is the norm of this context, not a
breach of it.

What release changes about the context, honestly assessed:

| Contextual shift | Assessment |
|---|---|
| **Aggregation** (one school's page → 513,655 rows) | Real shift: cross-school queries become trivial. But the aggregate reveals population patterns, not new individual facts — each row remains exactly what one school published. Re-examined for the named release in Part II §7. |
| **SES/geography enrichment** (research tier — never distributed) | Joins are place-level aggregates only — facts about the *school* and the *hometown city*, not the person. No individual-level attribute is inferred or attached in any distributed file. |
| **Research framing** (fan context → demographic research) | The purpose shifts from following a team to studying access and stratification in college sport. The one genuinely sensitive derived attribute (BISG race prediction) is confined to research-only files precisely because attaching an inferred race to a locatable individual *would* breach contextual integrity. |

**Disposition:** the context shift is acknowledged and bounded. The public
release contains nothing an athlete's own school did not publish about them;
the enrichment adds context about *places and institutions*, not people, and
does not ship.

## 3. Anonymization strategy

*[Superseded in v2.1.0. The v2.0.5 position was: the public tier is
"de-identified, not anonymized," quasi-identifiers are retained because they
are the research value, and linkage to the source page recovers the name
trivially. That analysis — re-identification is minutes-deep by design — is
precisely what Part II relies on when it concludes de-identification bought
friction, not protection. There is no anonymization claim in v2.1.0; rows are
identified by design and the assessment lives in
[DISCLOSURE_RISK.md](DISCLOSURE_RISK.md).]*

T3's fatal error was claiming anonymity while shipping quasi-identifiers.
v2.0.5 shipped the quasi-identifiers and claimed only de-identification;
v2.1.0 ships the names and claims nothing it does not do.

## 4. Competence and process of ethics review

- **No IRB.** The author is an independent researcher with no institutional
  affiliation. Under the Common Rule, an unaffiliated individual has no IRB
  obligation, and research on public data is in any case exempt under 45 CFR
  46.104(d)(4)(i). **Exemption is a floor, not ethical clearance** — T3 *had*
  IRB approval and still failed. Regulatory exemption is therefore treated
  here as answering nothing.
- **This document is the review.** Its competence rests on the named
  frameworks it applies — Zimmer (2010), Nissenbaum contextual integrity,
  Gebru et al. datasheets, Google Data Cards (Human & Sensitive Attributes;
  Intentionality, covering inferred attributes like BISG), and the ICPSR
  disclosure-review model — rather than on institutional sign-off it cannot
  obtain and will not simulate.
- **Standing decisions this review certifies** (as amended by Part II): BISG
  outputs research-only, never distributed; SES/income, census-tract, and
  mobility joins research-only, never distributed; aggregates in publications
  N<10-suppressed; per-record removal path in place before DOI minting.
- **Revisit:** this review is re-run at each annual release, and out-of-cycle
  on any trigger — a new field, a new enrichment source, a change in tier
  boundaries, an opt-out dispute, or evidence of misuse.

## 5. Minors

Some first-year athletes are **17 at fall enrollment**. Schools publish their
roster entries regardless of age — the NCAA public-roster norm does not carve
out 17-year-olds, and neither the scrape nor the release can distinguish them:
**no date of birth is collected or carried in any tier**. Age is therefore not
a field-level risk, but it raises the stakes of the standing mitigations, and
it is one reason the sensitive-attribute line — no per-athlete race
predictions distributed, ever — is absolute rather than negotiable.
*[v2.1.0 raises these stakes again — minors are now named. Part II §6.]*

## 6. The T3 lesson, applied

- **"Already public" does not discharge obligations.** Zimmer's core holding,
  adopted here in full. The public-facts argument in §1 carries weight only
  *because* it travels with the sensitive-attribute firewall, suppression
  rules, opt-out, and this review. Publicness is the starting point of the
  analysis, not the end of it. Part II §7 re-argues this for the named
  release, where it bites hardest.
- **The refuted overreach, also on record.** Zimmer's further claim — that
  naming the source school destroys anonymity by itself — was tested in this
  project's verification run and refuted 0-3. School names are retained.

---

# Part II — Named-release addendum (v2.1.0), 2026-08-14

v2.1.0 makes `first_name` and `last_name` public columns and adds `major`,
`previous_school`, and height/weight fields, plus season-stats sidecars for 10
sports (205,132 athletes, 39.9%). This addendum reviews that decision. It was
a locked, affirmative user decision, and this review's job is to state the
grounds honestly and stress-test them, not to ratify them by reflex.

## 1. Rationale

Two grounds, in order of weight:

1. **Names are school-published facts.** Every name in the release appears on
   a public athletics roster the athlete's own institution published, beside
   the same fields the row carries. v2.1.0 distributes no fact about any
   person that their school did not.
2. **Named records enable legitimate linkage.** Transfer tracking, honors and
   awards, draft and professional outcomes — the research uses that motivate
   a roster dataset — require joining records across sources by name.
   v2.0.5's de-identification structurally blocked them while providing, by
   its own assessment, only minutes of protection per row.

## 2. The public-facts standard

The release holds itself to a bright line: **every distributed field is an
institution-published fact from a public athletics roster, bio, or stats
page**. Nothing inferred, imputed, or third-party-joined ships in any public
file. Parsed columns are deterministic transformations of published strings.
The PII audit enforcing this ran clean over every staged file. This standard
is what "named release" means here — it is republication with provenance, not
profiling.

## 3. Precedent

Named distribution of exactly these facts is the industry and scholarly norm:

- **Commercial/reference:** Sports-Reference and College Basketball
  Reference, ESPN, 247Sports, and the NCAA's own stats portals publish named
  rosters, biographical roster facts, and season statistics for the same
  athletes, at larger per-athlete depth (photos, recruiting rankings) than
  this release.
- **The source institutions themselves:** every school athletics site
  publishes name + position + class + hometown + high school + height/weight
  as its standard public product.
- **Academic:** named athlete datasets — historical roster compilations,
  draft and outcome panels built from the same reference sites — are in
  routine scholarly use.

Precedent is not self-justifying (Part I §6: "already public" is a starting
point). Its role here is narrower: it establishes that the *norms of the
context* (Nissenbaum) treat named roster facts as public reference material,
and that this release does not create a novel exposure category.

## 4. Proportionality

What v2.1.0 adds to the world, versus what it risks:

- **No new sensitive attributes.** The delta over v2.0.5 is names plus four
  roster-card facts (major, previous school, height, weight — eight schema
  columns counting raw/parsed pairs) and stats
  sidecars — all already public per athlete. The delta over *the open web* is
  zero fields per athlete; the release is an aggregation of already-public
  pages, minus their photos, bios, contact routes, birthdates, and jersey
  numbers.
- **The risk added is bulk accessibility, not new facts** — analyzed in
  [DISCLOSURE_RISK.md](DISCLOSURE_RISK.md) §4. The mitigations target that
  risk specifically: no contact vectors to make a list actionable, no
  sensitive attributes to make it discriminatory, an opt-out to make it
  revocable per person, and a public ledger to make removals auditable.
- The proportionality claim, stated as a claim: the release's marginal risk
  channel (bulk operations on names) is bounded by controls aimed at exactly
  that channel, while its marginal value (population-scale, linkage-capable
  roster research) cannot be had any other way. This review judges the
  balance acceptable; it does not pretend the risk side is empty.

## 5. The affirmative line: race and income stay research-only

Naming rows makes the sensitive-attribute firewall **more** load-bearing, not
less, and this addendum re-certifies it as absolute: **BISG race/ethnicity
predictions, home-community income/SES measures, census-tract identifiers,
and Scorecard/Opportunity Insights mobility joins are never distributed** in
any file, and no released column derives from them
([DISCLOSURE_RISK.md](DISCLOSURE_RISK.md) §3). Attaching an inferred race or
an SES estimate to a *named* individual is the outcome every control in this
project exists to prevent. Third parties can run their own inference on names
— a property of names no release can revoke — but this project will not be
the distributor, the endorser, or the convenience layer for it.

## 6. Minors and Team IMPACT honorees

- **Early enrollees:** some listed athletes are 17; no DOB is carried, so
  they are named but not distinguishable. The controls that protect them are
  the same absences that protect everyone (no DOB, no contact info, no
  photos) plus a guardian-operable opt-out.
- **Team IMPACT honorees:** chronically ill children formally honored as team
  members and listed on real rosters, sometimes with distinctive entries
  (e.g., very low listed weights). Three decisions, each deliberate:
  1. **Kept as published** — they are genuine roster listings; silently
     deleting them would misrepresent the rosters and would itself single out
     ill children by editorial fiat.
  2. **Not flagged** — the dataset carries no column identifying them,
     because constructing one would create a health-adjacent attribute the
     source never published.
  3. **Explicitly covered by opt-out** — a parent or guardian can request
     removal ([OPT_OUT.md](OPT_OUT.md)), and
     [DISCLOSURE_RISK.md](DISCLOSURE_RISK.md) §5 names their presence rather
     than burying it.

## 7. The Zimmer objection, taken head-on

The strongest objection to v2.1.0 is Zimmer's (2010): *"the data is already
public" does not justify redistribution*, because aggregation and context
shift change what the data is. This release does not dodge the objection by
reciting publicness; it answers it in three parts:

1. **What aggregation actually changes here is accessibility, not
   publicity.** Each fact was deliberately published to the open web by the
   athlete's institution — publicity is not created by this release. What the
   release creates is a collapse in search cost: from 1,087 sites to one
   query; from per-page reading to bulk enumeration. That is a real,
   morally relevant change — it is simultaneously the entire research value
   and the entire marginal risk — and pretending it is neutral because "the
   facts were public" would repeat T3's mistake in a new key.
2. **T3's specific failures are not replicated.** T3 claimed anonymity it
   could not deliver, shipped non-public attributes (private-network data
   gathered by insiders), and offered subjects no recourse. v2.1.0 claims no
   anonymity; ships only institution-published fields with the sensitive
   tier firewalled; and offers standing, guardian-operable recourse with a
   public ledger. The Zimmer critique lands on datasets that *add* exposure
   while denying it; this release's added exposure is named, measured
   (DISCLOSURE_RISK §4), and mitigated where it can be.
3. **The mitigations are matched to the delta.** Since the delta is bulk
   accessibility of names, the controls are: no contact vectors (a list
   cannot reach anyone), no sensitive or inferred attributes (a list cannot
   rank anyone by race or income from this data), no photos/DOBs (a list
   does not become a dossier), opt-out with 14-day target propagating to all
   platforms (any person can leave), and the removals ledger (the leaving is
   verifiable). What remains — enumerability itself — is accepted and
   recorded as residual risk (DISCLOSURE_RISK §7), because the only controls
   that would remove it are un-naming or non-release.

## 8. Opt-out as an ongoing consent mechanism

No consent was obtained (Part I §1); that fact does not improve with time.
What v2.1.0 changes is the weight the opt-out must carry: for a named
release, opt-out functions as the standing, ongoing consent mechanism — every
listed person (or their guardian) holds a permanent, no-questions-asked exit
covering the combined file, the stats sidecars, and the samples, on all three
distribution platforms, recorded in a public ledger
([RELEASE_NOTES.md](RELEASE_NOTES.md), Removals). It remains a mitigation,
not consent — but it is now the release's central individual control, and its
14-day target and cross-platform propagation are commitments this review
treats as release-blocking if broken.

---
*Part I written 2026-07-07 against the v2.0.x de-identified releases; carried
forward with superseding annotations. Part II written 2026-08-14 against the
v2.1.0 staged build (513,655 rows / 1,087 schools / 28 sports). Next
scheduled review: 2026-27 release; out-of-cycle on any trigger in Part I §4.*
