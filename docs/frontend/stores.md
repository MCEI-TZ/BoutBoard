# `router/`
#frontend 
Location: `frontend/boutboard_app/src/router/`

## Purpose

Vue Router configuration — the map between URL paths and `views/`. Answers "what shows up at this URL?" and nothing else.

## Responsibilities

- Defining routes and mapping them to views.
- Route parameters (e.g. `/display/:matchId`).
- Navigation guards — currently none; will gate Admin Panel routes once authentication exists (see `../architecture/authentication.md`).

## What belongs here

- `index.js` (or `index.ts`) defining the route table: `/admin` → `MatchControlView`, `/display/:matchId` → `DisplayView`, and so on.
- Route guards, once they exist.

## What doesn't belong here

- Business logic or data fetching — a route definition should only say _which component_ renders at _which path_, not fetch or prepare data for it. Data fetching happens once the view mounts, via `stores/`.

## Naming conventions

A single `index.js` is sufficient at this project's current size. If the route table grows large enough to be hard to scan, it can be split by feature area (e.g. `adminRoutes.js`, `displayRoutes.js`) and combined in `index.js` — not necessary yet.

## Interactions

Maps paths to `views/`. The distinction between the `/admin` route (protected, operator-facing) and `/display/:matchId` (public, read-only) is a routing-level concern that will matter once authentication is introduced.