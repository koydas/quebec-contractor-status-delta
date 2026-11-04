# Validation gates

## Gate 0 — Commercial demand, before building

- Contact ten qualified consumers of recurring contractor roster verification (integrators, marketplaces, procurement automation vendors).
- Show **real** successive RBQ official active-licence observations, with captured source timestamps.
- Pilot price and buyer value are hypotheses; ask for a paid commitment, not only interest.
- **Pass:** two independent paid pilots within 14 days, or one paid integration with a clear repeat-check plan.
- **Stop:** no willingness to pay, or consumers only need one-time free RBQ lookup.
- External budget ceiling: CAD 25; track human time.

## Gate 1 — Official data viability

- Confirm daily delivery, exact field names, stable IDs and class mapping against data dictionary and live files.
- Measure full file size, download duration, caching behavior and parsing resources.
- Prove missing IDs only after completeness checks, not from a partial HTTP response.
- Verify CC BY 4.0 attribution obligations.
- Check real examples of presence transitions; **never label an absence as suspension or revocation**.

## Gate 2 — Offline schema and correctness

| Scenario | Expected |
| --- | --- |
| No prior snapshot, listed | `FIRST_OBSERVATION`, next snapshot present |
| No prior snapshot, absent in validated complete file | `FIRST_OBSERVATION`, `NOT_LISTED_IN_ACTIVE_DATASET` |
| Prior listed → listed, same classes | `NO_CHANGE` |
| Prior listed → validated absent | `CHANGED`; presence delta; review required |
| Prior absent → listed | `CHANGED`; presence delta |
| Class list changes in same valid record | `CHANGED` with exact fields |
| Failed download, truncated JSON/CSV, duplicate ID | `SOURCE_INCOMPLETE` and null next snapshot |
| Incomplete state with non-null snapshot | Schema validation rejects |
| Successful state with null snapshot | Schema validation rejects |
| Unrecognized snapshot version | Explicit incomplete outcome, old snapshot retained |
| Two independent callers | No cross-caller state leakage |

Before publication, add test fixtures, negative schema cases and tests that a source exception never produces `NO_CHANGE`.

## Gate 3 — Apify execution

Prove Actor configuration, memory/CPU costs on the actual published data size, bounded IDs, correct dataset output, stable JSON and repeated runs with caller-owned snapshots.

## Gate 4 — Commercial evidence

Measure unique paying consumers, scheduled call repetition, contribution after Apify fees and actual platform costs, support hours and observed source change volume. Store impressions and unpaid runs are not revenue.
