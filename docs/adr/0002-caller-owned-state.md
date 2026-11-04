# ADR-0002 — Caller-owned history before tenant watchlist hosting

- **Status:** Proposed
- **Date:** 2026-11-03

## Context

Recurring monitoring requires previous observations. A shared Actor store for customer watchlists would create tenant isolation and deletion obligations before product-market fit.

## Decision

The MVP accepts a bounded list of licence numbers and optional previous snapshots. Each successful observation returns a new snapshot of the **observed active-dataset presence**, selected fields and schema version. The caller is responsible for persisting snapshots and scheduling checks.

- A first observation always returns `FIRST_OBSERVATION`, even if the licence is not listed.
- Comparisons are deterministic and schema-versioned.
- A source failure is `SOURCE_INCOMPLETE` with no next snapshot.
- A verified absence is itself a recorded **observation**, but never a legal reason.
- No caller-specific watchlist or private metadata is retained between Actor calls.
- Public downloaded registry data may be safely cached, with attribution, update times and integrity constraints.

## Trade-offs

The consumer must store a snapshot and schedule subsequent runs. This reduces initial infrastructure, but may make the product harder to adopt than a hosted watchlist.

## Deferred

Hosted subscription state, user authentication, notifications, multi-register cross-checks and enforcement or sanctions interpretation.
