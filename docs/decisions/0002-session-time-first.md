# 0002: Agents get decisions during sessions; the PR check is a safety net

- Status: Accepted
- Date: 2026-11-18
- Applies to: `crates/decisis-mcp`, `integrations/`, build order

## Context
The goal is that neither people nor agents redo or re-explore what was already decided. Catching a contradiction only at pull request time means the wrong work was already done.

## Decision
The primary feature is delivering valid decisions to agents while they work: hooks where supported, a generated section in agent instruction files for every other agent, and MCP tools. The pull request check comes later as the last safety net.

## Alternatives considered
- **PR check first:** easier to demonstrate, but catches problems after the work is wasted.
- **MCP tools only:** depends on agents choosing to call them, which is unreliable.

## Consequences
- Correctness matters more: a stale decision served mid-session misleads an agent immediately. Only decisions verified as current are served; stale ones are withheld or clearly marked.
- Integrations depend on what each agent supports (hooks exist in some agents, not all).
