# Approach

How Decisis works, what gets built in which order, and what is still undecided. For why the project exists, see `VISION.md`.

## Core idea

1. **Record** a decision as a small structured record.
2. **Anchor** it to the code it governs.
3. **Detect** when that anchored code changes, and mark the decision as needing reconfirmation.
4. **Serve** only currently valid decisions to agents while they work.
5. **Check** pull requests for anything that slipped through.

Correctness comes first. Everything served to an agent must be either verified current or clearly marked as uncertain.

## The decision record (draft)

Fields under consideration. Not final; the replay experiment will shape this.

| Field | Meaning |
| --- | --- |
| id | Stable identifier |
| title | One-line summary |
| decision | What was decided |
| rationale | Why, including what was rejected |
| status | proposed, accepted, needs reconfirmation, superseded |
| anchors | The code this decision governs (files, symbols) and a fingerprint of that code |
| source | Where it came from: commit, PR, session, document |
| author and dates | Who decided, when it was recorded, when last confirmed |
| supersedes | The decision this one replaces, if any |

## Staleness: by code change, not by age

A decision is flagged when the code it is anchored to changes in a meaningful way, never just because time passed. An old decision that still matches the code stays valid. A recent decision whose code changed gets flagged.

Anchor precision is the critical variable. Options, from coarse to fine:

1. Whole file: any edit flags. Expected to be very noisy.
2. Line ranges: break when unrelated lines shift.
3. Symbol level (function, type, module), using a code parser.
4. Symbol level, ignoring formatting and comments.

The replay experiment decides which level to use.

## Two ways a decision goes wrong

- **Staleness:** the anchored code itself changes. Anchoring catches this.
- **Violation:** new code elsewhere contradicts the decision while the anchored code stays the same (for example, raw SQL written in a handler when the decision says all database access goes through one module). Anchoring alone does not catch this. A second mechanism (simple rules or an LLM check) may be needed. The replay experiment will show how common this is.

## How decisions reach agents during a session

Ordered from least to most reliable:

1. **MCP tools:** agents can ask which decisions apply to a file. Depends on the agent choosing to call them.
2. **Skill or instructions:** tell agents to check decisions before editing and to offer to record new ones.
3. **Hooks:** where an agent supports them (for example Claude Code), automatically inject the decisions for a file before it is edited. No reliance on the agent remembering.
4. **Generated agent file section:** Decisis writes a section of `AGENTS.md` or `CLAUDE.md` containing only currently valid decisions, grouped by area. Regenerated on change, never hand-edited. Works with any agent that reads these files.

The pull request check is the last safety net, not the main feature.

## Architecture

- `decisis-core`: records, anchors, storage, staleness logic. Knows nothing about agents.
- `decisis` (CLI): the human interface, and what hooks and CI call.
- `decisis-mcp`: thin MCP layer over the core.
- `decisis-replay`: the validation experiment.
- `integrations/`: hooks, skills, plugins, GitHub Action. Thin wrappers only.
- `apps/site`: landing page (Next.js scaffold); a dashboard later if needed.

## Build order

| Step | What | Status |
| --- | --- | --- |
| 1 | Core and CLI: store decisions, anchor them, detect staleness | Next |
| 2 | Replay experiment: measure flag precision on real git history | Next |
| 3 | Serve decisions during sessions: MCP, generated agent file section, Claude Code hook | Planned |
| 4 | Record decisions from sessions: skill that lets agents propose decisions | Planned |
| 5 | Pull request check | Planned |
| 6 | Import existing `CLAUDE.md` / `AGENTS.md` / rules files and report stale parts | Planned |

## Validation: the replay experiment

1. Pick real repositories with long histories (ideally ones that keep architecture decision records, which give real decisions and real moments they changed).
2. At an old commit, record 20 to 30 decisions with anchors.
3. Replay later commits one by one and record every flag.
4. Label each flag as real or a false alarm, and note real changes that were missed.
5. Measure precision (share of flags that were real), recall (share of real changes caught) and flags per week.
6. Compare the anchoring levels listed above.

Set the bar before running it (for example: at least half of flags are real, and most real changes are caught). If the bar is missed, rethink the approach before building integrations.

## Principles

- **Correctness over coverage.** A smaller set of trustworthy decisions beats a large set of unverified ones.
- **Generated views, not hand-maintained files.** Humans and agents read rendered summaries; records are the source of truth.
- **Agent-agnostic core.** Every agent gets the same decisions through the thinnest possible adapter.
- **Feature filter.** Add a feature only if it makes decisions more accurate, easier to capture, or more widely enforced. Decline features that only add more kinds of things to remember.

## Open questions

- Storage: plain-text decision files in the repo (reviewed in pull requests, shared through git) plus a local index? Leaning yes; not yet decided.
- Which anchoring level gives acceptable precision?
- How to detect violations, not just staleness?
- How to make recording a decision take under a minute?
- How to handle decisions that span multiple repositories?
