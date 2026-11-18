# Contributing

Decisis is early. Issues describing your own "we forgot why we decided this"
moments are as valuable as code.

Before opening a pull request, run:

    cargo fmt --all
    cargo clippy --workspace --all-targets -- -D warnings
    cargo test

If you touched `apps/site`, also run:

    pnpm site:build
