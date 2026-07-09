# NCAA All-Sports Rosters 2025-26 — v2.0

Individual-level roster data for **all 28 NCAA championship/emerging sports**, all three
divisions (D1/D2/D3), both genders, athletic year **2025-26** — the first year under the
House v. NCAA settlement (roster limits, revenue sharing, NIL).

- **515,393 athletes** across **1,089 schools** (~91.8% of officially reported NCAA
  participations; 96.9% of sponsor-list teams; 98.5% of D1).
- **De-identified** (no names, no contact info). Every field is an institution-published
  roster fact from official college athletics sites, validated against NCAA sponsor lists.
- **Public tier:** 19 columns. **Analysis tier:** 54 columns (adds Census town income,
  high-school public/private/religious classification, College Scorecard / mobility joins).

## Canonical record & DOI
- DOI: **10.57967/hf/9512** — https://doi.org/10.57967/hf/9512
- Hugging Face (record of origin): https://huggingface.co/datasets/dharits3/ncaa-college-athlete-rosters-2025-26
- Kaggle (mirror): https://www.kaggle.com/datasets/shahdha/ncaa-all-sports-rosters-2025-26

## What's in v2.0
- High-school public/private/religious classification (NCES CCD + PSS; fuzzy (name, state)
  match, threshold >=85, ~98% precision; resolved 394,447 / 76.5%).
- Census-validated town income & population (ACS 2022; r=0.97 with the original field;
  correctly re-scaled small-town population).

## License
CC0-1.0 (public domain). Roster facts are not copyrightable (Feist v. Rural). Please do not
attempt re-identification. Per-record removal honored on request: dharits3@gmail.com.

## Citation
Shah, D. (2026). *NCAA All Sports Rosters 2025-26: An Individual-Level Dataset Across All
Divisions* (Version 2.0) [Data set]. Hugging Face. https://doi.org/10.57967/hf/9512
