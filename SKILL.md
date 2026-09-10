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

1. Load `prompts/scout.md`. Classify the ask kind. Write a Tavily/Firecrawl brief that asks for a 5–7 part *fundamental* map — do **not** name those parts yourself.
2. Run the scout. Read the report. The headings / parts *are* the sub-topics (trim if needed).
3. Load `prompts/compose-nlm-research.md`. Write the NotebookLM Deep Research prompt from that map + scout evidence.
4. Prefer showing that prompt to the user before running `nlm` (until we say otherwise).
5. Do **not** build operator stacks or invent the syllabus from training data.

Those `prompts/*.md` files are the product. Keep this skill thin.

## Never

- Skip reading `docs/OUTLINE.md` on a fresh session.
- Invent 5–7 sub-topics from the model’s own knowledge before the scout returns. Classify the ask kind; brief Tavily to decompose; take the map from the report.
- Call `nlm` Deep Research with a vague topic-only prompt if scout tools were available.
- Add Tavily/Firecrawl dependencies without explicit user approval.
- Write Obsidian/vault notes as part of this skill.
- Reintroduce LangChain/LangGraph unless the user asks and OUTLINE.md is updated.

## Prerequisites

- `nlm` installed and authenticated (for harvest)
- Tavily / Firecrawl only after the user approves wiring them
