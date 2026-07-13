# How this dataset compares to the official NCAA participation numbers

**Official source:** NCAA Sports Sponsorship and Participation Rates Report (1956-57 through
2024-25), updated 2025-09-04 ([PDF](https://ncaaorg.s3.amazonaws.com/research/sportpart/2025RES_SportsSponsorshipParticipationRatesReport.pdf)),
announced in the NCAA's 2025-09-15 press release. **The most recent official year is
2024-25** — the 2025-26 edition is expected around September–October 2026, which makes this
dataset the only public individual-level count for the 2025-26 athletic year at the time of
release.

Both sides use the same convention: *"student-athletes who participate in more than one
sport are counted in each sport"* (NCAA methodology note), and this dataset likewise carries
one row per athlete per sport-roster.

## Headline

| | NCAA official (2024-25) | This dataset (2025-26) |
|---|---|---|
| Championship sports | 554,298 (M 318,949 / W 235,349) | |
| + Emerging sports | 561,290 | |
| Comparable universe (excl. rifle — 298 participants (M 100 / W 198), not covered here) | 560,992 | **515,085** |
| Men / Women | 318,949 / 235,349 | 291,171 / 223,914 |
| Teams | 19,928 championship (+ emerging) | 19,608 covered of 20,241 official sponsor-list cells (96.9%) |

**This dataset holds 91.8% of the NCAA's reported athlete-participations (515,085 /
560,992), ≈96.9% of teams, and 98.5% of Division I.** (v2.0.1; −308 vs v2.0 from
Louisiana Christian removal — coverage % unchanged at one decimal place.)

## Why ~92% of athletes at ~97% of teams — the gap decomposed

1. **Missing teams (~3.1%).** 633 named sponsor-list cells (see `coverage_missing.csv`) —
   down from 911 before the 2026-07 recovery sweep — now demonstrably structural:
   dormant programs whose newest published roster is years old, gender-unsplittable
   combined pages, WAF-fortress sites, Puerto Rico campuses on separate league sites,
   and emerging sports whose schools publish no roster.
2. **Published-roster vs reported-squad-size (~4–8%, sport-dependent).** The NCAA's figures
   are season-cumulative squad sizes reported by school compliance offices — everyone on the
   squad at *any* point (tryout walk-ons, scout-team players, medical redshirts, mid-season
   cuts). A school's website roster is a curated point-in-time snapshot. Per-team, at ~99%
   team coverage (note the D1 football official figure blends FBS + FCS — **FBS alone
   averaged 146.6**, FCS 124.5, sharpening the structural point):

   | | Official squad size | Published roster (this dataset) |
   |---|---|---|
   | Football D1 | 135.9/team | 110.0/team |
   | Football D3 | 112.4/team | 106.1/team |
   | Basketball M D1 | 15.8/team | 14.9/team |
   | Basketball M D3 | 19.8/team | 17.7/team |

   At D1 football, ~1 in 5 reported participants never appears on the public roster. That
   slice exists only in compliance filings and is not scrapeable by construction.
3. **Vintage (roughly flat, not +2–3%).** The official number is 2024-25. Naive
   extrapolation of that year's +2.9% growth would put 2025-26 at ~565–575k — but 2025-26
   is the first year of the **House v. NCAA settlement**: hard roster limits replaced
   scholarship limits at the 319 of ~390 D1 schools (82%) that opted in (all power
   conferences bound automatically; the Ivy League and service academies opted out).
   Football is capped at 105 (vs official 2024-25 average squads of 146.6 FBS / 124.5
   FCS), baseball at 34, M/W basketball 15, M/W soccer 28, W volleyball 18, wrestling 30,
   etc. — softened in year 1 by "Designated Student-Athlete" grandfathering. Net expected
   D1 shrinkage in capped sports is ~4–8k; combined with continued D2/D3/emerging growth,
   the true 2025-26 universe is roughly **flat (~561–569k)**. The cuts also fall
   disproportionately on the never-published compliance tail (item 2), so the *scrapeable*
   universe shrinks even less — this dataset's D1 football 110.0/team already reflects the
   capped year.

**Verification.** A 2026-07-07 audit live-verified 32 team rosters against schools'
current websites: 30 matched exactly (including all 8 FBS controls), confirming that
published-roster counts are captured accurately and that the residual gap is structural,
not scrape error. The follow-up work landed 2026-07-07/08: the recovery sweep filled
278 of the 911 missing sponsor cells (+5,203 athletes, every filled cell re-verified
against its source page for school/sport/gender/season/names), the ~600 pipeline-dropped
athletes were restored (Shawnee State, Southwestern TX, UNE football, Salem College NC,
Vassar, Williams skiing — the last gender-split from official EISA race results), the
two known under-captures were re-scraped in full (Jacksonville St. football 33→116 via
a November wayback snapshot, Bryn Mawr W track 4→25), and a basketball residue pass
added 28 teams. 42 stale/next-season captures and two phantom rosters (Saint Johns MN W,
Wabash W) were caught by the verification passes and removed before shipping.

## Per-sport (this dataset 2025-26 vs official 2024-25)

| Sport | Official | This dataset | Ratio | Team coverage here |
|---|---|---|---|---|
| Football | 83,794 | 72,718 | 87% | 98.8% |
| Track & field outdoor (M+W) | 68,266 | 64,362 | 94% | 95.5% |
| Track & field indoor (M+W) | 63,881 | 60,912 | 95% | 96.9% |
| Soccer (M+W) | 59,946 | 54,677 | 91% | 99.2% |
| Baseball | 41,580 | 38,168 | 92% | 99.5% |
| Basketball (M+W) | 36,440 | 32,706 | 90% | 99.5% |
| Cross country (M+W) | 30,821 | 28,290 | 92% | 95.6% |
| Lacrosse (M+W) | 30,597 | 29,236 | 96% | 95.9% |
| Swimming & diving (M+W) | 23,129 | 20,880 | 90% | 95.6% |
| Softball | 21,916 | 20,717 | 95% | 97.5% |
| Tennis (M+W) | 15,610 | 14,228 | 91% | 96.8% |
| Golf (M+W) | 14,679 | 13,666 | 93% | 95.1% |

Official per-sport values from the same report; official football total (83,794) is the sum
of the report's FBS+FCS/D2/D3 rows (the printed "overall" cell excludes the D1 sub-rows).
Coed-sport caution in the official tables: rifle/skiing coed squads are counted in both
gender tables there.

*(Counts reflect the 2026-07-08 build, after the recovery sweep, audit restorations,
and basketball residue pass.)*
