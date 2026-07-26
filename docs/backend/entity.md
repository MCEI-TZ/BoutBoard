# `entity/`
#backend 
Location: `backend/src/main/java/com/mcei_tz/boutboard/entity/`

## Purpose

The JPA-mapped domain model — the objects that represent what's actually persisted in PostgreSQL: `Match`, `Participant`, `Judge`, `Category`, `ScoreEvent`, `Group`, `SportRuleSet`, and their relationships.

## Responsibilities

- Defining persisted fields and their column mappings.
- Defining relationships between domain concepts (foreign keys, cardinalities) via JPA annotations.
- Enforcing invariants that are inherent to the data itself (e.g., a field that can never be null at the database level).

## What belongs here

- `@Entity`-annotated classes, one per persisted domain concept.
- `@Embeddable` value objects shared across entities, if any emerge (e.g. a shared time-range value object).
- JPA relationship annotations (`@OneToMany`, `@ManyToOne`, etc.).

## What doesn't belong here

- DTOs — entities are never returned directly from `controller/`; they're converted to response DTOs by `mapper/` first. Exposing entities directly over the API would leak persistence details and make the API shape hostage to schema changes.
- Business logic beyond basic data invariants — a `MatchStatus` transition rule (e.g., "a finished match can't be reopened") is domain logic and belongs in `service/`, not as a method on the entity itself, keeping entities as data holders rather than a place business rules quietly accumulate.

## Naming conventions

Singular nouns matching the domain concept (`Match`, not `Matches`).

## Interactions

Read and written by `repository/`, converted to/from DTOs by `mapper/`, orchestrated by `service/`.

The full entity-relationship model — cardinalities, primary keys, foreign keys — is diagrammed separately in [Database](../architecture/Database.md) rather than duplicated here, since that's a cross-entity concern, not a single-folder one.