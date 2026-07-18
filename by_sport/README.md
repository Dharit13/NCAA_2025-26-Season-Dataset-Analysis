# Per-sport slices — NCAA All-Sports 2025-26

Convenience splits of the public master (`data/ncaa_all_sports_rosters_2025-26.parquet`) for users who want a single sport, gender, or division without filtering the full 514,696-row file. **Every file is a subset of the master with the identical 19-column, de-identified schema** — nothing here is new data.

## Layout
```
by_sport/
  <sport>/
    all.csv            all divisions, all genders
    CODEBOOK.md        files + row counts + column dictionary for this sport
    <gender>/          men/ and/or women/ (single-gender sports have one)
      all.csv          that gender, all divisions
      d1.csv d2.csv d3.csv   that gender, one division
```

**28 sports · 203 CSV files.** The gender×division leaf files sum to exactly 514,696 rows — the full master, no loss or duplication. 12 sports are single-gender (e.g. football/baseball = men; softball/field_hockey = women).

## Cautions
- **Conference:** fully populated (post v2.0.3). `Independent` includes Notre Dame (football), Maranatha Baptist, Salem WV, and sport-specific independents.
- **Origin / hometown_state:** US territories (PR/VI/GU/AS/MP) are `domestic` with the matching USPS code (v2.0.4). British Virgin Islands remain `international`.
- **De-identified** (no names). `hometown_*`/`high_school` are quasi-identifiers — no re-identification.
- **Do not sum `track_indoor` + `track_outdoor`** — they share source rows by design.
- For the full data dictionary, enrichment provenance, and coverage, see the top-level `data/CODEBOOK.md` and `DATASHEET.md`.

_Generated 2026-07-18 from the v2.0.4 territory-origin fix build._
