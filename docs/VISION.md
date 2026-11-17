# Vision

## The problem: comprehension debt

Code is now written faster than anyone can understand it, and AI coding agents are accelerating that. The reasoning behind the code (why a workflow works this way, which approach was tried and rejected, what must never change) lives in people's heads, old chat sessions and scattered notes. Weeks later it is gone.

The cost shows up as:

- **Re-exploration.** People and agents dig through code again to work out why something is the way it is.
- **Redoing.** An agent confidently reintroduces something that was deliberately removed, because nothing told it otherwise.
- **Inconsistency.** Different sessions, agents and teammates make conflicting choices in the same area.

## Where this came from

The maintainer hit this while building several products at once: a month after shipping a feature, the decisions behind it were impossible to recall, and AI agents started every session from zero. Agent memory files helped briefly, then grew bloated and stale until nobody could tell which parts were still true.

## The real problem

The problem is not that nothing gets remembered. **It is that nobody can tell which memories are still true.**

Memory that is never checked against the code decays silently. A stale memory handed to an agent is worse than no memory, because the agent acts on it with confidence.

## Why existing approaches fall short (as of November 2026)

- **Hand-written agent files** (`CLAUDE.md`, `AGENTS.md`, editor rules) grow without limit, mix current and outdated guidance, and are tied to the tools that read them.
- **Built-in agent memory** is usually tied to one tool, and sometimes to one machine or one repository. Some products expire memories by age, which deletes old decisions that are still valid and keeps recent ones that are wrong.
- **Team knowledge and context platforms** pull context from many sources for large organizations, but focus on finding information more than on keeping each piece of it provably current.

Some of these products do verify memories against code, and they are good. Decisis focuses on the gaps that remain: decisions (the why, not just conventions), invalidation driven by code changes instead of age, and working the same way across every agent.

## What Decisis is

A shared record of engineering decisions that keeps itself honest:

- Each decision is linked to the code it governs.
- When that code changes, the decision is flagged for reconfirmation. Decisions that still hold stay, however old they are.
- Agents receive the decisions that apply to what they are editing, during the session, not just at review time.
- A check at pull request time catches anything that slipped through.

## Who it is for

- **Developers** who want to know why code works the way it does, with an answer that is still valid.
- **AI coding agents** that should follow past decisions instead of guessing.
- **Reviewers and tech leads** who want to know when a change contradicts an earlier decision.
- **New team members** who need the reasoning behind a codebase.

The first user is the maintainer, on their own projects.

## Non-goals

- Not a general memory store for chat transcripts or personal preferences.
- Not enterprise search across every company tool.
- Not a documentation wiki.
- Not tied to any single agent, editor or model.

## Goals of the project

1. Solve the maintainer's own problem first, then see whether teams share it.
2. Learn: about AI agents, about building systems that stay correct, and about solving problems as they appear.
3. Build in the open. The design will change as real use and real data push back, and that is expected.
