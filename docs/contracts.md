# Agent-facing event contract — draft

**Design only.** All data below is illustrative. The authoritative ID/class column mappings must be checked against RBQ's published documentation before implementation.

## Example input

```json
{
  "watchlist": [
    {
      "licenceNumber": "EXAMPLE-LICENCE-NUMBER",
      "previousSnapshot": null
    }
  ],
  "fields": ["registryPresence", "licenceClasses"],
  "maxItems": 50
}
```

The MVP accepts **exact licence numbers only**. Name search, NEQ matching and arbitrary URL crawling are deferred. Output always contains one result per requested ID.

## Example output

```json
{
  "schemaVersion": 1,
  "licenceNumber": "EXAMPLE-LICENCE-NUMBER",
  "status": "FIRST_OBSERVATION",
  "verification": "VERIFIED_CURRENT_OBSERVATION",
  "registryPresence": "LISTED_ACTIVE",
  "changes": [],
  "requiresHumanReview": false,
  "observedAt": "2026-11-03T15:00:00Z",
  "source": {
    "datasetUrl": "https://www.donneesquebec.ca/recherche/dataset/licencesactives",
    "publishedAt": null,
    "attribution": "Régie du bâtiment du Québec — CC BY 4.0"
  },
  "nextSnapshot": {
    "schemaVersion": 1,
    "licenceNumber": "EXAMPLE-LICENCE-NUMBER",
    "registryPresence": "LISTED_ACTIVE",
    "licenceClasses": [],
    "sourceObservedAt": "2026-11-03T15:00:00Z"
  }
}
```

## Outcome rules

| Status | Meaning | Next snapshot |
| --- | --- | --- |
| `FIRST_OBSERVATION` | No compatible prior observation | Present, based on current verified source |
| `NO_CHANGE` | Last and current observations match | Present |
| `CHANGED` | Presence or explicitly covered fields differ | Present |
| `SOURCE_INCOMPLETE` | Cannot prove complete correct observation | **Null**; retain previous |

`registryPresence` values: `LISTED_ACTIVE`, `NOT_LISTED_IN_ACTIVE_DATASET`, `UNKNOWN`. Only the first two are valid with successful outcomes. On `SOURCE_INCOMPLETE`, presence is `UNKNOWN`.

`CHANGED` always includes exact before/after differences and `requiresHumanReview: true`. That does not indicate a legal restriction or disciplinary action. Schema version mismatch is an incomplete outcome, not an implicit reset. The `schemas/event.schema.json` draft encodes the snapshot/status relationship.

The publication date must be null if the source does not provide a defensible timestamp; the Actor's observation clock cannot substitute for publisher freshness.
