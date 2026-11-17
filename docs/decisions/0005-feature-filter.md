# 0005: Add features only if they make decisions more accurate, easier to capture, or more widely enforced

- Status: Accepted
- Date: 2026-11-18
- Applies to: roadmap, feature requests

## Context
The project starts narrow (a correctness-focused decision record) and adds features as they are requested. Unfiltered requests ("remember my preferences", "store whole sessions", "search everything") would turn it into a generic memory tool, competing directly with built-in agent memory and well-funded context platforms.

## Decision
A feature is accepted only if it makes decisions:
1. **more accurate** (better anchoring, staleness or violation detection),
2. **easier to capture** (recording from sessions, PRs, imports), or
3. **more widely enforced** (more agents, hooks, CI, multiple repositories).

Requests that only add more kinds of things to remember are declined or deferred.

## Alternatives considered
- **Build whatever users ask for:** fast growth in scope, loss of focus.
- **Fixed roadmap with no requests:** ignores real users.

## Consequences
- Some reasonable requests will be declined; the reason can point to this record.
- Requests from people with the actual problem weigh more than general wishes.
