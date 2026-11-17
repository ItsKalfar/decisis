# 0004: Rust core in a single open-source monorepo

- Status: Accepted
- Date: 2026-11-18
- Applies to: repository layout, `Cargo.toml`, licensing

## Context
The project has a core library, a CLI, an MCP server, an experiment, agent integrations, a landing page and possibly a dashboard. It is open source from day one, and the maintainer wants to learn by building it.

## Decision
- One repository. A Rust workspace (`crates/*`) holds the core and binaries. Web apps will live in a pnpm workspace under `apps/`.
- The core is written in Rust: it ships as a single fast binary, has strong code-parsing libraries, and is the maintainer's strongest language.
- Shared package settings and lints are defined once in the root `Cargo.toml`; `unsafe` is forbidden.
- Licensed MIT OR Apache-2.0, the Rust ecosystem default. Revisit before the first public release if the project may later need protection from hosted competitors.

## Alternatives considered
- **Separate repositories per component:** more overhead for issues, releases and docs.
- **TypeScript core:** closer to the web apps, but a heavier runtime for a CLI and hooks that run on every edit.

## Consequences
- Contributors need Rust, and Node only for web work.
- Integrations must call the CLI or MCP server rather than duplicate core logic.
