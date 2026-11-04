# ADR-0001 — Observable registry facts are not disciplinary verdicts

- **Status:** Proposed
- **Date:** 2026-11-03

## Context

The RBQ public open dataset lists **active licence holders**. It does not independently prove why a previously included record is now absent. Absence may reflect a real status change, corrections, identifier changes, source lag or data faults.

## Decision

1. Only claim observable facts: `LISTED_ACTIVE` or `NOT_LISTED_IN_ACTIVE_DATASET`, as of a fully validated official publication.
2. A change in registry presence requires two **valid, comparable** observations; never treat a first observation as a transition.
3. Emit `SOURCE_INCOMPLETE` when full-file integrity, unique licence identity, parsing or required source coverage cannot be established. Retain the caller's last good snapshot.
4. Class or other field changes may be reported only after verifying that the official resource and data dictionary support a stable mapping.
5. **Never emit `REVOKED`, `SUSPENDED`, `CANCELLED`, `INVALID_CONTRACTOR` or regulatory accusations** based solely on presence changes.
6. Give a review hint, not an automated contractor approval/rejection.
7. Include attribution and provenance, and avoid republishing unnecessary personal details.

## Alternatives rejected

- Inferring licence discipline from daily missing records.
- Querying arbitrary contractor names with ambiguous fuzzy matching as an MVP.
- Calling each absence a revocation to increase alert value.

## Consequences

Lower coverage but a defensible evidence contract. To claim the legal basis for loss of status, a future product would need an independently verified official status/disciplinary source and carefully defined semantics.

## Official sources

- https://www.donneesquebec.ca/recherche/dataset/licencesactives
- https://www.donneesquebec.ca/recherche/showcase/registre-des-detenteurs-de-licence-rbq
