# `dto/`
#backend 
Location: `backend/src/main/java/com/mcei_tz/boutboard/dto/`

## Purpose

Defines the exact shape of data crossing the API boundary — what a client sends and what the API returns — independent of how that data is persisted. DTOs exist so that internal persistence details (JPA annotations, entity relationships, internal-only fields) never leak into the public API, and so the API shape can evolve without forcing a database migration, or vice versa.

## Responsibilities

- Defining request payloads with basic input validation annotations (`@NotBlank`, `@Min`, etc.).
- Defining response payloads shaped for what the frontend actually needs — not a 1:1 mirror of the database table.

## What belongs here

- `MatchRequest` / `MatchResponse`, `ParticipantRequest` / `ParticipantResponse`, `JudgeRequest` / `JudgeResponse`, and so on, one pair per resource exposed by `controller/`.
- Simple Bean Validation annotations on request fields.

## What doesn't belong here

- JPA annotations (`@Entity`, `@OneToMany`, etc.) — those belong in `entity/`. A DTO is a plain data carrier, not a persisted object.
- Business validation logic (e.g., "a match can't start without two participants") — that's a domain rule, not a shape constraint, and belongs in `service/`.
- Conversion logic to/from entities — that's `mapper/`'s responsibility.

## Naming conventions

Suffixed `Request` for incoming payloads, `Response` for outgoing ones. If this folder grows large, it may be split into `dto/request/` and `dto/response/` subpackages — not necessary at the current scale.

## Interactions

Consumed and produced by `controller/`. Converted to and from entities by `mapper/`. See [Request-lifecycle](../architecture/Request-lifecycle.md) for where DTOs sit in the overall request path.