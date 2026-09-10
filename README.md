# auto-nlm

An agent skill that drives the NotebookLM CLI (`nlm`) to build **fully sourced, well-configured notebooks** — and optionally Studio artifacts (slides, audio, flashcards, and friends).

The product is not “chat with NotebookLM.” It’s a **prompt system**: how the skill scopes a topic, writes deep-research queries, interrogates sources, and crafts Studio focus prompts so the notebook is actually usable.

Rebuilt in public from a private prototype. Early commits will look thin on purpose.

## Problem

Left alone, an agent asks NotebookLM one broad question, accepts the first fluent answer, and leaves you with a mushy notebook and weak artifacts.

## Approach

Keep the human at a few checkpoints. Everything else is orchestration around better prompts:

1. **Scope** — decompose the topic into concrete sub-topics (approve before any `nlm` call).
2. **Harvest** — named-entity deep research into a fresh notebook; curate sources instead of hoping.
3. **Interrogate** — structured multi-pass queries per sub-topic, with a thin quality gate and retries.
4. **Artifacts (optional)** — Studio focus prompts tuned per format (slides ≠ audio ≠ flashcards).

Obsidian / vault export is **out of scope** for this repo’s first life. The notebook *is* the deliverable.

## Design bets (why this can be better than the prototype)

| Old prototype habit | Rebuild direction |
|---|---|
| ~18KB `SKILL.md` loaded every turn | Thin router skill; load only the prompt file for the current phase |
| Static prompt blobs | Prompt recipes with slots (topic, entities, sub-topic, gaps from prior passes) |
| Vault synthesis as the finish line | Notebook quality + optional Studio bundle as the finish line |
| All passes described inline | Pass index in the skill; bodies live under `prompts/` and are read on demand |
| Highest leverage already known | Studio `--focus` prompts and research queries get the most design energy |

## Project map

Agents and humans: start at [`docs/OUTLINE.md`](docs/OUTLINE.md). Update it when the plan changes.

## Status

**Rebuilding.**

- [ ] Rewrite README to this framing *(this file)*
- [ ] Thin `SKILL.md` router (never-list + phase map + which file to open)
- [ ] Prompt recipe format + research / query / studio libraries
- [ ] Notebook lifecycle + source harvest loop
- [ ] Quality gate (small, testable)
- [ ] Optional Studio artifact step
- [ ] Install path + CI

## Who it's for

People who want NotebookLM notebooks that are sourced and configured on purpose — especially if they use agent CLIs and care about prompt quality.

## License

MIT (added with the first code commit)
