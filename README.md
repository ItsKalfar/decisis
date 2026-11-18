# Decisis

**Engineering decisions that check themselves.**

Decisis records the decisions behind your code, links each one to the code it
governs, and flags it when that code changes. AI coding agents read the
decisions that are still valid while they work, so nobody redoes or
re-explores what was already decided.

## Status

Early and experimental, built in public. The first milestone is measuring
whether staleness flags are accurate enough to trust, by replaying real git
history. The design may change as I learn.

## Why

Read [docs/VISION.md](docs/VISION.md) for the problem and
[docs/APPROACH.md](docs/APPROACH.md) for how it works.

## Development

    cargo build
    cargo test
    cargo run -p decisis

The landing page in `apps/site` uses Node 22 and pnpm:

    pnpm install
    pnpm site:dev

See [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

## License

Licensed under either of [MIT](LICENSE-MIT) or [Apache-2.0](LICENSE-APACHE),
at your option.

[![CI](https://github.com/ItsKalfar/decisis/actions/workflows/ci.yml/badge.svg)](https://github.com/ItsKalfar/decisis/actions/workflows/ci.yml)
