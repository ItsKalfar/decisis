# 0003: Validate staleness precision with replay before building integrations

- Status: Accepted
- Date: 2026-11-18
- Applies to: `crates/decisis-replay`, build order

## Context
The product only works if staleness flags are usually worth looking at. If most flags are false alarms, people and agents will ignore them, like flaky tests. This is the riskiest assumption and has not been measured.

## Decision
Before building MCP features, hooks or PR checks, build a replay experiment: record decisions at an old commit of real repositories, replay later commits, label each flag as real or false, and measure precision and recall across anchoring levels. Set the success bar before running it.

## Alternatives considered
- **Build integrations first and learn from users:** slower feedback, and early users would meet noisy flags.
- **Assume symbol-level anchoring works:** unproven, and it is the core of the product.

## Consequences
- Early work produces numbers and test fixtures instead of user-facing features.
- The results decide the anchoring approach and may change the design.
- `decisis-replay` is never published (`publish = false`).
