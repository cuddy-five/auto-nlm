# auto-nlm — project outline

**status:** exploratory  
**Last updated:** 2026-09-11  
**Owner:** Corey (@cuddy-five)

Doc/prompt `status:` is one of **exploratory** (trying), **firm** (we mean it for now), **evergreen** (keep true). All current docs and prompts are exploratory.

This file is the map of what we are building **right now**.  
If the plan changes, **update this file in the same change** (or the next commit).  
If you are an agent (or a human) trying to understand the project, **read this first**.

Keep this document **fundamental and open**. Do not lock taxonomies, tool matrices, or packet schemas here while we are exploring.

---

## For agents

1. **Source this file** before changing architecture, phases, prompt engines, or deps.
2. **Update this file** when you (or the user) change any decision below. Same PR/commit when possible.
3. Do **not** treat older chat or a stale `SKILL.md` as more authoritative than this outline.
4. Do **not** add dependencies (Tavily, Firecrawl, etc.) until the user explicitly approves that dep.
5. Do **not** commit/push unless the user says to. Propose a small slice; they apply and commit.
6. Do **not** turn a “maybe / let’s try” into a rewrite of the repo.

---

## One-sentence goal

A **high-quality LLM** scouts the web with Tavily and/or Firecrawl, then writes a **pointed Google NotebookLM Deep Research prompt**. We ship the **instructions and types** that make that handoff good.

---

## Product (what ships)

| In scope now | Out of scope (for now) |
|---|---|
| User query → LLM + web tools | Operator algebra / assembler / Python FactPack engine |
| Tavily and/or Firecrawl (after approval) | LangChain / LangGraph |
| LLM writes the NotebookLM Deep Research prompt from tool results | Obsidian / vault writeback |
| Instructions + (exploring) a small classification model | Replacing NotebookLM Deep Research itself |
| Optional Studio later | |

Deliverable: a **good NotebookLM Deep Research prompt** (then a good notebook). The scout is in service of that prompt.

---

## Architecture (still true)

```
User query
    → high-quality LLM
    → tools: Tavily and/or Firecrawl
    → LLM writes NotebookLM Deep Research prompt
    → (later) nlm runs that prompt
```

**Dropped:** Lego operators, assembler, code prompt-gate, FactPack-as-runtime.

**Still true from the old skill:** human can review the composed NLM prompt before `nlm`; named entities belong in that prompt; Obsidian stays out.

The host LLM does **not** invent the sub-topic map from memory. The scout (Tavily/web) names the parts.

---

## What we are trying now

**Pydantic classification + short LLM instructions.**

Idea (basic-memory flavored): the host classifies the user query into a small typed object — categories and observations — instead of following a long rulebook. That classified ask should help the LLM **see what kind of question it is** and **pick a better path into Tavily** (what to ask, which mode, how to decompose).

Nothing about that model is locked: which fields, which categories, how much instruction text, or which Tavily mode follows from which class. We will find out by trying.

Do not pre-write the syllabus. Classification is about the *query*, not the topic outline.

---

## Phase map (working, not a spec)

1. User query
2. Classify the query (exploration: pydantic + short instructions)
3. Brief / run Tavily (and/or Firecrawl)
4. Read the map from the scout
5. Compose the NotebookLM Deep Research prompt
6. Optional human review of that prompt
7. Harvest with `nlm`
8. Later: vet, Studio

---

## Repo layout (target)

```
auto-nlm/
  README.md
  SKILL.md
  AGENTS.md
  docs/OUTLINE.md          # THIS FILE
  docs/OLD_FLOW.md
  prompts/
    scout.md               # short scout notes (exploration)
    compose-nlm-research.md  # later
```

No `operators/` / `assembler/` / `gates/` unless we revive them on purpose and update this file.

---

## Build order (for now)

1. Keep this outline open and fundamental
2. Try classify (pydantic + a little instruction) → better Tavily brief
3. Wire Tavily only after the user approves
4. Compose NLM prompt; run `nlm`
5. Optional: vet, Studio

---

## Open questions

- Shape of the classification model (fields, categories).
- How much LLM instruction is enough.
- When to use Tavily search vs research vs Firecrawl.
- Which host LLM in production.
- Whether the human must approve the composed NLM prompt before `nlm` (recommended: yes).

---

## Change log (short)

| Date | Change |
|---|---|
| 2026-09-10 | Initial outline: B+D now, E later, pre-research after CP0, no Obsidian |
| 2026-09-10 | **Hard pivot:** LLM + Tavily/Firecrawl scout → LLM composes NotebookLM Deep Research prompt. Operators/assembler/gates dropped as the engine. |
| 2026-09-10 | **Sub-topic rule:** host LLM does not invent the map; Tavily names the parts. |
| 2026-09-11 | **Exploration:** try pydantic classifications + short LLM instructions so the host can identify the query and pick a better Tavily path. Keep this outline fundamental and open. Do not lock a taxonomy. |
| 2026-09-11 | Docs/prompts get `status:` exploratory \| firm \| evergreen. All are exploratory. |
