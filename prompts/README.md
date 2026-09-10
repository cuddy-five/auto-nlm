# prompts/ (examples only)

Runtime prompt building lives in:

- `operators/` — named Lego pieces
- `assembler/` — stacks pieces into a string
- `gates/` — checks the string before `nlm` runs

Files here are **examples and docs**, not the engine. Prefer adding an operator over adding a new filled template.

## Roadmap

- **Now (B+D):** operators + assembler + prompt gate
- **Later (E):** assembler also reads session state (gaps, entities seen, failed sources) when building the next prompt
