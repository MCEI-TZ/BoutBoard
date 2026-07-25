# Architecture decision records (ADRs)
#architecture #decisions 

This folder records _why_ BoutBoard is built the way it is, not just _what_ it does. Each file is a short, numbered record of one non-obvious technical decision: the problem, the choice made, its trade-offs, and what was rejected instead.

## Why this exists

Code and even the [Overview](../architecture/Overview.md) docs show the current shape of the system, but not the reasoning behind it. Without that reasoning written down, it's easy for a future contributor (including future-you) to "helpfully" revert a deliberate choice, not realizing it was already considered and rejected for a specific reason.

If you're about to propose changing something in this list, read the matching ADR first — the trade-off you're seeing may already be documented.

## Index

| ID   | Title                                                                                   | Status   | Summary                                                                                                                                                                                              |
| ---- | --------------------------------------------------------------------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0001 | [Monorepo structure for frontend and backend](0001-monorepo-structure.md)               | Accepted | One repository for `backend/`, `frontend/`, and `docs/`, instead of splitting them across separate repos.                                                                                            |
| 0002 | [Native WebSocket + STOMP instead of Socket.IO](0002-native-websocket-over-socketio.md) | Accepted | Real-time updates use Spring's first-party WebSocket/STOMP support rather than Socket.IO, avoiding a third-party protocol mismatch.                                                                  |
| 0003 | [Scoring rules as configuration, not per-sport code](0003-configurable-rules-engine.md) | Accepted | Match rules (points, duration, win conditions) are modeled as data (`SportRuleSet`), with WKF Karate Kumite as the default configuration, so new sports don't require rewriting the scoring service. |

## Status values

- **Proposed** — under discussion, not yet acted on.
- **Accepted** — the current, active decision.
- **Superseded by ADR-NNNN** — no longer the current approach; the file stays as history and links to whichever ADR replaced it.

An ADR is never edited to reflect a change of mind after acceptance. If a decision is reversed, a new ADR is written, and the old one's status line is updated to point forward to it.

## Adding a new decision

1. Copy [template](template.md) to `NNNN-short-slug.md`, using the next sequential number.
2. Fill in Context, Decision, Consequences, and Alternatives considered — the template explains each section.
3. Add a row to the index table above.
4. Open it as a pull request like any other change — see the root [CONTRIBUTING](../../CONTRIBUTING.md).

Not every choice needs an ADR — only ones where a reasonable person could have gone a different direction, and where that direction is worth knowing about later. A one-line naming preference doesn't need one; choosing a communication protocol or a persistence strategy does.