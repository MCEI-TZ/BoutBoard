# `mapper/`
#backend 
Location: `backend/src/main/java/com/mcei_tz/boutboard/mapper/`

## Purpose

The single place where conversion between `entity/` and `dto/` happens, using MapStruct. Centralizing this conversion means there's exactly one place to look when a field seems to be missing or mismapped between what's stored and what the API returns.

## Responsibilities

- Converting entities to response DTOs.
- Converting request DTOs to entities (for creation/updates).
- Nothing beyond structural conversion — no business logic, no validation.

## What belongs here

- One `@Mapper` interface per entity that has a DTO counterpart: `MatchMapper`, `ParticipantMapper`, `JudgeMapper`, etc.
- MapStruct-generated mapping methods (`toResponse(Match)`, `toEntity(MatchRequest)`).

## What doesn't belong here

- Manual, hand-written field-by-field mapping scattered inside `service/` or `controller/` — if a conversion is needed, it belongs in a mapper, not inline wherever it's first needed. This is what keeps entity/DTO conversion from drifting out of sync across the codebase.
- Business logic disguised as mapping (e.g., computing a derived score total during mapping) — mapping should be structural only; derived values are computed in `service/` and passed in already computed.

## Naming conventions

Suffixed `Mapper`, named after the entity it maps (`MatchMapper` maps `Match`).

## Interactions

Used by `service/` to convert repository results into response DTOs before returning them to `controller/`, and to convert incoming request DTOs into entities before persisting via `repository/`.