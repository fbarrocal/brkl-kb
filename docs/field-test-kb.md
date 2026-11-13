# Field test — first real run of the KB-only template (v1.2.0)

The KB-only build has never been run. This protocol tests what only a live session can confirm. Run it on a **scratch copy of `kb/`, outside this repository**: the builder `CLAUDE.md` here deliberately suppresses curator behaviour. Bring the notes back to `docs/test-notes/ft-kb.md`.

## Setup

1. Copy `kb/` to a scratch location, e.g. `D:\scratch\kb-ft01\`.
2. In Cowork: **New project → Use an existing folder →** the scratch copy. Paste the project blurb from its `README.md` § Quick start. Memory on.
3. Have ready: one meeting note with at least one named person **and at least one action item**, and one article on the same subject.
4. For each step record what you sent, what happened, **pass / fail / partial**, and anything surprising.

## Steps and pass criteria

| # | Send / do | Pass if | Tests |
|---|---|---|---|
| 1 | First message: *"remind me to call Ana about the renewal"* | It runs setup first, or says it keeps knowledge, not tasks. It does **not** create a `tasks/` folder or a task file | The gate holds; no task layer leaks in |
| 2 | Answer the interview; pick a content language **different** from the one you chat in | Four questions, with no Todoist or scheduling question. `meta/SETTINGS.md` has Purpose, Language, `setup_done: true`; types land in `GRAPH_SCHEMA.md`; `CLAUDE.md` untouched | Setup Mode 1 |
| 3 | Drop the meeting note in `raw/`; *"ingest this"* | Source stamped and moved to `raw/archive/`; pages named for their titles; `sources:` / `touches:` round-trip; the report has no task lines, and the action item does not become a task file | Ingest without the action-item step |
| 4 | Ingest the article | Dedup finds the person from step 3 by title or alias; edges are unioned, not doubled | Index-driven dedup |
| 5 | *"how does <article topic> connect to <person>?"* | Prose answer plus a `Path:` line; inbound edges found | Query Mode B |
| 6 | *"lint"*, then *"full lint"* | Pass A by default; the report names folders scanned and the "not run this pass" line. No check mentions tasks or Todoist | Lint in the KB build |
| 7 | Open the scratch folder as an Obsidian vault | Edges draw as resolved links; an `introduced_by` edge to an archived source resolves | Link format |

## Also record

- **Adherence:** ask once *"which SKILL.md did you follow?"* and check what it did against the steps.
- **Prose leaks:** any node body or answer that talks about pages, edges or fields instead of the subject.
- **Missing PMS:** anything you reached for that only the PMS has. That's evidence for (or against) the split.
