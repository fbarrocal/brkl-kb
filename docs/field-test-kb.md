# Field test — first real run of the template (v1.3)

The template has never been run. This protocol tests what only a live session can confirm. Run it on a **scratch copy of `kb/`, outside this repository**: the builder `CLAUDE.md` here deliberately suppresses curator behaviour. Bring the notes back to `docs/test-notes/ft-kb.md`.

## Setup

1. Copy `kb/` to a scratch location, e.g. `D:\scratch\kb-ft01\`.
2. In Cowork: **New project → Use an existing folder →** the scratch copy. Paste the project blurb from its `README.md` § Quick start. Memory on. (Or open the folder in Claude Code.)
3. Have ready: one meeting note with at least one named person and one commitment ("Ana will send the contract"), and one article on the same subject.
4. For each step record what you sent, what happened, **pass / fail / partial**, and anything surprising.

## Steps and pass criteria

| # | Send / do | Pass if | Tests |
|---|---|---|---|
| 1 | First message: *"what do I know about the renewal?"* | It runs setup first instead of answering | The first-run gate |
| 2 | Answer the interview; pick a content language **different** from the one you chat in | Four questions: purpose, node types, edges, language. `meta/SETTINGS.md` has Purpose, Language, `setup_done: true`; types land in `meta/GRAPH_SCHEMA.md`; `CLAUDE.md` untouched | Setup Mode 1 |
| 3 | Drop the meeting note in `raw/`; *"ingest this"* | Source stamped and moved to `raw/archive/`; pages named for their titles; `sources:` / `touches:` round-trip; no folder outside `raw/`, `knowledge/` and `meta/` appears. Note what happened to the commitment | Ingest |
| 4 | Ingest the article | Dedup finds the person from step 3 by title or alias; edges are unioned, not doubled | Index-driven dedup |
| 5 | *"how does <article topic> connect to <person>?"* | Prose answer plus one `Path:` line; inbound edges found | Query Mode B |
| 6 | *"lint"*, then *"full lint"* | Pass A by default; the report names the folders scanned and ends with the "not run this pass" line; check numbers run 1–17 | Lint |
| 7 | Open the scratch folder as an Obsidian vault | Edges draw as resolved links; an `introduced_by` edge to an archived source resolves | Link format |

## Also record

- **Adherence:** ask once *"which SKILL.md did you follow?"* and compare what it did with the steps.
- **Prose leaks:** any page body or answer that talks about pages, edges or fields instead of the subject.
- **Missing features:** anything you reached for that the template doesn't do. Note it instead of working around it.
