# NCAA All-Sports Rosters 2025-26 — Release Notes

## v2.0.3 — 2026-07-14 (audit correction; data tip 2026-07-17)

Comprehensive dataset + join audit on the public **19-column** tier. Schools
cross-checked against the official NCAA directory and web-verified for the 2025-26
season. **Schema unchanged.**

| | v2.0.2 | v2.0.3 |
|---|---:|---:|
| Athletes | 515,085 | **514,696** |
| Schools | 1,089 | **1,087** |
| Public columns | 19 | 19 |

### Why −389 athletes / −2 schools

Removed out-of-scope institutions that were **not NCAA members competing under NCAA
sponsorship in 2025-26**:

| School | Reason | Rows |
|---|---|---:|
| Shawnee State | Competing as NAIA that year | 172 |
| Johnson & Wales University Charlotte | NCAA exploratory / USCAA | 217 |

**Kept** six NCAA provisional members (Jamestown, Roosevelt, Point Park, Carlow, Regent,
Middle Georgia State). Johnson & Wales **Providence** remains (NCAA).

### What else changed

- **Conference labels** web-verified for 2025-26, including Notre Dame (per-sport
  ACC / Big Ten hockey / Independent football), SUNY Brockport → Empire 8, SUNY Canton →
  SUNYAC, Upper Iowa → GLVC, Penn College → United East, Vermont State Lyndon → North
  Atlantic, Salve Regina → NEWMAC, Norwich → GNAC, Utica → Empire 8, Marywood → Atlantic
  East.
- **High-school recovery** on public `high_school` (source-page restores where blank);
  analysis-tier `hs_category` patches applied in the private research build (not shipped
  here).
- Prior v2.0.2 conference spelling normalizations retained.
- **`by_sport/`** convenience slices rebuilt from the corrected master (203 CSVs;
  gender×division leaves sum to 514,696).
- Conference is **fully populated** (Shawnee State's intentional blank rows are gone with
  the school).

### Unchanged

- De-identification posture (no names).
- Public column set and semantics.
- DOI **10.57967/hf/9512** (cite with version **2.0.3**).

---

## v2.0.2 — 2026-07-14 (conference label normalization)

Cosmetic conference-label normalization on the public **19-column** tier. **No rows added
or removed** (515,085), **no schema change**, **no other field changed** — only six
conference spellings from different source feeds were normalized to their canonical
official names:

| Was | Now | Rows |
|---|---|---:|
| `Old Dominion Athletic Conf.` | `Old Dominion Athletic Conference` | 6,112 |
| `Southern California Intercollegiate Athletic Conf.` | `Southern California Intercollegiate Athletic Conference` | 4,893 |
| `Southern Intercol. Ath. Conf.` | `Southern Intercollegiate Athletic Conference` | 4,083 |
| `Atlantic 10 conference` | `Atlantic 10 Conference` | 597 |
| `Sunbelt Conference` | `Sun Belt Conference` | 416 |
| `New England Wrestling Assn` | `New England Wrestling Association` | 28 |

`Southern Conference` (D1) and `Southern Athletic Association` (D3) are **left distinct** —
they are genuinely different leagues. DOI **10.57967/hf/9512** (cite with version **2.0.2**).

## v2.0.1 — 2026-07-13 (label correction)

Conference/division label corrections on the public **19-column** tier after a full
**2025-26** re-verify. **Schema unchanged.**

| | v2.0 | v2.0.1 |
|---|---:|---:|
| Athletes | 515,393 | **515,085** |
| Schools | 1,089 | 1,089 |
| Public columns | 19 | 19 |

### Why −308 athletes

**Louisiana Christian University** (NAIA) roster rows had been mixed into the Louisiana /
UL Lafayette scrape path via `lcwildcats.net`. Those **308** rows are removed. No other
schools were dropped for this release.

### What else changed

- **Conference labels** corrected for 2025-26 vintage: stale realignment names and
  core-collision / join errors filled or fixed. Conference is now populated for
  **≈99.97%** of rows (empty only where intentional — see below).
- **Division labels** corrected where mislabeled; public `division` is the authoritative
  D1/D2/D3 field.
- **`by_sport/`** convenience slices rebuilt from the corrected master (203 CSVs;
  gender×division leaves still sum to the full master).

### Known intentional exceptions (not bugs)

| Case | Notes |
|---|---|
| **Shawnee State** | Empty `conference` (172 rows). NCAA D2 member competing as NAIA in 2025-26 — no NCAA conference affiliation that year. *(Removed entirely in v2.0.3.)* |
| **Independents** | `Independent` is correct for full independents including **Notre Dame**, **Maranatha Baptist**, and **Salem WV**, and for athletes on sport-specific independent schedules. |

### Unchanged

- De-identification posture (no names).
- Public column set and semantics.
- DOI **10.57967/hf/9512** (cite with version **2.0.1**).

---

## v2.0 — 2026-07-08

Individual-level roster data for **all 28 NCAA championship/emerging sports**, all three
divisions (D1/D2/D3), both genders, athletic year **2025-26** — the first year under the
House v. NCAA settlement (roster limits, revenue sharing, NIL).

- **515,393 athletes** across **1,089 schools** (~91.8% of officially reported NCAA
  participations; 96.9% of sponsor-list teams; 98.5% of D1).
- **De-identified** (no names, no contact info). Every field is an institution-published
  roster fact from official college athletics sites, validated against NCAA sponsor lists.
- **Public tier:** 19 columns. **Analysis tier:** 54 columns (adds Census town income,
  high-school public/private/religious classification, College Scorecard / mobility joins) —
  research-only; not distributed in this GitHub tree.

### Canonical record & DOI

- DOI: **10.57967/hf/9512** — https://doi.org/10.57967/hf/9512
- Hugging Face (record of origin): https://huggingface.co/datasets/dharits3/ncaa-college-athlete-rosters-2025-26
- Kaggle (mirror): https://www.kaggle.com/datasets/shahdha/ncaa-all-sports-rosters-2025-26

### What's in v2.0

- High-school public/private/religious classification (NCES CCD + PSS; fuzzy (name, state)
  match, threshold ≥85, ~98% precision; resolved 394,447 / 76.5%).
- Census-validated town income & population (ACS 2022; r=0.97 with the original field;
  correctly re-scaled small-town population).

### License

CC0-1.0 (public domain). Roster facts are not copyrightable (Feist v. Rural). Please do not
attempt re-identification. Per-record removal honored on request: dharits3@gmail.com.

### Citation

Shah, Dharit (2026). *NCAA All Sports Rosters 2025-26: An Individual-Level Dataset Across All
Divisions* (Version 2.0.3) [Data set]. Hugging Face. https://doi.org/10.57967/hf/9512
