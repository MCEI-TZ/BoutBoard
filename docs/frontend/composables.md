# `composables/`
#frontend 
Location: `frontend/boutboard_app/src/composables/`

## Purpose

Reusable reactive logic built with the Vue Composition API — `use*` functions that encapsulate a piece of stateful behavior so it isn't duplicated across components. The distinguishing question versus `stores/`: a composable holds _local, per-usage_ reactive logic (each component using it gets its own instance), while a store holds _shared, singleton_ state (every consumer sees the same instance).

## Responsibilities

- Encapsulating reusable reactive behavior that isn't inherently global — a countdown timer's tick logic, for example, is the same code every match clock uses, but each instance of it is independent.

## What belongs here

- `useCountdownTimer.js`, `useMatchClock.js` — reactive logic reusable across components, returning refs and functions rather than rendering anything themselves.

## What doesn't belong here

- Logic only ever used by a single component — keep that local to the component instead of extracting a composable prematurely.
- Direct API calls — a composable that needs data calls `services/` or reads from `stores/`, the same as a component would.

## Naming conventions

Prefixed `use`, per Vue Composition API convention (`useMatchClock`).

## Interactions

Consumed by `components/` and `views/`. May read from `stores/` when the reusable logic needs access to shared state, but does not replace a store — if the logic's _result_ needs to be shared across independent components, that belongs in a store instead of a composable.