# `components/`
#frontend 
Location: `frontend/boutboard_app/src/components/`

## Purpose

Reusable, presentation-focused pieces of UI — the building blocks that `views/` assemble into full screens. A component here should be usable in more than one context without knowing which screen it's on.

## Responsibilities

- Rendering UI based on the data passed to it.
- Emitting events when the user interacts with it, without deciding what should happen as a result.

## What belongs here

- Small, focused single-file components: `ScoreCounter.vue`, `MatchClock.vue`, `VictoryAnimation.vue`, `PenaltyBadge.vue`.
- Components that receive data via `props` and communicate outward via `emit` — not by reaching into global state directly.

## What doesn't belong here

- Page-level components tied to a specific route — those are `views/`.
- Direct API or WebSocket calls (`axios`, STOMP client usage) — components stay presentation-only; data comes from a parent view or a `stores/` binding, fetched via `services/`.

## Naming conventions

PascalCase, descriptive of what the component shows or does (not where it's used): `ScoreCounter.vue`, not `AdminPanelScoreBox.vue`.

## Interactions

Composed together inside `views/`. Receive their data as props — from a view directly, or from a view that reads a `stores/` value and passes it down.