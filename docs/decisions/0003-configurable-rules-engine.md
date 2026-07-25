# ADR-0003: Scoring rules as configuration, not per-sport code
#decisions 
- **Status**: Accepted
- **Date**: 2026-07-25

## Context

BoutBoard's initial and primary use case is WKF Karate Kumite: point values (Yuko = 1, Waza-ari = 2, Ippon = 3), a fixed match duration, an automatic win condition at an 8-point lead, and a tiered penalty system. The stated goal, however, is for the project to extend to other point-based combat sports later, without becoming a rewrite each time.

The straightforward path — write the scoring service directly against Kumite's specific rules — would deliver the current requirement fastest, but every rule (point values, duration, win conditions) would be embedded in code as constants or conditionals, tied to one sport.

## Decision

Model a sport's scoring rules as data — a `SportRuleSet` — rather than as sport-specific branches in the scoring service:

- Point types and their values (e.g., Yuko/Waza-ari/Ippon and their point worth)
- Match duration
- Win conditions (e.g., point-gap threshold that ends a match early)
- Penalty categories and their escalation tiers

The scoring service is written once, generically, against whichever `SportRuleSet` is active for a given match. WKF Karate Kumite ships as the built-in default configuration — it is data the system reads, not a special case the code branches on.

## Consequences

### Positive

- Supporting a new point-based combat sport becomes primarily a configuration/data task (define its `SportRuleSet`), not a new code module or a modification to the scoring service.
- The scoring service's logic (award a point, check for a win condition) is tested once against the rule-set abstraction, rather than needing duplicated test suites per sport.
- Kumite's rules are transparently visible as data, which also makes them easy to document and verify against the official WKF ruleset rather than buried in conditional logic.

### Negative / trade-offs

- More upfront design effort than hardcoding Kumite directly — the `SportRuleSet` shape has to anticipate what varies across sports (point values, durations, win conditions, penalty tiers) before a second sport exists to validate those assumptions against.
- If a future sport needs a fundamentally different scoring _mechanism_ (not just different numbers — e.g., a sport scored by judge panel consensus rather than discrete point events), the current rule-set model may not stretch to cover it without a follow-up design change. This ADR covers rule _parameters_, not scoring _mechanisms_.
- Slightly harder to reason about "what happens in a Kumite match" by reading code alone — part of the behavior lives in configuration data, which needs to be documented clearly (see `../architecture/database.md` once written) so it isn't a hidden dependency.

## Alternatives considered

- **Hardcode Kumite rules directly in the scoring service** — fastest for the current MVP, but exactly the approach that would require rewriting the scoring service for every future sport, which contradicts this project's stated goal.
- **Full plugin/module system per sport** (each sport as an independently deployable module with its own logic) — rejected as over-engineering for a project maintained by one developer on a multi-week timeline. Revisit only if a future sport genuinely needs a different scoring _mechanism_, not just different rule values.