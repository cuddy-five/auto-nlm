# AGENTS.md

Fresh context? Start here.

## Quick reference

1. Read [`docs/OUTLINE.md`](docs/OUTLINE.md) first — living build plan and source of truth for architecture.
2. Read [`docs/OLD_FLOW.md`](docs/OLD_FLOW.md) — **fundamental context**. What the original `nlm-deep-dive` skill did, and what we set out to improve. Read this to understand *why* the project exists and what must not be lost (checkpoints, named entities, Studio focus). Then trust **OUTLINE.md** for the *current* pipeline (LLM scout → compose NLM prompt).
3. Read [`SKILL.md`](SKILL.md) for the thin runtime router.
4. Read [`README.md`](README.md) for the human-facing pitch.

If anything you are about to do disagrees with `docs/OUTLINE.md`, **stop and update the outline** (or ask the user) before coding. When the plan changes, alter `docs/OUTLINE.md` in the same change when possible.

If you change the story of the old skill, update `docs/OLD_FLOW.md` too.

## Locked direction (short)

- **Current engine:** high-quality LLM + Tavily (dynamic search or deep research) and/or Firecrawl → LLM writes the NotebookLM Deep Research prompt.
- **Sub-topics:** host LLM never invents the part map; Tavily names the parts (`docs/OUTLINE.md`).
- **Exploring:** pydantic classifications + short LLM instructions so the host can identify the query and pick a better Tavily path. Not locked.
- **Our job:** engineer that handoff (`prompts/scout.md`, later compose). Keep docs fundamental while we explore.
- **Not the engine:** operator algebra, assembler, Python FactPack, LangChain/LangGraph.
- **Out of scope for now:** Obsidian/vault writeback.
- **Deps:** do not install Tavily/Firecrawl until the user approves.

## Working rules

- Small slices. User commits and pushes.
- No new dependencies without explicit user approval.
- Do not treat chat history as more authoritative than `docs/OUTLINE.md`.
