# 0006: Store decisions as plain-text files in the repo

- Status: Proposed
- Date: 2026-11-18
- Applies to: `crates/decisis-core` (storage)

## Context
Decisions must be reviewable, versioned and shared across a team and across agents. The first version should not require running a server.

## Decision (proposed)
Store each decision as a small plain-text file under `.decisis/` in the repository it governs, with a local index (rebuildable from the files) for fast lookups. Decisions are reviewed in pull requests like code and shared through git.

## Alternatives considered
- **Database or hosted service:** needed eventually for decisions spanning repositories, but adds infrastructure before it is useful.
- **One large file:** recreates the bloat problem and causes merge conflicts.

## Consequences
- History, review and sharing come from git for free.
- Decisions spanning multiple repositories are not handled yet.
- File format must be easy to read and merge. To be settled during core development.
