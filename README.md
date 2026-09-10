# auto-nlm

An agent skill that drives the NotebookLM CLI (`nlm`) to build **fully sourced, well-configured notebooks** — and optionally Studio artifacts (slides, audio, flashcards, and friends).

The product is not “chat with NotebookLM.” It’s a **prompt system**: how the skill scopes a topic, writes deep-research queries, interrogates sources, and crafts Studio focus prompts so the notebook is actually usable.

Rebuilt in public from a private prototype. Early commits will look thin on purpose.

## Problem

Left alone, an agent asks NotebookLM one broad question, accepts the first fluent answer, and leaves you with a mushy notebook and weak artifacts.

## Approach

1. User asks a research question.
2. A high-quality LLM scouts the web with **Tavily** (dynamic search or deep research) and/or **Firecrawl**.
3. That same LLM writes a **pointed NotebookLM Deep Research prompt** from what it found.
4. We run that prompt through NotebookLM (`nlm`). Studio artifacts can come later.

Our work is **engineering those LLM prompts**, not a Python operator framework.

Obsidian / vault export is **out of scope**. The notebook is the deliverable.

## Project map

Agents and humans: start at [`docs/OUTLINE.md`](docs/OUTLINE.md). Update it when the plan changes. [`docs/OLD_FLOW.md`](docs/OLD_FLOW.md) is the original skill.

## Status

**Rebuilding (LLM scout → compose pipeline).**

- [x] Living outline + AGENTS entry
- [ ] `prompts/scout.md` and `prompts/compose-nlm-research.md`
- [ ] Wire Tavily/Firecrawl (after approval)
- [ ] Run through real `nlm`
- [ ] Optional Studio

## Who it's for

People who want NotebookLM notebooks that are sourced on purpose — especially if they use agent CLIs and care about prompt quality.

## License

MIT (added with the first code commit)
