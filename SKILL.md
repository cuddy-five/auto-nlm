---
name: auto-nlm
description: >-
  Use when building a fully sourced NotebookLM notebook with the nlm CLI —
  scoping a topic, assembling research/query/studio prompts from operators,
  gating those prompts before calling nlm, curating sources, and optionally
  generating Studio artifacts. Prefer this over ad-hoc nlm chat. Do not use
  for Obsidian vault writing (out of scope).
---

# auto-nlm

Drive NotebookLM by **building better prompts**, not by chatting harder.
The notebook is the deliverable. Studio artifacts are optional.

## How prompts get built (v1)

1. **Operators (B)** — small named pieces (e.g. named-entity anchor, recency, exclusions, format contract). An assembler turns a stack of operators into the string sent to `nlm`.
2. **Prompt gate (D)** — before any `nlm` call, score the draft prompt against a rubric. Fail → rewrite → only then run.
3. **State-conditioned (E)** — later. Once a dive stores what sources and answers we already got, later prompts can be built from that state. Not required for the first working slices.

Do not use fill-in-the-blank markdown as the engine. Templates may help *document* an operator; they are not the product.

## Never

- Skip a checkpoint that asks for user approval.
- Call `nlm` research/query/studio before CP0 (scope) is approved.
- Call `nlm` with a prompt that failed the prompt gate.
- Use vague topic-only research strings — named entities required.
- Load every operator/recipe into context at once — only what this step needs.
- Write Obsidian/vault notes as part of this skill (out of scope for v1).

## Phase map

| Phase | Name | Build prompts with | Gate |
|---|---|---|---|
| 0 | Scope | (inline) | **CP0** — user approves sub-topics |
| 0.5 | Pre-research | scout → fact pack for operators | (after CP0; before harvest) |
| 1 | Notebook | — | — |
| 2 | Harvest | research operators → assembler → **prompt gate** | **CP1** — sources strong enough |
| 2b | Vet | vetting prompt | clean sources |
| 3 | Interrogate | query operators → assembler → **prompt gate** | answer quality gate per pass |
| 4 | Artifacts (optional) | studio operators → assembler → **prompt gate** | user picks bundle |
| 5 | Done | — | checklist |

See `docs/OUTLINE.md` for the full living plan. Pre-research tools (Tavily / Firecrawl) are planned; do not add deps until the user approves.

Session state (notebook id, sub-topics, quota, later: remnants for E) lives outside this file.

## Phase 0 — Scope

Decompose the topic into 4–7 concrete sub-topics. Each must be specific enough to justify named-entity research.

Present the map. **Stop for CP0.** No `nlm` until the user approves or edits.

## Layout (filling in later slices)

- `operators/` — operator definitions (research / query / studio)
- `assembler/` — turns an operator stack into a prompt string (tested)
- `gates/` — prompt-gate rubrics (+ later answer gates)
- `prompts/` — optional examples / docs only; not the runtime engine

## Prerequisites

- `nlm` installed and authenticated
- This skill directory on disk (install path TBD)
