# AGENTS.md

Instructions for AI coding agents working in this repository. Keep this file short: it is loaded into every session. Depth lives in `docs/`.

## What this project is

Decisis records the engineering decisions behind a codebase, links each decision to the code it governs, and flags the decision when that code changes. Agents read the decisions that are still valid while they work, so nobody (human or agent) redoes or re-explores what was already decided.

One line: **engineering decisions that check themselves.**

- Why it exists and who it is for: `docs/VISION.md`
- How it works and what we build in which order: `docs/APPROACH.md`
- Decisions made about this project itself: `docs/decisions/`

## Current stage

Early and experimental. The Rust workspace skeleton exists; the core logic does not yet. The next milestone is the **replay experiment**: replaying real git history to measure whether staleness flags are accurate enough to trust. Do not build integrations (MCP features, hooks, PR checks, web) ahead of that unless asked.

## Repository layout

| Path | Purpose | Status |
| --- | --- | --- |
| `crates/decisis-core` | The engine: decision records, anchors, storage, staleness checks | Skeleton |
| `crates/decisis-cli` | The `decisis` command line tool (package name `decisis`) | Skeleton |
| `crates/decisis-mcp` | MCP server exposing decisions to agents | Skeleton |
| `crates/decisis-replay` | Git-history replay experiment, never published | Skeleton |
| `integrations/` | Agent hooks, skills, plugins, GitHub Action | Planned |
| `apps/site` | Landing page (Next.js, pnpm workspace); a dashboard later | Scaffold |
| `docs/` | Vision, approach, decision records | Active |

## Commands

```sh
cargo build                                           # build all crates
cargo test                                            # run all tests
cargo run -p decisis                                  # run the CLI
cargo fmt --all                                       # format
cargo clippy --workspace --all-targets -- -D warnings # lint, must pass
pnpm install                                          # web deps (Node 22, pnpm)
pnpm site:dev                                         # run the landing page
pnpm site:build                                       # build it, CI runs this
```

Run `cargo fmt`, `cargo clippy` and `cargo test` before calling any change done.

## How to work here

1. **Check decisions first.** Before changing an area, read the relevant files in `docs/decisions/`. Do not contradict an accepted decision. If you think one is wrong, say so and propose a new decision record that supersedes it; do not silently work around it.
2. **Record new decisions.** When a meaningful design choice is made during a session, offer to write it up as a new file in `docs/decisions/` using the template in that folder's README.
3. **The maintainer is learning by building this step by step.** Prefer explaining, reviewing and suggesting over writing large amounts of code. Make small, focused changes, and only make big ones when explicitly asked. When you write code, explain the non-obvious parts.
4. **Keep the core independent.** `decisis-core` must not know about any specific agent, editor or MCP. Agent-specific logic belongs in `decisis-mcp` or `integrations/`, and those stay thin.
5. **Correctness over features.** Prefer a smaller feature that is reliably right over a larger one that is sometimes wrong. A wrong decision served to an agent is worse than none.
6. **Test the core.** Every behavior in `decisis-core` gets a unit test. Real-world scenarios from the replay experiment become test fixtures.

## Code conventions

- Rust 2024 edition, stable toolchain. Shared package settings and lints live in the root `Cargo.toml`; crates inherit them with `.workspace = true`.
- `unsafe` is forbidden workspace-wide.
- Add dependencies through `[workspace.dependencies]` in the root, then reference them with `name.workspace = true`.
- Library errors use typed errors (`thiserror`); binaries may use `anyhow`.
- Public items in `decisis-core` get doc comments.

## Vocabulary

Use these terms consistently in code, docs and messages:

- **Decision:** one recorded engineering choice, with its rationale.
- **Anchor:** the link from a decision to the code it governs (file, symbol).
- **Stale / needs reconfirmation:** the anchored code changed, so a human must confirm or update the decision.
- **Superseded:** replaced by a newer decision, kept for history, never served to agents.
- **Violation:** new code that contradicts a decision without changing its anchored code.

## Writing style

For docs, comments, commit messages and PR descriptions: plain and direct. Do not use dashes as connectors between sentences or clauses.
