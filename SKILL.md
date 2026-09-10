---
name: auto-nlm
description: >-
  Use when the user wants a fully sourced NotebookLM notebook. A high-quality
  LLM scouts with Tavily (dynamic search or deep research) and/or Firecrawl,
  then writes a pointed NotebookLM Deep Research prompt. Do not use for
  Obsidian vault writing. Do not invent operator/assembler pipelines.
---

# auto-nlm

The notebook is the deliverable. The path there is:

**user query → LLM + web tools → LLM writes NotebookLM Deep Research prompt → `nlm`**

See `docs/OUTLINE.md` (current plan) and `docs/OLD_FLOW.md` (original skill).

## How prompts get built (current)

1. Load `prompts/scout.md`. Call Tavily dynamic search, Tavily deep research, or Firecrawl as that file directs.
2. Load `prompts/compose-nlm-research.md`. Turn scout results into one NotebookLM Deep Research prompt.
3. Prefer showing that prompt to the user before running `nlm` (until we say otherwise).
4. Do **not** build operator stacks or a Python assembler unless OUTLINE.md is updated to bring them back.

Those `prompts/*.md` files are the product. Keep this skill thin.

## Never

- Skip reading `docs/OUTLINE.md` on a fresh session.
- Call `nlm` Deep Research with a vague topic-only prompt if scout tools were available.
- Add Tavily/Firecrawl dependencies without explicit user approval.
- Write Obsidian/vault notes as part of this skill.
- Reintroduce LangChain/LangGraph unless the user asks and OUTLINE.md is updated.

## Prerequisites

- `nlm` installed and authenticated (for harvest)
- Tavily / Firecrawl only after the user approves wiring them
