# Old flow (`nlm-deep-dive`)

What the private prototype skill did, and what “make it better” meant when we started `auto-nlm`.

Source skill: `cuddy-five/agents` → `skills/nlm-deep-dive`.

## What it did, in order

1. **Scope** — Split your topic into 4–7 sub-topics. You approve (**CP0**) before any `nlm` runs.
2. **Make a notebook** — Create a NotebookLM notebook, keep the real UUID (aliases are display-only).
3. **Harvest sources** — Run Deep Research style prompts that must use **named entities** (real papers, people, systems), not vague “tell me about X.”
4. **Ask 6 kinds of questions per sub-topic** — substance, tensions, failure modes, gaps, gotchas, plus later adversarial. Prompt text lived in files under `prompts/queries/`.
5. **Quality gate on answers** — If a reply was thin (no citations / no meat), re-ask up to a few times.
6. **Optional Studio stuff** — Infographic, slides, audio, flashcards. Biggest lever: the `--focus` prompt text.
7. **Write Obsidian notes** — Atomic notes + a map-of-content in your vault (after another approval).
8. **Done checklist** — Final sign-off.

### Extras it also had

- **Presets** (e.g. `ui-cognition` pre-fills a known topic shape)
- A pile of **nlm CLI gotchas**
- Small **Python helpers** (slugify, quality gate, wikilink check) with tests

## What “make it better” meant when we started auto-nlm

- Keep the good bones (checkpoints, named entities, multi-pass asking, Studio focus).
- Improve **how prompts get built** (operators + gate, later state; pre-research with Tavily/Firecrawl).
- Drop vault/Obsidian as the main product.

See also: [`OUTLINE.md`](OUTLINE.md) (living plan), [`../AGENTS.md`](../AGENTS.md) (fresh-context entry).
