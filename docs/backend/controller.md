# `controller/`
#backend 

Location: `backend/src/main/java/com/mcei-tz/boutboard/controller/`

## Purpose

The REST entry points into the backend — the HTTP boundary. Controllers translate HTTP requests into calls on the service layer, and service results back into HTTP responses. They are intentionally "thin": no business logic lives here.

## Responsibilities

- Mapping HTTP verbs and paths to service calls (`GET /matches/{id}` → `matchService.getById(id)`).
- Validating request shape (via `@Valid` on request DTOs).
- Choosing HTTP status codes for responses.
- Nothing else.

## What belongs here

- One `@RestController` class per REST resource: `MatchController`, `ParticipantController`, `JudgeController`, `CategoryController`.
- Endpoint method signatures accepting and returning DTOs from `dto/` — never entities from `entity/` directly.

## What doesn't belong here

- Business rules or validation beyond basic request-shape checks — belongs in `service/`.
- Direct repository access — controllers call services, services call repositories. Never skip a layer.
- Entity-to-DTO conversion — that's `mapper/`'s job, invoked from the service layer, not the controller.

## Naming conventions

Classes are suffixed `Controller`, one per resource, matching the resource name in its request mapping (`MatchController` → `/matches`).

## Interactions

Controllers receive request DTOs, call `service/`, and return response DTOs. They never talk to `repository/` or `mapper/` directly. The full request lifecycle — controller through service, repository, and database — is diagrammed in [Request-lifecycle](../architecture/Request-lifecycle.md).

Real-time events (score updates, clock changes) do **not** go through this folder — those are handled by `websocket/` instead.