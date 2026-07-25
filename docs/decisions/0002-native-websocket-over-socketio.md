# ADR-0002: Native WebSocket + STOMP instead of Socket.IO
#decisions 
- **Status**: Accepted
- **Date**: 2026-07-25

## Context

The Display view needs to receive score, penalty, and clock updates from the backend in real time, without polling. Two options were on the table: Socket.IO (a popular real-time library, commonly recommended as a "modern" choice) and Spring's built-in WebSocket support using the STOMP sub-protocol.

Socket.IO is not a thin wrapper over the WebSocket standard — it defines its own message protocol and is designed primarily around a Node.js server. Using it from a Spring Boot backend requires a third-party, community library (`netty-socketio`) rather than anything Spring ships or documents officially. There are known reports in the community of Socket.IO clients connecting to a plain Spring WebSocket server but failing to have their messages actually received, due to this protocol mismatch.

Separately, SockJS (a compatibility shim for browsers that historically lacked WebSocket support) was considered as a fallback layer on top of native WebSocket, but is largely unnecessary today — modern browsers support WebSocket natively, which is the environment this project targets.

## Decision

Use Spring's native, first-party WebSocket support with STOMP as the messaging sub-protocol on both ends: Spring Boot's `spring-boot-starter-websocket` on the backend, and `@stomp/stompjs` connecting directly over a native WebSocket on the frontend — with no SockJS fallback and no Socket.IO.

## Consequences

### Positive

- Backend relies entirely on official, documented Spring functionality — no third-party or community library standing in for a missing first-party integration.
- STOMP provides structured publish/subscribe semantics (named destinations, topic subscriptions) out of the box, instead of requiring a custom message-routing scheme built on raw WebSocket frames.
- Smaller dependency footprint on the frontend: no `sockjs-client`, since the fallback it provides isn't needed for this project's target browsers.
- Avoids the protocol-mismatch failure mode documented in the community around Socket.IO clients talking to non-Socket.IO servers.

### Negative / trade-offs

- Loses Socket.IO's built-in conveniences (automatic reconnection, rooms, namespaces) — equivalent behavior (reconnect logic, subscription management per match) has to be implemented explicitly using STOMP primitives.
- No fallback transport for browsers/networks that block raw WebSocket connections (the scenario SockJS existed for). Acceptable for this project's assumed environment (a scoring table on a local or event network), but would need revisiting if BoutBoard were ever deployed in a more locked-down network context.

## Alternatives considered

- **Socket.IO via `netty-socketio`** — rejected due to the protocol mismatch with browser Socket.IO clients and the lack of first-party Spring support; would mean maintaining a real-time layer outside the well-documented Spring integration path.
- **Raw WebSocket with a custom message format** (no STOMP) — rejected because STOMP's destination/subscription model solves a problem (structured pub/sub) that would otherwise have to be designed and maintained by hand, for no real benefit over using the existing sub-protocol.