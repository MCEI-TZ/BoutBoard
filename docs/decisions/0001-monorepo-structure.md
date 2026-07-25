# ADR-0001: Monorepo structure for frontend and backend
#decisions 
- **Status**: Accepted
- **Date**: 2026-07-25

## Context

BoutBoard consists of two codebases that must evolve together — a Spring Boot backend and a Vue frontend — plus project documentation. The project is maintained by a single developer, is open source from day one, and doubles as a portfolio piece that needs to be easy for an outside reviewer (a recruiter, a future contributor) to explore in one place.

A change to the WebSocket event contract, for example, almost always touches both the backend (`websocket/` handlers) and the frontend (`services/` STOMP client) in the same logical unit of work.

## Decision

BoutBoard is a single Git repository containing `backend/`, `frontend/`, and `docs/` as top-level folders, rather than separate repositories per component.

## Consequences

### Positive

- A single pull request can span both frontend and backend changes when a feature genuinely requires both — no coordinating merges across repos.
- One issue tracker, one set of GitHub Actions workflows, one version history. Simpler for a solo maintainer.
- Documentation lives alongside the code it describes, in the same repo, the same PR diffs, the same review process.
- A single link (`github.com/<user>/BoutBoard`) is enough for someone reviewing the project — nothing is fragmented across multiple repos.

### Negative / trade-offs

- CI naively configured would rebuild and retest both frontend and backend on every push, even when only one changed. Mitigated with path-based triggers in GitHub Actions (see `../workflows/ci-cd.md`) so only the affected part runs.
- If the project ever grows to have separate frontend and backend teams with independent release cadences, a monorepo makes that separation harder than starting with two repos would have. Not a current concern at this project's scale, but worth revisiting if that changes.
- Repo size and commit history grow faster than either component would alone.

## Alternatives considered

- **Polyrepo** (separate `boutboard-backend`, `boutboard-frontend`, `boutboard-docs` repositories) — rejected for now. It adds coordination overhead (matching versions across repos, cross-repo PRs for one feature) that isn't justified by a single-maintainer project, and it splits a portfolio piece across multiple links instead of one coherent repo.