# `service/`
#backend 
Location: `backend/src/main/java/com/mcei_tz/boutboard/service/`

## Purpose

The business logic layer, and the busiest folder in the backend by design. This is where domain rules actually live: what makes a valid match, how a score event affects the current state, when a match ends. Every other backend folder exists to support this one — controllers expose it over HTTP, repositories give it data access, mappers translate its inputs and outputs.

## Responsibilities

- Enforcing domain rules (e.g., a match cannot start with fewer than two participants; a score event cannot be applied to a match that has already ended).
- Orchestrating calls to `repository/` and `mapper/`.
- Interpreting the active `SportRuleSet` when processing score events (see ADR-[0003](../decisions/0003-configurable-rules-engine.md)) — this is where "is this an automatic win?" gets decided, using rule-set data rather than sport-specific conditionals.
- Triggering WebSocket broadcasts when match state changes as a result of a business operation (in coordination with `websocket/`).

## What belongs here

- One `@Service` class per domain area: `MatchService`, `ScoreService`, `ParticipantService`, `JudgeService`.

## What doesn't belong here

- HTTP concerns (status codes, request/response shape) — that's `controller/`'s job.
- Direct JPQL/SQL — services call `repository/`, never the `EntityManager` directly.
- STOMP message handling itself (subscribing, message destinations) — that lives in `websocket/`, which calls into this layer to do the actual work.

## Naming conventions

Suffixed `Service`. Whether to introduce a `Service` interface alongside each implementation is left as a lightweight, evolving convention rather than a strict rule — start with a concrete class, and only extract an interface when a second implementation is genuinely needed (for example, a test double more specialized than a mock provides).

## Interactions

The central hub of the backend: called by `controller/` and `websocket/`, calls into `repository/` and `mapper/`. See both [Request-lifecycle](../architecture/Request-lifecycle.md) and [Websocket-flow](../architecture/Websocket-flow.md) — this folder appears in both flows.