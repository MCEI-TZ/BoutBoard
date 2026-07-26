# `repository/`
#backend 
Location: `backend/src/main/java/com/mcei_tz/boutboard/repository/`

## Purpose

The data access layer — the only place in the application that speaks Spring Data JPA directly. Everything above this layer works with entities without knowing or caring how they're actually queried or persisted.

## Responsibilities

- CRUD operations on entities via Spring Data JPA.
- Custom query methods where the derived query naming convention isn't enough (via `@Query` or method-name query derivation).

## What belongs here

- Interfaces extending `JpaRepository` (or `CrudRepository`), one per aggregate root entity: `MatchRepository`, `ParticipantRepository`, `JudgeRepository`.
- Custom finder methods (`findByStatus`, `findByCategoryId`, etc.).

## What doesn't belong here

- Business logic — a repository answers "how do I fetch/store this?", never "should this be allowed?". Rule enforcement belongs in `service/`.
- DTO conversion — repositories work exclusively with entities; mapping happens afterward in `mapper/`.
- Direct use from `controller/` — repositories are only ever called from `service/`, preserving a single, testable layer between HTTP and the database.

## Naming conventions

Suffixed `Repository`, matching the entity it manages (`MatchRepository` for `Match`).

## Interactions

Called exclusively by `service/`. Operates on `entity/` objects. The schema these repositories query against is versioned through Flyway migrations — see [Database](../architecture/Database.md).