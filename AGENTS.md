# AGENTS.md

Fresh context? Start here.

## Quick reference

1. Read [`docs/OUTLINE.md`](docs/OUTLINE.md) first — living build plan and source of truth for architecture.
2. Read [`SKILL.md`](SKILL.md) for the thin runtime router (how an agent should run the skill).
3. Read [`README.md`](README.md) for the human-facing pitch.

If anything you are about to do disagrees with `docs/OUTLINE.md`, **stop and update the outline** (or ask the user) before coding. When the plan changes, alter `docs/OUTLINE.md` in the same change when possible.

## Locked direction (short)

- **B+D now:** operator stacks + prompt quality gate before `nlm`.
- **E later:** state-conditioned prompts once we have session remnants.
- **Pre-research:** Tavily + Firecrawl (top-N URLs) after CP0 — deps not installed until the user approves.
- **Out of scope for now:** Obsidian/vault writeback.

## Working rules

- Small slices. User commits and pushes.
- No new dependencies without explicit user approval.
- Do not treat chat history as more authoritative than `docs/OUTLINE.md`.
