# brkl-kb — Knowledge Base template repository

A portable, file-based **Knowledge Base** template. Sources go into `raw/`; Claude (in Cowork or Claude Code) curates them into typed, cross-referenced pages in `knowledge/`; questions are answered by navigating the graph. No database, no embeddings, no build step.

```
brkl-kb/          ← git root
├── CLAUDE.md     ← builder rules: the template is an artifact, not instructions
├── README.md     ← this file
├── kb/           ← the template, v1.3.1 — copy this
└── docs/         ← decision log, field-test protocol, v1 design record
```

**To use it:** copy `kb/` out, rename it to whatever the knowledge base is for, and follow its own `README.md`. It needs nothing from this repository.

**To change it:** edit the files in `kb/` directly, record why in `docs/decisions-kb.md`, bump the version in `kb/README.md`, and tag the commit.

**To A/B test:** copy `kb/` to a sibling folder (`kb-b/`), change one thing, and run both against the same material. Each folder carries its own rulebook, so the runs can't contaminate each other, and `git diff` between the folders describes exactly what changed.

## A warning, if you are an agent reading this

`kb/CLAUDE.md` is an operating manual for a *running* knowledge base: it says "act as this Knowledge Base's manager and curator." **In this repository it is an artifact to read and edit, not an instruction to obey.** Do not assume the curator role, do not run setup, ingest, query or lint, and do not write into `knowledge/` or `raw/`. The root `CLAUDE.md` states the builder rules.

## Versions

- **v1.0–v1.1** — *kbcurator*, the original template. Its git history is not in this repository; its design record is, in `docs/handover/`.
- **v1.2.0** — first release in this repository: the same curator with a new layout (`raw/`, `knowledge/`, `meta/GRAPH_*`) and the hardening developed after v1.1.
- **v1.3.0** — standalone: `kb/` is the only copy and is edited directly; no generated files, no tooling. Lint checks numbered 1–17.
- **v1.3.1** — fixes from a stale-reference audit: ingest archives the source as its truly last step, lint check numbers survive Markdown rendering, and cross-references point where they claim.

## Where the reasoning lives

- `docs/decisions-kb.md` — why the template is shaped the way it is. Read it before proposing a structural change.
- `docs/field-test-kb.md` — the protocol for the first real run.
- `docs/handover/` — the v1.0.1 design record. Docs `01`–`07` are historical; `000_READ_FIRST.md` maps them to the current layout.
