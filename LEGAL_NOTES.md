# Legal Notes — NCAA All Sports 2025-26 Dataset

> **This is an informational summary by the dataset author (Dharit Shah,
> dharits3@gmail.com, independent researcher). It is not legal advice, and no
> attorney-client relationship is created by reading it.** It records the legal
> analysis behind the release design so that users, archives, and schools can see
> the reasoning. Where authority is unsettled, that is stated.

Companion docs: [README.md](README.md) (release design, tiers),
[OFFICIAL_COMPARISON.md](OFFICIAL_COMPARISON.md) (coverage). The dataset:
individual-level rosters for all 28 NCAA sports, 2025-26, scraped from official
school athletics sites; the **public tier is de-identified** (no names, no contact
info, no per-athlete demographic predictions); named rosters stay in per-sport
restricted folders and never leave the repo.

---

## 1. FERPA

**Statute/regs:** 20 U.S.C. § 1232g; 34 CFR Part 99.

**Roster data is textbook directory information.** The regulatory definition of
"directory information" at [34 CFR § 99.3](https://www.law.cornell.edu/cfr/text/34/99.3)
expressly lists: name, photograph, major field of study, **"participation in
officially recognized activities and sports," "weight and height of members of
athletic teams,"** dates of attendance, degrees/honors, and most recent prior
institution. That list reads like a roster page schema because roster pages are
the canonical use case. Directory information may be disclosed without consent
under [34 CFR § 99.31(a)(11)](https://www.law.cornell.edu/cfr/text/34/99.31),
subject to the [§ 99.37](https://www.law.cornell.edu/cfr/text/34/99.37) conditions:
the school must give public notice of what it designates as directory information
and a window for students to **opt out** in writing. Schools that publish rosters
have made exactly this designation.

**FERPA obligations attach to funded institutions, not to third-party re-users.**
FERPA is a Spending Clause condition: it "applies to educational agencies or
institutions that receive funds from programs administered by the U.S. Department
of Education" ([DOE, Protecting Student Privacy FAQ](https://studentprivacy.ed.gov/faq/which-educational-agencies-or-institutions-does-ferpa-apply);
overview: [CRS IF13155](https://www.congress.gov/crs-product/IF13155)). Its
redisclosure limits ([§ 99.33](https://www.law.cornell.edu/cfr/text/34/99.33))
bind parties that received education records *from an institution under a
consent exception* — they do not reach an independent researcher who collected
information the schools had already published to the open web under the
directory-information exception. There is no FERPA private right of action even
against schools (*Gonzaga Univ. v. Doe*, 536 U.S. 273 (2002),
[oyez.org/cases/2001/01-679](https://www.oyez.org/cases/2001/01-679)); enforcement
is DOE funding conditions on institutions.

**Practical conclusion for this dataset:** the scraped roster facts were
disclosed by the schools themselves under FERPA's directory-information
exception, and FERPA imposes no obligation on this project's collection or
redistribution of them. This dataset additionally strips names and contact
information from everything it distributes, which is more restrictive than what
FERPA permits schools themselves to publish.

**The one residual edge:** a student who filed a § 99.37 directory-information
opt-out should never have appeared on a public roster page at all. If one did,
that is the **school's compliance failure, not a re-user's** — but such a person
could exist in this data through no fault of their own. The dataset's opt-out
process (see README / datasheet: email the author; per-record removal is
supported across all published copies) covers this remainder: any athlete, or a
school on an athlete's behalf, can have their rows removed without stating a
reason.

## 2. Right of publicity / NIL

**The right of publicity targets commercial appropriation of identity** — using
a person's name/image/likeness to sell or endorse something — not the
dissemination of factual information about them
(overview: [Free Speech Center, MTSU](https://firstamendment.mtsu.edu/article/right-of-publicity/)).
A statistical dataset of institution-published facts performs no endorsement and
attaches no one's identity to any product; the public tier does not even contain
names.

**Controlling authority on athlete facts:** *C.B.C. Distribution & Marketing,
Inc. v. Major League Baseball Advanced Media, L.P.*, 505 F.3d 818 (8th Cir.
2007), cert. denied, 553 U.S. 1090 (2008)
([opinion PDF via Yale ISP](https://law.yale.edu/sites/default/files/area/center/isp/documents/c.b.c._distrib._mktg._v._major_league_baseball_advanc.pdf);
[Harvard L. Rev. note](https://harvardlawreview.org/wp-content/uploads/2008/02/CBC_v_MLBAM.pdf)).
The First Amendment protected a *commercial* fantasy-sports product's
unlicensed use of **players' actual names plus performance statistics**, because
the information was "readily available in the public domain." A fortiori it
protects a free, non-commercial, de-identified research dataset. For college
athletes specifically: *Daniels v. FanDuel, Inc.*, 109 N.E.3d 390 (Ind. 2018)
(certified question), aff'd, 909 F.3d 876 (7th Cir. 2018)
([Ind. opinion](https://law.justia.com/cases/indiana/supreme-court/2018/18s-cq-134.html);
[7th Cir.](https://law.justia.com/cases/federal/appellate-courts/ca7/17-3051/17-3051-2018-11-29.html))
— fantasy operators' use of **college players' names, pictures, and statistics**
fell within the newsworthiness exception to Indiana's right-of-publicity statute,
read broadly to cover "all types of factual, educational, and historical data."

**State NIL statutes do not change this.** The 30+ state NIL laws (and the House
settlement framework) govern *athletes' ability to be paid for endorsements* and
schools'/NCAA's ability to restrict that — they create no new cause of action
against factual data compilations, and they inherit the right-of-publicity
news/public-affairs exemptions (e.g., Cal. Civ. Code § 3344(d): "news, public
affairs, or sports broadcast or account" is exempt;
[ABA overview of NIL landscape](https://www.americanbar.org/groups/entertainment_sports/publications/entertainment-sports-lawyer/spring-2024/beginners-guide-ncaa-name-image-likeness-rights/)).
Caveat honestly stated: right of publicity is **50-state common/statutory law
with real variation**, C.B.C. and Daniels are one circuit and one state court,
and the First Amendment boundary is contested at the margins (transformative-use
splits). None of that margin plausibly reaches a names-free factual dataset.

**Practical conclusion for this dataset:** redistributing institution-published
roster facts — without names in the public tier, without commercial endorsement
use — does not implicate the right of publicity or state NIL statutes.
**Nonetheless the README carries a no-endorsement clause**: because the data is
CC0, downstream commercial use is permitted, and a downstream user who attached
athlete identities to a product endorsement *could* create publicity-rights
exposure for themselves. The clause puts them on notice that the license conveys
no publicity or endorsement rights.

## 3. Scraping

Collection was **logged-out scraping of public web pages**. The Ninth Circuit
held the CFAA's "without authorization" concept inapplicable to publicly
available, no-login data — *hiQ Labs v. LinkedIn*, 31 F.4th 1180 (9th Cir. 2022,
on remand from the Supreme Court)
([opinion](https://cdn.ca9.uscourts.gov/datastore/opinions/2022/04/18/17-16783.pdf)) —
reaffirmed in *Meta v. Bright Data* (N.D. Cal. 2024, applying hiQ to logged-out
scraping; [order](https://www.courtlistener.com/docket/66686783/meta-platforms-inc-v-bright-data-ltd/)).
**Caveats:** hiQ is one circuit and arose on a preliminary-injunction posture
(the case later settled with judgment against hiQ on *contract* grounds);
website terms-of-service claims are **civil breach-of-contract questions, not
criminal access ones**, and this project never accepted any school site's ToS by
account creation. Scraping was rate-limited against public pages of the very
institutions whose disclosures FERPA authorizes.

## 4. Copyright

**Facts are not copyrightable.** *Feist Publications v. Rural Telephone*, 499
U.S. 340 (1991) ([opinion](https://supreme.justia.com/cases/federal/us/499/340/))
— no "sweat of the brow" protection; a compilation gets at most **thin**
protection for original *selection and arrangement*, which does not cover the
underlying facts. Roster facts (name, position, height, hometown, high school)
are facts; schools' arrangement of them is not reproduced here. The **US has no
sui generis database right** (unlike the EU); Congress has repeatedly declined
to create one. Consequence: neither the schools' pages nor this dataset's facts
carry enforceable copyright in the data itself.

## 5. Why CC0 + citation request

Because Feist leaves (at most) thin, uncertain rights in a factual compilation,
**an attribution license (CC BY / ODC-BY) would assert conditions on rights that
likely do not exist** — unenforceable and a known source of license-stacking
friction. **CC0 1.0** waives whatever thin rights exist and is the default at
Dryad, Figshare, and Dataverse for exactly this reason
([Dryad, "Why does Dryad use CC0?"](https://blog.datadryad.org/2011/10/05/why-does-dryad-use-cc0/),
[CC0 FAQ](https://wiki.creativecommons.org/wiki/CC0_FAQ)). It also satisfies
FAIR R1.1 (clear, machine-readable license) cleanly. Attribution is requested as
a **scholarly-norms citation request** (see README), not a license condition —
the standard data-repository pattern. The restricted (named) tiers are not CC0;
they are repo-internal under data-use terms and are never deposited anywhere
(Zenodo's terms bar non-anonymized personal data; nothing named goes to HF
either).

---

*Written 2026-07-07 against build 2026-07-06; legal citations verified against
the sources linked above on that date.*
