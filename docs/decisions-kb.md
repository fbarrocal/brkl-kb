# KB decision log

Why the KB template is shaped the way it is. Before proposing a structural change, check this file. Then check the history it points to: **brkl-pms `docs/decisions-v2.md`**, which covers everything decided while this code lived inside the PMS, and **brkl-pms `docs/handover/`**, the v1.0.1 design record. If a point was already argued and settled there, don't reopen it without new field evidence.

History: <https://github.com/fbarrocal/brkl-pms/tree/master/docs>

## Still load-bearing from v1 and v2

- **Graph over vectors, entirely on Claude.** The graph *is* the markdown; Claude is both curator and query engine. Accepted cost: not local, a ceiling of about 150–200 pages.
- **Typed edges as per-predicate frontmatter fields**, so Dataview, Breadcrumbs, Juggl and Graph Link Types read the graph with no plugin dependency.
- **Static `CLAUDE.md`, two mutable rules files** (`meta/GRAPH_SCHEMA.md` for vocabulary, `meta/SETTINGS.md` for configuration). The agent can't corrupt its own rulebook, and an upgrade is a file copy.
- **`curator_notes`** stops dedup and lint from merging pages a human kept apart on purpose.
- **Prose-first answers plus one `Path:` line**, so a traversal can be told apart from a confabulation.
- **Visible `meta/`**, because Cowork didn't discover skills in a dot-folder (field test 01).
- **Every edge link resolves to a filename** (brkl-pms v2.1.0). Pages are named for their title; archived sources are linked `[[stem|title]]`.
- **Lint in three passes** (quick, full, fidelity), reported by severity, with a "what this cannot check" section.

## v1.2.0 (2026-11-13) — split out of brkl-pms

**Decision (Fernando):** the knowledge base becomes its own template and repository again. brkl-pms depends on it instead of containing it. The driver is real: some projects need only a knowledge base, and since brkl-pms v2.4.0 the only template asked for a Todoist connection, a task layer and a sync schedule. There was nowhere left to start a plain KB.

**How the dependency works: source split, build-time composition.**

- `core/` is the KB with **slots**: marked places an extension may fill. Built alone, it gives `kb/`. brkl-pms vendors the core at a tag, fills the slots with its task layer, and builds `pms/`.
- *Why at build time rather than at runtime:* a runtime plugin (the core's skills checking "is an extension installed?") would add one more instruction the agent must remember on every operation. Procedure adherence is the least proven thing in this design. Worse, the most dangerous rule in the PMS, that the traversal set is `knowledge/` ∪ `tasks/`, would sit behind that indirection, and getting it wrong fails silently. Built templates stay as explicit as before: one `CLAUDE.md`, every rule inline.
- *Why not two vaults (the PMS calling the KB):* tasks would leave the graph, which ADR-001 already rejected; Obsidian links don't cross vaults; two rulebooks in one session repeats decision 17's open risk (a nested rulebook leaking into a session that only meant to read).
- *Why not one template with a "tasks off" flag:* it lets nothing evolve apart. Every KB edit would still happen in files full of Todoist text.
- *Why two repositories (Fernando's call, over a monorepo recommendation):* the KB gets its own identity, issues and releases. The cost is accepted: a change that touches a slot needs a KB release, then a vendor bump in brkl-pms.

**The split itself was a pure refactor.** Every PMS-only passage of brkl-pms v2.6.1 was cut verbatim into a fill and replaced by a slot whose default is the KB wording (or nothing). The acceptance test: brkl-pms's build from the vendored v1.2.0 core is byte-identical to v2.6.1's `pms/`, except the version line. That passed. It is why there are 180 slots. Many are a few words long, because the PMS text was woven through the shared paragraphs and this version doesn't restructure it.

**Placeholders, not search-and-replace.** The core says `{{SYS}}` / `{{SYSTEM}}` wherever the text names the system, so the same sentence reads "this KB" or "this PMS". Where a sentence needs different words, not just a different name, it's a slot.

**The leak check is the mechanical stale-reference sweep.** `tools/build.py` fails if `kb/` contains task or Todoist vocabulary, the word PMS, or a `conventions` section. It caught three leaks during the split (a task in query's "don't fabricate" line, task fields in lint's prerequisites, "captures" in lint's pass table) and two of its own false positives.

**What the KB build gives up, deliberately:**

- **Scheduled tasks.** The KB never writes outside its folder, as in v1. A weekly lint is a reminder the user sets up, not something setup creates.
- **The `Conventions` section of `meta/SETTINGS.md`.** Both of its settings (task id prefix, archive window) were task settings.
- **Sequential lint numbers.** The KB has checks 1–13 and 15–18; #14, #19 and #20 belong to the PMS. Renumbering would break the byte-identical PMS build. The pass tables never listed checks in order anyway.

## Known gaps

- **Nothing in v1.2.0 has been field-run as a KB-only template.** `field-test-kb.md` is the protocol.
- **The v1.1 → v1.2 upgrade steps in `kb/README.md` are untested** against a real v1.1 knowledge base.
- **A fill can go wrong silently when the core text around its slot changes.** The build checks names, not meaning. An extension must read its own build diff after every core bump (see `SLOTS.md`).
- **180 slots is a lot of fine-grained seams.** The next structural pass should move extension additions into their own sentences and paragraphs so fewer, larger slots do the same job. That changes the PMS build's text, so it needs its own review on the brkl-pms side.
