# KB decision log

Why the Knowledge Base template is shaped the way it is. Before proposing a structural change, check this file and `handover/03_decisions.md` (the v1.0.1 record). If a point was already argued and settled, don't reopen it without new field evidence.

## Load-bearing decisions (v1.0 → v1.2)

- **Graph over vectors, entirely on Claude.** No external database or embeddings: the graph *is* the markdown, and Claude is both curator and query engine. Accepted cost: not local (content goes to Anthropic), and a ceiling of about 150–200 pages / 50–100k tokens.
- **Typed edges as per-predicate frontmatter fields**, not a custom `edges:` block — the Obsidian Wikilink Types storage format, so Dataview, Breadcrumbs, Juggl and Graph Link Types read the graph with no plugin dependency.
- **A static `CLAUDE.md` and two mutable rules files.** `meta/GRAPH_SCHEMA.md` is the vocabulary (it grows and gets diffed); `meta/SETTINGS.md` is configuration (written once). The agent can't corrupt its own rulebook, and an upgrade is a file copy. In v1 a single root `SETTINGS.md` held both.
- **`curator_notes`** stops dedup and lint from merging pages a human kept apart on purpose. It is respected by tooling and never restated in prose.
- **Query answers are prose first, plus one `Path:` line** — the compromise between "no metadata narration" (user) and "a bare-prose answer can't be told apart from confabulation" (agent).
- **Visible `meta/`, not a hidden `.claude/`.** Field test 01 showed Cowork failed to discover skills in a dot-folder.
- **Folders are named for what they hold:** `raw/` (the inbox; processed sources move to `raw/archive/`) and `knowledge/` (flat; a page's type is a field, not a folder). `knowledge/` is not called `graph/`: the graph is the typed edges between pages, not a folder.
- **Every edge link resolves to a filename.** Obsidian and every plugin above resolve links by filename, not by title or alias, so pages are named for their title and an archived source is linked `[[<file-stem>|<title>]]`. Checked against the source of Obsidian's API, Dataview, Breadcrumbs 4.22.1, Juggl 1.5.0 and Graph Link Types.
- **Edge fields are sets** (union on every write, so re-ingesting a corrected source never doubles an edge). **Symmetric predicates are mirrored** on both pages; the `(symmetric)` marker in the schema's inverse column is the trigger, so a new symmetric predicate inherits the rule.
- **Provenance runs both ways:** `sources:` on a page, `touches:` on a source, and lint checks the round trip. An unsourced page is the shape a fabricated one takes.
- **Ingest stamps what's missing.** A file dropped in `raw/` usually carries no frontmatter, so ingest adds the bookkeeping fields (and a `.md` sidecar for a binary). That is the only edit ever made to a source.
- **Lint has three passes** — quick (default, the only one worth scheduling), full, fidelity — and reports by severity, not by check number, with a closing "not run this pass" line and a "what this cannot check" section. The fidelity check samples and must state its sample size.
- **Old trigger phrases stay** ("revise o KM", "add this to my knowledge base"). A rename is not a reason to stop recognizing what the user actually types.
- **Embedding the KB inside a host project (v1.1, decision 17) was reversed.** The KB is a top-level folder whose `CLAUDE.md` is the session's rulebook, and it never writes outside that folder.

## v1.2.0 (2026-11-13) — its own repository

The knowledge base had been developed inside a larger template's repository (a knowledge graph plus a task layer). v1.2.0 took it out as its own template, so projects that need only a knowledge base have one. It shipped with a source/build split — a `core/` with slots and a generated `kb/` — which v1.3.0 removed.

## v1.3.0 (2026-11-17) — standalone, no build

**Decision (Fernando):** this template is independent. Nothing is built from it, it is built from nothing, and it refers to no other project.

- **`core/`, its slots, `core.json` and `tools/` are gone. `kb/` is the only copy and is edited directly.** The slots existed only so another template could share this text. With that sharing ruled out they had no job, and 180 inline markers made the source hard to read. A generated copy also put a build and a check in front of every change, for no gain when every change is deliberate and made by the builder.
- **Accepted cost:** any template that shares this lineage now evolves separately. A fix made here reaches nothing else, and nothing made elsewhere arrives here, unless someone ports it by hand.
- **Lint checks renumbered 1–17.** The gaps (14, 19, 20) belonged to checks this template never had.
- **The v1.0.1 design record was copied into `handover/`,** so this repository needs nothing outside itself.

## v1.3.1 (2026-11-17) — stale-reference audit

A read-only audit of `kb/` found references that had drifted from the files they describe. Fixed together:

- **Ingest: the index update now runs before archiving (steps 9 and 10 swapped).** The "if you can't finish" section rested on archiving being the last act, but the index update ran after it. An interruption between the two left a source archived, so it looked done, while its pages were missing from the index. Now a source that still looks pending is the only sign of an unfinished ingest. That is the state the recovery rule already assumes.
- **Lint check numbers are written into the text, not as Markdown list markers.** Out-of-sequence markers (1, 2, 3, 8, …) are renumbered by any CommonMark renderer, so in Obsidian or on GitHub #8 showed as "4" and #15 as "10", and stopped matching the pass table and severity bands. The agent reads raw text and wasn't affected; humans were.
- **Lint #16 cites the right source.** It credited `CLAUDE.md` with naming the "Page X … through edge W" failure mode. That example lives in the query skill; `CLAUDE.md` states the rule (§ Role).
- **Lint scope and example report match the rules.** Scope now names `raw/archive/` for #17. The Pass A example now lists `raw/` as scanned, since #8 and #12 scan it and the report must say what was scanned.
- **`GRAPH_SCHEMA.md` default types list the seed's bookkeeping fields** (`created`, `updated`, `curator_notes`). Lint #2 compares fields against § Nodes, so the shorter list invited false drift reports.
- **README wording:** `.gitattributes` added to the layout; the `.gitignore` description matches the file; "`CLAUDE.md` explains the design in full" became "how it works", because the *why* lives here and does not ship with a copy of `kb/`.

## Known gaps

- **This template has never been field-run.** `field-test-kb.md` is the protocol.
- **The v1.1 → v1.2+ upgrade steps in `kb/README.md` are untested** against a real v1.1 knowledge base.
- **Nothing verifies that `CLAUDE.md` or `meta/skills/` were not edited** in an instance — the invariant the design rests on. Git is the current answer.
- **Procedure adherence is unproven:** discovery of the skills is solved (visible `meta/`); whether the agent follows them step by step, rather than improvising, is not.
