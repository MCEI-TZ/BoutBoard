# `stores/`
#frontend 
Location: `frontend/boutboard_app/src/stores/`

## Purpose

Pinia stores — shared, reactive state that more than one component needs to read or react to: the current match's score, the clock, connection status to the WebSocket. If two components need to see the same value change at the same time, that value belongs in a store, not in a component's local state.

## Responsibilities

- Holding shared reactive state.
- Exposing actions that `views/` and `components/` call to trigger a state change (e.g. `startMatch()`, `awardPoint()`).
- Calling `services/` to fetch or send data, and updating state based on the result — including state updates driven by incoming STOMP messages.

## What belongs here

- One store per domain concern: `matchStore.js` (current match, score, clock), `participantsStore.js`, `judgesStore.js`. An `authStore.js` is planned alongside authentication.

## What doesn't belong here

- Direct HTTP or STOMP client code — a store calls `services/` to perform the actual communication; it doesn't construct requests or manage the socket connection itself. This keeps "what changed" (store) separate from "how we found out" (service).
- DOM manipulation or component-specific rendering logic.

## Naming conventions

Following Pinia convention: exposed as `useMatchStore()`, `useParticipantsStore()`, etc.

## Interactions

The hub between the UI and the outside world: read by `views/` and `components/`, populated and updated by calling `services/` — including reacting to STOMP messages pushed from the backend without an explicit call, since a subscription callback updates store state the same way an action does.