# Opt-Out / Record Removal — NCAA All Sports Rosters 2025-26 (v2.1.0)

Any athlete listed in this dataset can have their rows removed, no questions
asked. As of v2.1.0 the dataset is **named** — `first_name`/`last_name` are
public columns — which makes this process the release's central individual
control ([ETHICS_REVIEW.md](ETHICS_REVIEW.md), Part II §8). It exists because
the dataset was built without notifying the athletes in it — removal is the
standing lever offered in place of the consent that was never sought.

## Who may request

- the athlete themself;
- a **parent or legal guardian** — some first-year athletes are 17 at
  enrollment, and some rosters include **Team IMPACT honorees** (children
  honored as team members), whose removal a parent/guardian can request the
  same way;
- an authorized representative (agent, attorney, school athletics/compliance
  staff).

Institutions may also request removal of an entire team's rows.

## How to request

Email **dharits3@gmail.com** with the subject line **"Dataset removal
request"** and either:

1. the **`athlete_id`** of the row(s) (from any distributed file), **or**
2. the athlete's **name + school + sport(s)** (season 2025-26).

Nothing else — no ID documents, no reason. Requests are **honored without
question**: demanding proof of identity would mean collecting more personal
data than the dataset itself holds, so a plausible connection to the named
athlete is sufficient.

Corrections (wrong hometown, misparsed high school, misspelled name, etc.) go
through the same channel.

## What gets removed, and when

Removal is keyed on `athlete_id` and covers **every distributed artifact**:

| Scope | Detail |
|---|---|
| Combined file | the athlete's row(s) in the master roster file (CSV and Parquet) |
| Per-sport files | the athlete's row(s) in every `by_sport` split (all / men / women / division files, CSV and Parquet) |
| Stats sidecars | all rows for that `athlete_id` in the per-sport season-stats files |
| Samples | any occurrence in the sample files |
| Platforms | propagates to the next published version on **all three**: Hugging Face (canonical), Kaggle (mirror), and GitHub |

| Step | Commitment |
|---|---|
| Removal | published in the next version on every platform, **target turnaround 14 days** from the request |
| Record | an entry in the public **removals ledger** — the Removals section of [RELEASE_NOTES.md](RELEASE_NOTES.md) — noting the version and the **count** of rows removed, never the requester's identity. The ledger starts empty at v2.1.0. |
| IDs | removed `athlete_id`s are retired and never reused |

Removal works even after the DOI is minted: a DataCite DOI locks the
repository against deletion and renaming, but not against content commits —
per-record removal is a normal file commit.

## Honest limits of removal

- **The sources:** removal affects this dataset only. **It cannot remove the
  school's own roster page**, the Wayback Machine's captures of it, NCAA
  records, or third-party sports sites — the dataset cannot un-publish what
  the institution published.
- **Downstream copies:** the data are CC0; copies made before a removal
  cannot be recalled. Removal guarantees the athlete is absent from every
  version published *from that point forward*, on every platform above.
- **Prior versions:** Hugging Face preserves git revision history, so earlier
  versions of the files remain technically accessible. On request, the
  maintainer will file a Hugging Face support ticket to purge the affected
  rows from historical revisions — documented here because it depends on
  platform action, not only the maintainer's.

## Contact

Maintainer: Dharit Shah — dharits3@gmail.com (or open a discussion on the
dataset's Hugging Face page). This process is linked from the dataset card
and reviewed at each annual release.
