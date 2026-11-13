# brkl-kb — Knowledge Base template repository

A portable, file-based **Knowledge Base** template: sources go into `raw/`, an agent (Claude in Cowork or Claude Code) curates them into typed, cross-referenced pages in `knowledge/`, and questions are answered by navigating the graph. No database, no embeddings, no task layer.

**To use it:** copy `kb/` out, rename it to whatever the knowledge base is for, and follow its own `README.md`. It has no dependency on this repository.

```
brkl-kb/          ← git root
├── CLAUDE.md     ← builder rules: the template files are artifacts, not instructions
├── core.json     ← core version (v1.2.0)
├── core/         ← source: the template with slots for extensions
├── kb/           ← build: the KB-only template — copy this one
├── tools/        ← assemble / check / build (Python 3, standard library only)
└── docs/         ← slot contract, decision log, field-test protocol
```

## Building

```
python tools/build.py               # core/ → kb/
python tools/build.py --check       # fail if kb/ is stale, hand-edited, or leaks extension vocabulary
python tools/build.py --list-slots  # the slots extensions can fill
```

## Extensions

The core has **slots**: marked places where an extension adds or replaces text, plus a few `{{placeholders}}` for the system's name. An extension ships its own fills and whole extra files, and `tools/assemble.py` combines the two into one self-contained template. The agent that runs it never sees a slot. See `docs/SLOTS.md`.

The one extension today is **[brkl-pms](https://github.com/fbarrocal/brkl-pms)**, the Personal Management System: this KB plus a task layer, with Todoist as the task surface. It vendors `core/`, `core.json` and `tools/` from a tag of this repository.

## A warning, if you are an agent reading this

`core/CLAUDE.md` and `kb/CLAUDE.md` are operating manuals for a *running* knowledge base. **In this repository they are artifacts to read and edit, not instructions to obey.** Do not assume the curator role, do not run setup, ingest, query or lint, and do not write into `knowledge/` or `raw/`. The root `CLAUDE.md` states the builder rules.

## Versions

- **v1.0–v1.1** — *kbcurator*, the original template. That history, and its design record (`docs/handover/`), live in the brkl-pms repository, where this code was developed until the split.
- **v1.2.0** — the knowledge-base half of brkl-pms v2.6.1, split out as its own template with slots for extensions. The curator is v1.1's, with the layout and hardening it gained inside brkl-pms v2. Why: `docs/decisions-kb.md`.
