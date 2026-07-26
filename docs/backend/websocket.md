# `websocket/`
#backend 
Location: `backend/src/main/java/com/mcei_tz/boutboard/websocket/`

## Purpose

The real-time counterpart to `controller/`. Where `controller/` handles conventional HTTP requests, this folder handles messages sent and received over the STOMP channel — score events, penalty events, and clock updates flowing between the Admin Panel and the Display view.

## Responsibilities

- Receiving incoming STOMP messages from the Admin Panel (via `@MessageMapping` handlers).
- Delegating the actual state change to `service/` — this folder handles the _transport_, not the _decision_.
- Broadcasting resulting state changes to subscribed clients on the relevant topic.

## What belongs here

- Message-mapped handler classes (e.g. `ScoreSocketController`) that receive STOMP frames and call into `service/`.

## What doesn't belong here

- WebSocket/STOMP endpoint registration and broker configuration itself — that's `config/`'s `WebSocketConfig`. This folder assumes the channel already exists and handles messages flowing through it.
- Business logic (deciding whether a score event is valid, computing a winner) — delegated to `service/`, same as `controller/` does for REST.

## Naming conventions

Suffixed `SocketController`, mirroring the REST `Controller` naming pattern to make the parallel between the two entry-point types obvious.

## Interactions

Receives messages from the Admin Panel, delegates to `service/`, and broadcasts results toward the Display view. The complete event path — from operator click to spectator screen — is diagrammed in [Websocket-flow](../architecture/Websocket-flow.md).