# Decisions

Decisions made about Decisis itself. This folder is the project's own first use case: once the core works, these records move into Decisis's own format.

## Rules

- One decision per file: `NNNN-short-title.md`, numbered in order.
- Never rewrite an accepted decision. To change it, write a new record that supersedes it and set the old one's status to `Superseded by NNNN`.
- Agents: read the relevant records before changing an area, and offer to write a new record when a design choice is made in a session.

## Template

```md
# NNNN: Title

- Status: Proposed | Accepted | Superseded by NNNN
- Date: YYYY-MM-DD
- Applies to: paths or areas this governs

## Context
What situation or problem led to this decision.

## Decision
What we decided.

## Alternatives considered
What else was considered and why it was rejected.

## Consequences
What this makes easier, harder, or rules out.
```

## Index

| # | Decision | Status |
| --- | --- | --- |
| 0001 | Invalidate decisions by code change, not by age | Accepted |
| 0002 | Agents get decisions during sessions; the PR check is a safety net | Accepted |
| 0003 | Validate staleness precision with replay before building integrations | Accepted |
| 0004 | Rust core in a single open-source monorepo | Accepted |
| 0005 | Add features only if they make decisions more accurate, easier to capture, or more widely enforced | Accepted |
| 0006 | Store decisions as plain-text files in the repo | Proposed |
