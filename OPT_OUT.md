# Opt-Out / Record Removal — NCAA All Sports Rosters 2025-26

Any athlete listed in this dataset can have their rows removed, no questions asked.
This document is the process. It exists because the dataset was built without notifying
the athletes in it (stated plainly in [DATASHEET.md](DATASHEET.md) §3) — removal is the
standing lever offered in place of the consent that was never sought
([ETHICS_REVIEW.md](ETHICS_REVIEW.md) §1).

## Who may request

- The athlete themself;
- a parent or legal guardian (some first-year athletes are 17 at enrollment);
- an authorized representative (agent, attorney, school athletics/compliance staff).

Institutions may also request removal of an entire team's rows.

## How to request

Email **dharits3@gmail.com** with the subject line **"Dataset removal request"** and:

1. the athlete's **full name as it appears on the school's roster** (the published
   files carry no names, so this is needed to locate the rows via the internal
   source mapping);
2. **school**, **sport(s)**, and season (2025-26);
3. nothing else — no ID documents, no reason. Requests are **honored without
   question**: demanding proof of identity would mean collecting more personal data
   than the dataset itself holds, so a plausible connection to the named athlete is
   sufficient.

Corrections (wrong hometown, misparsed high school, etc.) go through the same channel.

## What happens, and when

| Step | Commitment |
|---|---|
| Acknowledgment | within **7 days** |
| Removal | the athlete's row(s) deleted from all distributed files in the **next published version, no later than 30 days** after the request |
| Record | an `ERRATA.md` entry noting the version and the **count** of rows removed — never the requester's identity |
| IDs | removed `athlete_id`s are retired and never reused |

Removal works **even after the DOI is minted**: a DataCite DOI locks the Hugging Face
repository against deletion and renaming, but not against content commits — per-record
removal is a normal file commit. (This is why the DOI is deliberately minted last; see
[RELEASE_CHECKLIST.md](RELEASE_CHECKLIST.md) Gate 4.)

## Honest limits of removal

- **Prior versions:** Hugging Face preserves git revision history, so earlier versions
  of the files remain technically accessible. On request, the maintainer will file a
  Hugging Face support ticket to purge the affected rows from historical revisions —
  documented here because it depends on platform action, not only the maintainer's.
- **Downstream copies:** the data are CC0; copies made before a removal cannot be
  recalled. Removal guarantees the athlete is absent from every version published
  *from that point forward*.
- **The sources:** removal affects this dataset only. The school's own roster page,
  the Wayback Machine, and NCAA records are outside the maintainer's control — the
  dataset cannot un-publish what the institution published.

## Contact

Maintainer: Dharit Shah — dharits3@gmail.com (or open a discussion on the dataset's
Hugging Face page). This process is linked from the dataset card and reviewed at each
annual release.
