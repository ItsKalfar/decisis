# 0001: Invalidate decisions by code change, not by age

- Status: Accepted
- Date: 2026-11-18
- Applies to: `crates/decisis-core` (anchors, checks)

## Context
Agent memory decays silently. Hand-written memory files grow stale with no signal about which parts are still true. Some existing products expire memories after a period without use, which deletes old decisions that still hold and keeps recent ones that are already wrong. The decisions most worth keeping are often the oldest ones.

## Decision
Every decision is anchored to the code it governs. A decision is flagged for reconfirmation when its anchored code changes meaningfully. Age alone never expires or flags a decision.

## Alternatives considered
- **Time-based expiry:** simple, but removes exactly the long-lived decisions people forget.
- **LLM judgment on every read:** flexible, but costly, slow and not repeatable.
- **No invalidation:** recreates the bloated, untrustworthy memory problem.

## Consequences
- Anchor precision becomes the most important technical problem (see 0003).
- Checking a decision can be cheap and deterministic.
- Violations that happen outside the anchored code are not caught by this alone and need a separate mechanism.
