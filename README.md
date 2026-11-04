# Quebec Contractor Status Delta

**Evidence-backed changes in Quebec RBQ active-licence records, for automated contractor watchlists.**

> **Status: design only.** No Actor, paid customers, registry snapshots or verified commercial demand yet.

## Problem

An agent that monitors a roster of contractors needs to know when the **official active-licence dataset** changes—not just perform a one-time lookup. It must also distinguish a dataset anomaly from a meaningful change.

The [RBQ active-licences dataset](https://www.donneesquebec.ca/recherche/dataset/licencesactives) is published on Données Québec, refreshed daily and licensed CC BY 4.0. It contains **active** licences; it is **not a register of proven suspensions, cancellations or disciplinary findings**. RBQ also maintains a separate [licence holder registry](https://www.donneesquebec.ca/recherche/showcase/registre-des-detenteurs-de-licence-rbq), whose history and fields may differ.

**A licence absent from the active dataset is NOT automatically revoked, suspended or fraudulent.**

## MVP hypothesis

- Input: up to 50 **known RBQ licence numbers**, with last good caller-owned snapshots.
- Output: one result per licence: `FIRST_OBSERVATION`, `NO_CHANGE`, `CHANGED`, or `SOURCE_INCOMPLETE`.
- Compare official presence in the active registry and any **verified, documented** licence-class fields.
- Return exact before/after values, source timestamp where available, evidence and an advisory review hint.
- If a valid complete file does not list a watched licence, report `NOT_LISTED_IN_ACTIVE_DATASET` as an observed state; **do not claim a legal cause**.
- Source failure, incomplete file, duplicate IDs or ambiguous parsing must never emit a false absence or a `NO_CHANGE`.

This is **not** a contractor vetting certification, sanction search, lead-generation database, or legal compliance opinion.

## Documentation

- [Architecture and failure boundaries](docs/architecture.md)
- [Draft event contract](docs/contracts.md) and [JSON Schema](schemas/event.schema.json)
- [ADR-0001: active-dataset semantics](docs/adr/0001-active-dataset-semantics.md)
- [ADR-0002: caller-owned state](docs/adr/0002-caller-owned-state.md)
- [Business, source and correctness gates](docs/validation.md)
- [Apify publication checklist](docs/apify.md)

## Commercial gate

Existing RBQ lookup products compete in this segment. Our **unverified** differentiation is a reproducible, narrow change event suitable for recurring automation. Before developing the Actor, test demand with a real example of two official observations and a *paid* pilot offer to qualified integrators.

## Attribution

Data source: Régie du bâtiment du Québec, [Liste des licences actives](https://www.donneesquebec.ca/recherche/dataset/licencesactives), published via Données Québec under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). This project is not affiliated with or endorsed by RBQ.
