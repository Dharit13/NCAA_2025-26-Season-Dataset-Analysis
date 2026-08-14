# Legal Notes — NCAA All Sports Rosters 2025-26 (v2.1.0)

> **This is an informational summary by the dataset author (Dharit Shah,
> dharits3@gmail.com, independent researcher). It is not legal advice, and no
> attorney-client relationship is created by reading it.** It records the
> legal analysis behind the release design so that users, archives, and
> schools can see the reasoning. Where authority is unsettled, that is stated.

The dataset: individual-level rosters for all 28 NCAA sports, 2025-26
athletic year — 513,655 athletes, 1,087 schools, D1/D2/D3 — scraped from
official school athletics sites. **As of v2.1.0 the public release is
named:** `first_name`/`last_name` are distributed columns, alongside major,
previous school, height/weight, and per-sport season-stats sidecars. Every
distributed field is an institution-published fact from a public roster, bio,
or stats page; per-athlete demographic predictions and SES joins exist only
in a research tier that is never distributed. Companions:
[DISCLOSURE_RISK.md](DISCLOSURE_RISK.md),
[ETHICS_REVIEW.md](ETHICS_REVIEW.md), [OPT_OUT.md](OPT_OUT.md).

---

## 1. FERPA

**Statute/regs:** 20 U.S.C. § 1232g; 34 CFR Part 99.

**Roster data — including names — is textbook directory information.** The
regulatory definition at [34 CFR § 99.3](https://www.law.cornell.edu/cfr/text/34/99.3)
expressly lists: **name**, photograph, **major field of study**,
**"participation in officially recognized activities and sports," "weight and
height of members of athletic teams,"** dates of attendance, degrees/honors,
and **most recent previous educational institution**. That list reads like a
roster page schema because roster pages are the canonical use case — and it
now also reads like this dataset's v2.1.0 schema: name, major, previous
school, height, and weight are distributed fields precisely because they sit
inside the enumerated directory-information categories the schools themselves
invoked when publishing them. Directory information may be disclosed without
consent under [34 CFR § 99.31(a)(11)](https://www.law.cornell.edu/cfr/text/34/99.31),
subject to the [§ 99.37](https://www.law.cornell.edu/cfr/text/34/99.37)
conditions: the school must give public notice of what it designates as
directory information and a window for students to **opt out** in writing.
Schools that publish rosters have made exactly this designation.

**FERPA obligations attach to funded institutions, not to third-party
re-users.** FERPA is a Spending Clause condition: it applies to educational
agencies and institutions receiving Department of Education program funds
([DOE, Protecting Student Privacy FAQ](https://studentprivacy.ed.gov/faq/which-educational-agencies-or-institutions-does-ferpa-apply);
overview: [CRS IF13155](https://www.congress.gov/crs-product/IF13155)). Its
redisclosure limits ([§ 99.33](https://www.law.cornell.edu/cfr/text/34/99.33))
bind parties that received education records *from an institution under a
consent exception* — they do not reach an independent researcher who
collected information the schools had already published to the open web under
the directory-information exception. There is no FERPA private right of
action even against schools (*Gonzaga Univ. v. Doe*, 536 U.S. 273 (2002),
[oyez.org/cases/2001/01-679](https://www.oyez.org/cases/2001/01-679));
enforcement is DOE funding conditions on institutions.

**Practical conclusion for this dataset:** the scraped roster facts — names
included — were disclosed by the schools themselves under FERPA's
directory-information exception, and FERPA imposes no obligation on this
project's collection or redistribution of them. v2.1.0 distributes names but
still less than what schools publish: no photographs, no biographical text,
no contact information, no birthdates.

**The one residual edge — sharper in a named release:** a student who filed a
§ 99.37 directory-information opt-out should never have appeared on a public
roster page at all. If one did, that is the school's compliance failure, not
a re-user's — but such a person could now appear *by name* in this data
through no fault of their own. The dataset's opt-out process
([OPT_OUT.md](OPT_OUT.md)) covers this remainder: any athlete, a
parent/guardian, or a school on an athlete's behalf can have the rows removed
without stating a reason, with a 14-day target and a public removals ledger.

## 2. Right of publicity / NIL

**The right of publicity targets commercial appropriation of identity** —
using a person's name/image/likeness to sell or endorse something — not the
dissemination of factual information about them
(overview: [Free Speech Center, MTSU](https://firstamendment.mtsu.edu/article/right-of-publicity/)).
A statistical dataset of institution-published facts performs no endorsement
and attaches no one's identity to any product.

**Controlling authority on named athlete facts — now directly on point.**
*C.B.C. Distribution & Marketing, Inc. v. Major League Baseball Advanced
Media, L.P.*, 505 F.3d 818 (8th Cir. 2007), cert. denied, 553 U.S. 1090
(2008)
([opinion PDF via Yale ISP](https://law.yale.edu/sites/default/files/area/center/isp/documents/c.b.c._distrib._mktg._v._major_league_baseball_advanc.pdf);
[Harvard L. Rev. note](https://harvardlawreview.org/wp-content/uploads/2008/02/CBC_v_MLBAM.pdf)):
the First Amendment protected a *commercial* fantasy-sports product's
unlicensed use of **players' actual names plus performance statistics**,
because the information was readily available in the public domain. In the
de-identified releases that holding applied a fortiori; for v2.1.0 —
athletes' actual names plus roster facts and season statistics, distributed
free and non-commercially — it is the fact pattern itself, at a weaker level
of commercial exploitation than the use the Eighth Circuit protected. For
college athletes specifically: *Daniels v. FanDuel, Inc.*, 109 N.E.3d 390
(Ind. 2018) (certified question), aff'd, 909 F.3d 876 (7th Cir. 2018)
([Ind. opinion](https://law.justia.com/cases/indiana/supreme-court/2018/18s-cq-134.html);
[7th Cir.](https://law.justia.com/cases/federal/appellate-courts/ca7/17-3051/17-3051-2018-11-29.html))
— fantasy operators' use of **college players' names, pictures, and
statistics** fell within the newsworthiness exception to Indiana's
right-of-publicity statute, read broadly to cover "all types of factual,
educational, and historical data."

**State NIL statutes do not change this.** The 30+ state NIL laws (and the
House settlement framework) govern *athletes' ability to be paid for
endorsements* and schools'/NCAA's ability to restrict that — they create no
new cause of action against factual data compilations, and they inherit the
right-of-publicity news/public-affairs exemptions (e.g., Cal. Civ. Code
§ 3344(d): "news, public affairs, or sports broadcast or account" is exempt;
[ABA overview of NIL landscape](https://www.americanbar.org/groups/entertainment_sports/publications/entertainment-sports-lawyer/spring-2024/beginners-guide-ncaa-name-image-likeness-rights/)).
Caveat honestly stated: right of publicity is 50-state common/statutory law
with real variation; C.B.C. and Daniels are one circuit and one state court,
and the First Amendment boundary is contested at the margins
(transformative-use splits). A free, non-commercial factual compilation sits
well inside the protected core those cases mark out; the margin matters for
**downstream commercial users**, addressed next.

**The license line, stated explicitly:** the dataset is released under
**CC0 1.0** ([LICENSE](LICENSE)), and **CC0 covers the database compilation
only. It does not grant, waive, or license any name/image/likeness or right
of publicity in any listed athlete.** Those rights belong to the athletes and
are not the maintainer's to convey. A downstream user who puts athlete names
to commercial or endorsement use — merchandising, advertising, implied
endorsement of a product — bears their own compliance burden under the
publicity and NIL law of every relevant state. This note is carried in the
release documentation as a no-endorsement clause; it puts downstream users on
notice that the license conveys no persona rights.

## 3. Scraping

Collection was **logged-out scraping of public web pages**. The Ninth Circuit
held the CFAA's "without authorization" concept inapplicable to publicly
available, no-login data — *hiQ Labs v. LinkedIn*, 31 F.4th 1180 (9th Cir.
2022, on remand from the Supreme Court)
([opinion](https://cdn.ca9.uscourts.gov/datastore/opinions/2022/04/18/17-16783.pdf))
— reaffirmed in *Meta v. Bright Data* (N.D. Cal. 2024, applying hiQ to
logged-out scraping;
[order](https://www.courtlistener.com/docket/66686783/meta-platforms-inc-v-bright-data-ltd/)).
**Caveats:** hiQ is one circuit and arose on a preliminary-injunction posture
(the case later settled with judgment against hiQ on *contract* grounds);
website terms-of-service claims are civil breach-of-contract questions, not
criminal access ones, and this project never accepted any school site's ToS
by account creation. Scraping was rate-limited against public pages of the
very institutions whose disclosures FERPA authorizes. The move to named
distribution changes nothing in this section: the facts doctrine and the
access analysis do not depend on which lawfully collected fields ship.

## 4. Copyright

**Facts are not copyrightable.** *Feist Publications v. Rural Telephone*, 499
U.S. 340 (1991)
([opinion](https://supreme.justia.com/cases/federal/us/499/340/)) — no "sweat
of the brow" protection; a compilation gets at most **thin** protection for
original *selection and arrangement*, which does not cover the underlying
facts. Roster facts (name, position, height, weight, hometown, high school,
major) are facts; schools' arrangement of them is not reproduced here. The US
has no sui generis database right (unlike the EU); Congress has repeatedly
declined to create one. Consequence: neither the schools' pages nor this
dataset's facts carry enforceable copyright in the data itself. Unchanged by
the named release — a name is as much a fact as a hometown.

## 5. Why CC0 + citation request

Because Feist leaves (at most) thin, uncertain rights in a factual
compilation, an attribution license (CC BY / ODC-BY) would assert conditions
on rights that likely do not exist — unenforceable and a known source of
license-stacking friction. **CC0 1.0** waives whatever thin rights exist and
is the default at Dryad, Figshare, and Dataverse for exactly this reason
([Dryad, "Why does Dryad use CC0?"](https://blog.datadryad.org/2011/10/05/why-does-dryad-use-cc0/),
[CC0 FAQ](https://wiki.creativecommons.org/wiki/CC0_FAQ)). It also satisfies
FAIR R1.1 (clear, machine-readable license) cleanly. Attribution is requested
as a scholarly-norms citation request, not a license condition. Two limits of
CC0, both stated in the release documentation rather than left implicit:

- **CC0 waives the maintainer's rights in the compilation; it conveys no
  third-party rights** — not the athletes' publicity/NIL rights (§2), and
  not any right to misuse names in ways other law forbids.
- **CC0 makes use conditions unenforceable**, so the release's use guidance
  (no individual-level sensitive inference, no re-identification-adjacent
  misuse) is a stated norm, not a term. See
  [DISCLOSURE_RISK.md](DISCLOSURE_RISK.md) §3.

The research tier (BISG predictions, SES/income joins, tract identifiers,
mobility joins) is not CC0; it is repo-internal, never distributed, and never
deposited anywhere.

## 6. The opt-out mechanism as a good-faith control

No law surveyed above requires a removal process for republished
directory-information facts. One exists anyway
([OPT_OUT.md](OPT_OUT.md)): row-level removal keyed on `athlete_id`, covering
the combined file, the stats sidecars, the per-sport splits, and the sample
files ("every distributed artifact" per OPT_OUT.md), propagating to the next version on
all three distribution platforms (Hugging Face, Kaggle, GitHub), 14-day
target, recorded in a public removals ledger
([RELEASE_NOTES.md](RELEASE_NOTES.md), Removals section — empty at v2.1.0).
Legally it functions as a good-faith control: it operationalizes the § 99.37
residual (§1), gives named individuals — including parents/guardians of
listed minors — a working remedy no statute obliges, and evidences the
non-exploitative character of the release should any margin question in §2
ever be tested.

---

*Rewritten 2026-08-14 for the v2.1.0 named release, carried forward from the
2026-07-07 notes; legal citations were verified against the sources linked
above as of the original writing.*
