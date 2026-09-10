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

Build an agent skill that writes **strong NotebookLM Deep Research prompts** (and optional Studio focus prompts) so notebooks are fully sourced and well configured — not a vault/Obsidian exporter.

---

## Product (what ships)

| In scope now | Out of scope (for now) |
|---|---|
| Scope a topic with user checkpoints | Obsidian / Zettelkasten / vault writeback |
| Pre-research the web to gather specifics for prompts | Full autopilot with no user gates |
| Assemble Deep Research + Studio prompts | Replacing NotebookLM’s own Deep Research |
| Gate prompts before calling `nlm` | Spec-compiler mini-language (deferred) |
| Optional Studio artifacts (slides, audio, flashcards, …) | |

Deliverable the user cares about: a **good NotebookLM notebook** (+ optional Studio outputs).

---

## Prompt engine (locked direction)

- **B — Operators (now):** small named pieces snapped into a stack; an assembler turns the stack into the string sent to `nlm`.
- **D — Prompt gate (now):** score/rewrite the draft prompt against a rubric **before** any `nlm` call.
- **E — State-conditioned (later):** later prompts use session state (gaps, entities seen, weak claims, failed sources).
- **Not v1:** fill-in-the-blank `{{slot}}` templates as the engine; notebook-spec compiler language.

Pre-research output becomes **inputs to operators** (and later fuel for E).

---

## Pre-research (scout)

**Tools (planned, not installed until approved):**

- **Tavily** — web research / discovery (what exists, names, links, open questions).
- **Firecrawl** — fetch a **small top-N** of those URLs for deeper text (not crawl-everything).

**Timing (working default):** after **CP0** (scope approved), before NotebookLM harvest.

**Job:** build a short “fact pack” (entities, seed URLs/docs, terms, exclusions, open questions) so Deep Research prompts are pointed.

Pre-research does **not** replace Deep Research. It makes *our* prompts specific.

---

## Phase map (working)

| Phase | Name | Notes |
|---|---|---|
| 0 | Scope | User approves 4–7 sub-topics (**CP0**) |
| 0.5 | Pre-research | Tavily → top-N Firecrawl → fact pack |
| 1 | Notebook | Create/name notebook in `nlm` |
| 2 | Harvest | Operators + gate → Deep Research; prefer seeds + include/exclude controls |
| 2b | Vet | Clean unreliable / duplicate / inaccessible sources |
| 3 | Interrogate | Multi-pass queries; ACG (Analyze / Challenge / Gap) over time |
| 4 | Artifacts (optional) | Studio `--focus` via studio operators + gate |
| 5 | Done | Checklist |

Checkpoints stay. No skipping CP0.

---

## Deep Research prompt lessons (from scout research)

Steal these into operators / gates (not as one giant static prompt):

1. Seed trusted sources before big runs when possible.
2. Explicit Sources include/exclude (and scope) control lines.
3. ACG loop: Analyze → Challenge → Gap → focused re-run.
4. Post-harvest vetting (“remove bad/duplicate; don’t invent metadata”).
5. Citation asks that match NotebookLM (filename + page for files; full URL for web).
6. Job-shaped prompts (survey vs narrow empirical vs lit review vs market vs code).

---

## Repo layout (target)

```
auto-nlm/
  README.md                 # human 60-second story (living)
  SKILL.md                  # thin router for agents using the skill
  docs/
    OUTLINE.md              # THIS FILE — project map for humans + agents
  operators/                # B — named Lego pieces (to build)
  assembler/                # B — stack → prompt string (to build)
  gates/                    # D — prompt (and later answer) rubrics (to build)
  prompts/                  # examples/docs only — not the runtime engine
  # pre-research client code — only after deps approved
```

---

## Build order (for now)

1. README + this outline + thin `SKILL.md` aligned to B+D+E-later + pre-research
2. First research operators + assembler stub + tests
3. Prompt-gate rubric + tests
4. Wire pre-research (after user approves Tavily/Firecrawl deps)
5. Harvest + vet prompts against real `nlm`
6. Interrogation operators; then E (state) when we have session data
7. Studio operators
8. Install path + CI

---

## Open questions

- Exact Tavily/Firecrawl APIs and auth setup (blocked on user approving deps).
- Top-N size for Firecrawl (recommendation: small, e.g. 3–8 URLs per sub-topic).
- Whether seed uploads into NotebookLM are automated via `nlm` or manual at first.

---

## Change log (short)

| Date | Change |
|---|---|
| 2026-09-10 | Initial outline: B+D now, E later, pre-research after CP0, no Obsidian, Deep Research lessons captured |
