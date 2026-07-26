# `exception/`
#backend 
Location: `backend/src/main/java/com/mcei_tz/boutboard/exception/`

## Purpose

Centralizes error handling so that every layer can signal a problem the same way, and the API returns consistent, predictable error responses regardless of which layer raised the issue.

## Responsibilities

- Defining custom exception types for domain-specific error conditions.
- Translating exceptions into HTTP error responses in one place, via a global exception handler.
- Defining the shape of an error response.

## What belongs here

- Custom exception classes such as `MatchNotFoundException`, `InvalidScoreEventException`, `ParticipantAlreadyRegisteredException` — typically thrown from `service/`, where the business rule that's being violated is known.
- A `GlobalExceptionHandler` (`@RestControllerAdvice`) mapping each exception type to an HTTP status and error response body.
- An `ErrorResponse` shape describing what the client receives on failure (status, message, timestamp). It lives here rather than in `dto/` because it's tightly coupled to error handling and isn't part of a resource's normal request/response contract.

## What doesn't belong here

- The business validation logic that decides _when_ to throw — that stays in `service/`, which throws these exceptions; `exception/` only defines and catches them.

## Naming conventions

Exception classes suffixed `Exception`. The global handler is named `GlobalExceptionHandler`.

## Interactions

Any layer can throw an exception defined here, most often `service/`. Only `GlobalExceptionHandler` catches them, keeping error-to-HTTP-status translation in one place instead of scattered `try/catch` blocks across `controller/`.