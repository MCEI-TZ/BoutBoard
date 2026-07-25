# ADR-XXXX: Short, descriptive title
#decisions 
- **Status**: Proposed | Accepted | Superseded by ADR-YYYY
- **Date**: YYYY-MM-DD

## Context

What problem are we solving? What constraints (time, team size, existing architecture) shaped the decision? Describe the situation neutrally — a reader should understand _why this even needed a decision_ without already knowing the outcome.

## Decision

State the decision in one or two sentences, then elaborate if needed. Be concrete: name the technologies, patterns, or structures chosen.

## Consequences

### Positive

- What gets easier or better because of this decision.

### Negative / trade-offs

- What gets harder, what we're giving up, or what technical debt this introduces. Every real decision has a cost — if this list is empty, the trade-offs probably weren't considered hard enough.

## Alternatives considered

- **Option name** — why it was rejected (not just "it's worse," but the specific reason it didn't fit this project's constraints).

---

### How to use this template

1. Copy this file to `NNNN-short-slug.md`, where `NNNN` is the next number in sequence (check `README.md` for the last used number).
2. Never edit or delete an accepted ADR after the fact, even if the decision turns out to be wrong. If it's reversed, write a _new_ ADR that supersedes it, and update the old one's status line to point to the new one. The history of "we tried X, here's why we moved to Y" is more valuable than a clean-looking file.
3. Add a row to `README.md`.