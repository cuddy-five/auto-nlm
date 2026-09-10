# auto-nlm — project outline

**Status:** living plan (not a frozen spec)  
**Last updated:** 2026-09-10  
**Owner:** Corey (@cuddy-five)

This file is the map of what we are building **right now**.  
If the plan changes, **update this file in the same change** (or the next commit).  
If you are an agent (or a human) trying to understand the project, **read this first**.

---

## For agents

1. **Source this file** before changing architecture, phases, prompt engines, or deps.
2. **Update this file** when you (or the user) change any decision below. Same PR/commit when possible.
3. Do **not** treat older chat or a stale `SKILL.md` as more authoritative than this outline.
4. Do **not** add dependencies (Tavily, Firecrawl, etc.) until the user explicitly approves that dep.
5. Do **not** commit/push unless the user says to. Propose a small slice; they apply and commit.

---

## One-sentence goal

A **high-quality LLM** scouts the web with Tavily and/or Firecrawl, then writes a **pointed Google NotebookLM Deep Research prompt**. We ship the **LLM prompts** that make that handoff good.

---

## Product (what ships)

| In scope now | Out of scope (for now) |
|---|---|
| User query → LLM + web tools | Operator algebra / assembler / Python FactPack engine |
| Tavily dynamic search **or** Tavily deep research **or** Firecrawl | LangChain / LangGraph |
| LLM writes the NotebookLM Deep Research prompt from tool results | Obsidian / vault writeback |
| Engineered LLM instructions (the skill’s real product) | Replacing NotebookLM Deep Research itself |
| Optional Studio later | |

Deliverable: a **good NotebookLM Deep Research prompt** (then a good notebook). The scout is in service of that prompt.

---

## Architecture (locked 2026-09-10 — hard pivot)

```
User query
    → high-quality LLM (guided by our prompts)
    → tools: Tavily dynamic search | Tavily deep research | Firecrawl
    → LLM reads tool results
    → LLM writes NotebookLM Deep Research prompt
    → (later) nlm runs that prompt
```

We **engineer two instruction layers** (maybe more later):

1. **Scout prompt** — classify the *kind* of ask (topic-agnostic), write a Tavily/Firecrawl brief that asks for a small learning map, pick tools. Does **not** invent the sub-topic names.
2. **Composer prompt** — read the scout report (now the map exists), write the NotebookLM Deep Research prompt.

**Dropped:** Lego operators, assembler, code prompt-gate, FactPack-as-runtime. Those were a previous design.

**Still true from the old skill:** human can review the composed NLM prompt before `nlm`; named entities and source controls belong *inside* the composer prompt; Obsidian stays out.

---

## Sub-topics come from the scout (locked)

The host LLM must **not** invent 5–7 sub-topics for an arbitrary query. That is guessing from training data (the mushy-outline trap).

| Who | Knows | Does not know |
|---|---|---|
| Host LLM | How to ask for a map; *kind* of question | What the map’s parts are named |
| Tavily (scout) | What the web treats as the parts of this topic | Your NotebookLM prompt |

**5–7 is a budget** (“small learning map”), not a list the model invents.

**Topic-agnostic ask kinds** (property of the sentence, not the domain):

- learn / how does X work → mechanisms, in learning order
- X vs Y → comparison axes
- what’s going on with X → timeline / players / stakes
- is X true / does X work → claims, evidence, limits

**Same scout ask every time (idea):** decompose into 5–7 *fundamental* sub-topics for this kind of question — parts you’d need to understand or decide — not products, news, or listicles. Tavily fills in the names. Trim if it returns 4 or 8.

**Pipe:**

1. User query  
2. Host LLM → Tavily brief only (kind + budget + source prefs). No “transformers, backprop, …” yet.  
3. Tavily → report whose headings *are* the sub-topics  
4. Host LLM reads report → now knows the map  
5. Host LLM writes NotebookLM Deep Research prompt from that map  

If the map is wrong (touristy, missing a core part), **edit after the scout** (or one follow-up search). Do not make the first LLM omniscient.

---

## Phase map (working)

| Phase | Name | Who |
|---|---|---|
| 1 | User query | Human |
| 2 | Classify ask + write scout brief | Host LLM |
| 3 | Scout / decompose | Tavily (and/or Firecrawl) |
| 4 | Read map from scout report | Host LLM |
| 5 | Compose NotebookLM Deep Research prompt | Host LLM |
| 6 | (optional) Human review of that prompt | Human |
| 7 | Harvest | `nlm` Deep Research |
| 8 | Later | Vet, Studio, etc. |

---

## Deep Research lessons (feed the **composer** prompt)

1. Seed trusted sources when we have them.
2. Explicit Sources include/exclude (and scope) control lines.
3. ACG later (Analyze / Challenge / Gap) — not required for first composer slice.
4. Post-harvest vetting — later.
5. Citation asks NotebookLM understands (filename + page; URL for web).
6. Job-shaped prompts (survey vs narrow vs lit review vs market vs code).

---

## Repo layout (target)

```
auto-nlm/
  README.md
  SKILL.md                 # router + never-list; points at prompt files
  AGENTS.md
  docs/OUTLINE.md          # THIS FILE
  docs/OLD_FLOW.md         # original nlm-deep-dive (why we started)
  prompts/
    scout.md               # LLM: how to call Tavily / Firecrawl
    compose-nlm-research.md  # LLM: how to write the NLM Deep Research prompt
```

No `operators/` / `assembler/` / `gates/` unless we revive them on purpose and update this file.

---

## Build order (for now)

1. This outline + AGENTS + thin SKILL aligned to the LLM pipeline
2. Draft `prompts/scout.md` and `prompts/compose-nlm-research.md` (the product)
3. Wire Tavily/Firecrawl only after the user approves deps
4. Run against real `nlm`
5. Optional: human review checkpoint, vet, Studio

---

## Open questions

- Which “high-quality LLM” in production (host agent vs a named API).
- When the scout should pick Tavily **dynamic search** vs Tavily **deep research** vs Firecrawl.
- Whether the human must approve the composed NLM prompt before `nlm` runs (recommended: yes).

---

## Change log (short)

| Date | Change |
|---|---|
| 2026-09-10 | Initial outline: B+D now, E later, pre-research after CP0, no Obsidian |
| 2026-09-10 | **Hard pivot:** LLM + Tavily/Firecrawl scout → LLM composes NotebookLM Deep Research prompt. Operators/assembler/gates dropped as the engine. Product is engineered LLM prompts. |
| 2026-09-10 | **Sub-topic rule:** host LLM does not invent 5–7 parts; it classifies ask kind and briefs Tavily; syllabus comes from scout report. |
