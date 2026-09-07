# Domain Docs

How engineering skills consume this repo's domain documentation.

## Before exploring, read these

- `CONTEXT.md` at the repo root, or
- `CONTEXT-MAP.md` at the repo root if it exists: it points at one `CONTEXT.md` per context. Read each relevant one.
- `docs/adr/`: read ADRs that touch the area being changed.

If these files do not exist, proceed silently. The domain-modeling workflows create them only when terms or decisions are resolved.

## File structure

Single-context repo:

/
├── CONTEXT.md
├── docs/adr/
└── src/

## Use the glossary's vocabulary

Use terms as defined in `CONTEXT.md`; avoid synonyms that the glossary explicitly avoids. If a needed term is missing, reconsider whether it is project vocabulary or record the gap for `/domain-modeling`.

## Flag ADR conflicts

Explicitly surface any contradiction with an ADR rather than silently overriding it.
