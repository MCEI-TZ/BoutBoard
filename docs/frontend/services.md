# `services/`
#frontend 
Location: `frontend/boutboard_app/src/services/`

## Purpose

Encapsulates every way the frontend talks to the backend — REST calls and the STOMP WebSocket connection. This is the only folder that knows _how_ a piece of data is fetched or sent (which endpoint, which STOMP destination); everything else in the frontend only knows _that_ it can ask for it.

## Responsibilities

- Configuring and exposing the Axios client for REST calls.
- Establishing and managing the STOMP connection over native WebSocket (see ADR-0002): connecting, subscribing to match topics, publishing score events.
- Translating raw HTTP/STOMP responses into plain data — not into UI state directly.

## What belongs here

- `apiClient.js` — the configured Axios instance (base URL, interceptors).
- `matchService.js`, `participantService.js`, `judgeService.js` — REST calls grouped by resource.
- `socketService.js` — STOMP connect/subscribe/publish logic.

## What doesn't belong here

- Reactive UI state — a service returns data or emits events; it doesn't hold `ref`/`reactive` state itself. That's `stores/`'s job.
- Component-specific logic — services are UI-agnostic and could, in theory, be reused outside Vue entirely.

## Naming conventions

Suffixed `Service` for REST resource clients, or `Client` for lower-level wrappers (`apiClient.js`).

## Interactions

Called by `stores/`, which hold the resulting state. Views and components never call `services/` directly — they go through a store, so that state stays observable and consistent regardless of which component triggered the update. See [Request-lifecycle](../architecture/Request-lifecycle.md) and [Websocket-flow](../architecture/Websocket-flow.md) for the full round trip on both channels.