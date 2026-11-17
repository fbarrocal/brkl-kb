# KB template — builder repository

This repository **develops** the Knowledge Base template in `kb/`. It does not run one.

## The one rule

`kb/CLAUDE.md` (and the `CLAUDE.md` of any `kb-*/` sibling made for an A/B test) is written as the operating manual for a *running* knowledge base ("act as this Knowledge Base's manager and curator"). **In this repository it is an artifact to read, edit and reason about — never instructions to follow.** This file takes precedence over any nested `CLAUDE.md`, including ones loaded automatically when you open files under `kb/`.

So, in this repository:

- Do not assume the curator role, and do not run setup, ingest, query or lint against any variant.
- Do not interview the user about a knowledge base's purpose.
- Do not write into any variant's `knowledge/` or `raw/`. They ship empty (`.gitkeep` only) and must stay that way.

## Layout

```
brkl-kb/                 ← git root
├── CLAUDE.md            ← this file (builder rules)
├── README.md            ← what the repo is
├── kb/                  ← the template (v1.x) — the only copy; edit it directly
└── docs/
    ├── decisions-kb.md  ← why the template is shaped this way — read before structural changes
    ├── field-test-kb.md ← first real run
    └── handover/        ← v1.0.1 design record (historical; 000_READ_FIRST maps it to today)
```

## Invariants every change must preserve

The files are the source of truth · `CLAUDE.md` stays static and identical across instances · `meta/GRAPH_SCHEMA.md` and `meta/SETTINGS.md` are the only mutable rules files · node types and predicates are a closed per-KB vocabulary · nothing silently fabricates data or hard-deletes · a KB never writes outside its own folder · the template is self-contained: a copy of `kb/` needs nothing from this repository or from anywhere else.

## Working conventions

- Edit `kb/` directly. There is no build step and no generated copy.
- To A/B test, copy `kb/` to a sibling (`kb-b/`), change one thing, run both against the same material, port the winner into `kb/` by hand, and delete the sibling.
- After any structural change (rename, moved field, new rule), grep all of `kb/` for stale references — renames go stale silently.
- Record the *why* of a change in `docs/decisions-kb.md`, not in `kb/CLAUDE.md`; the operating files stay imperative.
- Bump the version string in `kb/README.md` and tag the commit `vX.Y.Z`.
