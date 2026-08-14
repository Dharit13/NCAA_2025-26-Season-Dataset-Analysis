# by_sport/ — per-sport slices (v2.1.0)

Per-sport slices of the combined file
(`data/ncaa_all_sports_rosters_2025-26_enriched.{csv,parquet}`; 513,655 rows total).
Every file carries the same locked 27-column schema — see
[../data/CODEBOOK.md](../data/CODEBOOK.md).

## Layout

```
by_sport/<sport>/
  all.parquet          full sport slice (27 cols)
  all.csv              CSV twin (empty strings for nulls; Parquet has typed nulls)
  CODEBOOK.md          per-sport coverage, stat-column vocabulary, sport-specific caveats
  <gender>/            one directory per competition gender the sport sponsors
    all.csv            all divisions
    d1.csv  d2.csv  d3.csv
  stats.parquet        season-stats sidecar — 10 sports only (see below)
  stats.csv
```

Single-gender sports (e.g. football, softball) have one gender directory, and its
`all.csv` is identical to the sport-level `all.csv`. Per-sport row counts and coverage
rates: [../samples/sport_summary.csv](../samples/sport_summary.csv) and
[../metadata.json](../metadata.json).

## Stats sidecars

10 sports ship a season-stats file with **sport-specific columns by design** (never
merged into one wide cross-sport file): baseball (27,470 rows), basketball (26,818),
field_hockey (5,439), football (34,982), ice_hockey (6,345), lacrosse (24,789), soccer
(42,750), softball (17,160), volleyball (17,767), water_polo (1,612). One row per
athlete with ≥1 published stat line; column definitions in each sport's `CODEBOOK.md`.

Join on `athlete_id`:

```python
roster = pd.read_parquet("by_sport/soccer/all.parquet")
stats  = pd.read_parquet("by_sport/soccer/stats.parquet")

df = roster.merge(stats, on="athlete_id", how="left")
```

Left-merge so roster athletes without stats survive with nulls — redshirts and
non-appearing reserves are real roster states, not join failures.

## Track shared-rows warning

`track_indoor` and `track_outdoor` **share source rows by design** — schools publish one
track & field roster, and each sport is materialized and reconciled independently
(distinct `tfi_`/`tfo_` id prefixes). The same athlete typically appears in both sports
(and often in `cross_country`). This matches the NCAA's own per-sport participation
convention. Never dedupe blindly across sports; pick a unit of analysis first.
