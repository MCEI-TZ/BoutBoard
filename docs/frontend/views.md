# `views/`
#frontend 
Location: `frontend/boutboard_app/src/views/`

## Purpose

Page-level components, one per route: the Admin Panel screens and the Display screen. A view's job is to assemble `components/` into a full screen and wire them up to state and data.

## Responsibilities

- Assembling components into a complete screen.
- Reading from and acting on `stores/` (e.g., triggering a store action when the operator clicks "start match").
- Handling route parameters (e.g., a `matchId` from the URL) relevant to that screen.

## What belongs here

- Route-level components: `MatchControlView.vue` (Admin Panel's live scoring screen), `DisplayView.vue` (the public scoreboard for the TV), `ParticipantsView.vue`, `JudgesView.vue`.

## What doesn't belong here

- Small, reusable UI pieces meant to be used in more than one screen — extract those into `components/` instead of growing a view file indefinitely.
- Raw API/WebSocket client logic — a view calls a `stores/` action, which in turn calls `services/`; a view never constructs an HTTP request or a STOMP subscription itself.

## Naming conventions

Suffixed `View` (`DisplayView.vue`, not `Display.vue`), to make it visually obvious in the file tree which files are route targets versus reusable components.

## Interactions

Registered as route targets in `router/`. Composed from `components/`. Read and act on `stores/`, never calling `services/` directly — see `stores/` for why that indirection exists.