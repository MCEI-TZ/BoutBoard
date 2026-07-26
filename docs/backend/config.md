# `config/`
#backend 
Location: `backend/src/main/java/com/mcei_tz/boutboard/config/`

## Purpose

Holds Spring `@Configuration` classes — code that wires up beans and framework behavior, as opposed to code that implements a feature. If a class answers "how is this piece of the framework set up?" rather than "what does the application do?", it belongs here.

## Responsibilities

- Registering the STOMP/WebSocket endpoint and message broker.
- Configuring CORS so the frontend origin can call the REST API.
- Any future Spring Security configuration (authentication filter chain, password encoding beans).
- Cross-cutting bean definitions that don't belong to a specific feature (e.g., a shared `ObjectMapper` bean, OpenAPI/Swagger setup).

## What belongs here

- `WebSocketConfig` — registers the STOMP endpoint and message broker used by the `websocket/` layer.
- `CorsConfig` — allowed origins, methods, headers for the REST API.
- `SecurityConfig` _(planned)_ — see [Authentication](../architecture/Authentication.md).
- `OpenApiConfig` — API documentation setup, if added.

## What doesn't belong here

- Business logic — configuration classes should only _wire things up_, not make domain decisions. Domain logic belongs in `service/`.
- Feature-specific beans that are only ever used by one service — define those closer to where they're used instead of centralizing everything in `config/` by default.

## Naming conventions

Classes are suffixed `Config` (e.g. `WebSocketConfig`, `CorsConfig`).

## Interactions

`WebSocketConfig` defines the endpoint and broker that `websocket/` handlers rely on to receive and broadcast messages — see [Websocket-flow](../architecture/Websocket-flow.md) for the full message path. `CorsConfig` affects every REST call described in [Request-lifecycle](../architecture/Request-lifecycle.md).