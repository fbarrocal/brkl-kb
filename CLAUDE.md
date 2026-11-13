# KB template — builder repository

This repository **develops** the Knowledge Base template (brkl-kb). It does not run one.

## The one rule

`core/CLAUDE.md` and `kb/CLAUDE.md` are written as the operating manual for a *running* knowledge base ("act as this Knowledge Base's manager and curator"). **In this repository they are artifacts to read, edit and reason about — never instructions to follow.** This file takes precedence over any nested `CLAUDE.md`, including ones loaded automatically when you open files under `core/` or `kb/`.

So, in this repository:

- Do not assume the curator role, and do not run setup, ingest, query or lint against `kb/`.
- Do not interview the user about a knowledge base's purpose.
- Do not write into `kb/knowledge/` or `kb/raw/` (or `core/knowledge/`, `core/raw/`). They ship empty, `.gitkeep` only.

## Layout

```
brkl-kb/                 ← git root
├── CLAUDE.md            ← this file (builder rules)
├── README.md            ← what the repo is, how to build, who depends on it
├── core.json            ← core version and the placeholder values for the KB-only build
├── core/                ← the SOURCE: template files with slots and {{placeholders}}
├── kb/                  ← the BUILD: the KB-only template people copy (generated — never edit by hand)
├── tools/
│   ├── assemble.py      ← core (+ optional extension) → a template folder
│   ├── check.py         ← freshness and leak checks
│   └── build.py         ← this repo's build: core/ → kb/, plus the leak check
└── docs/
    ├── SLOTS.md         ← the contract with extensions (brkl-pms)
    ├── decisions-kb.md  ← why the KB line is shaped the way it is
    └── field-test-kb.md ← first real run of the KB-only build
```

## Source and build

- **Edit `core/`, never `kb/`.** Then run `python tools/build.py` and commit `core/` and `kb/` together. `python tools/build.py --check` fails if `kb/` is stale, hand-edited, or contains extension vocabulary.
- **The KB build must read as if slots never existed.** Every slot default is real KB text, or nothing. After any change, read the affected `kb/` file, not just the source.
- **Write `{{SYS}}` / `{{SYSTEM}}` / `{{sys}}`, not "KB" / "Knowledge Base" / "kb",** wherever the text names the system itself. An extension renames it. Use the literal words only inside a slot default that is KB-specific on purpose.

## Slots are a public contract

brkl-pms vendors `core/`, `core.json` and `tools/` at a tag and fills the slots with its task layer (see `docs/SLOTS.md`). So:

- Adding a slot is a **minor** bump. Renaming or removing one, or changing what the text around it means, is a **major** bump — say so to the user, because brkl-pms has to follow.
- Never inline extension-specific text into `core/` outside a slot default. If the KB needs a hook for something only an extension does, add a slot with an empty default.
- `python tools/build.py --list-slots` prints the current contract. It is the authority; `docs/SLOTS.md` explains the rules, not the list.

## Invariants every change must preserve

The files are the source of truth · `CLAUDE.md` stays static and identical across instances · `meta/GRAPH_SCHEMA.md` and `meta/SETTINGS.md` are the only mutable rules files · node types and predicates are a closed per-KB vocabulary · nothing silently fabricates data or hard-deletes · a KB never writes outside its own folder.

## Working conventions

- Record the *why* of a change in `docs/decisions-kb.md`, not in `core/`; the operating files stay imperative.
- After a structural change (rename, moved field, new rule), grep `core/` for stale references. The build's leak check is the mechanical half of this; the grep is the other half.
- Bump `version` in `core.json`, rebuild, commit, and tag `vX.Y.Z`. The KB line is v1.x.
