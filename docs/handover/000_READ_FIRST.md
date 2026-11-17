# READ FIRST — the v1.0.1 design record

> **These docs describe the template at v1.0.1 (July 2026),** when it was called *kbcurator* and lived in a `template/` folder. Docs `01`–`07` are a historical record of *why* the design is shaped as it is. They are not a description of the current files, which are in `kb/` at the repository root.
>
> | v1.0.1 | now |
> |---|---|
> | `template/` | `kb/` |
> | `sources/` | `raw/` (processed sources move to `raw/archive/`) |
> | `content/` | `knowledge/` |
> | `meta/index.md` | `meta/GRAPH_INDEX.md` |
> | root `SETTINGS.md` (purpose + schema) | `meta/SETTINGS.md` (purpose, language) **+** `meta/GRAPH_SCHEMA.md` (vocabulary) |
> | `meta/host-snippet.md`, embedded installs (decision 17) | **removed** — the KB is a top-level folder and never writes outside it |
>
> **Two invariants from `04` were amended on purpose:** "SETTINGS.md is the only mutable rules file" became "exactly two mutable rules files under `meta/`", and decision 17 (embedding in a host project) was reversed. Everything else in `04` still holds. The reasoning after v1.0.1 is in `../decisions-kb.md` — read it before proposing a structural change.

## How to absorb it

1. Read `01` → `07` in order, as history, with the table above in mind.
2. Then read the live template: `kb/README.md`, `kb/CLAUDE.md`, `kb/meta/GRAPH_SCHEMA.md`, and the four skills under `kb/meta/skills/` (setup, ingest, query, lint).
3. Then `../decisions-kb.md` for what changed after v1.0.1 and why.

## One warning — do not run the template

`kb/CLAUDE.md` is an operating manual for a *running* knowledge base: it says "act as this Knowledge Base's manager and curator." In this repository it is an artifact to read and reason about, not instructions to obey. Do not assume the curator role, do not run setup, ingest, query or lint, and do not interview anyone about a knowledge base's purpose. The repository-root `CLAUDE.md` states this rule for builder sessions.

## Conventions

- Confidence tags appear throughout: **[Certain]** (verified or definitional), **[Likely]** (well-grounded inference), **[Guessing]** (explicit uncertainty).
- The v1 design was validated across four real-world "test note" rounds; the docs separate what is *proven* from what is *instruction-strength but unproven*.
