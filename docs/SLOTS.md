# Slots — the contract between the KB core and its extensions

The KB core (`core/`) is written once and built two ways: alone, as the KB-only template (`kb/`), and with an extension, as a larger template (brkl-pms builds `pms/`). The extension never edits core text. It fills **slots** the core left for it, and adds whole files the core doesn't have. `tools/assemble.py` does the combining and fails rather than guessing.

The point of doing it at build time: each built template is one plain, explicit set of files. The agent running it reads a single `CLAUDE.md` and never has to work out what's installed. All the composition happens here, where a script can check it.

## Syntax

**In a core file**, a slot is one of:

```
<!-- slot:NAME -->
default lines
<!-- /slot:NAME -->                      block: both markers alone on their lines

text<!-- slot:NAME -->default<!-- /slot:NAME -->text      inline: exact characters

<!-- slot:NAME /-->                      empty slot (block if alone on its line, else inline)
```

The default is what the KB-only build gets. It is either real KB text or nothing.

**In an extension,** `ext/fills/<same path as the core file>` holds the fills, with the same two forms and `fill` instead of `slot`:

```
<!-- fill:NAME -->
replacement lines
<!-- /fill:NAME -->
```

A filled slot is replaced by its fill; an unfilled one keeps its default. Text outside fill blocks is ignored, so fill files can carry headings and notes. A block slot takes a block fill; an inline slot takes an inline fill.

**Placeholders.** `{{SYS}}`, `{{SYSTEM}}` and `{{sys}}` name the system (KB / Knowledge Base / kb in `core.json`; an extension overrides them in its `extension.json`). `{{VERSION}}` is the version of what is being built: the core's for `kb/`, the extension's for an extended build. `{{CORE_VERSION}}` is always the core's.

**Extension files.** `ext/files/` adds whole files (skills, templates, folders) to the build. It may not overwrite a core file. To change core text, fill a slot.

## What the build refuses

- A fill for a slot the core file doesn't have: the slot was renamed or removed in the core.
- A fill file for a core file that doesn't exist.
- An extension file at a path the core already uses.
- An unknown placeholder, a nested or unclosed marker, a block/inline mismatch.
- An extension whose `requires_core` (`^X.Y.Z`: same major, at least that version) doesn't match `core.json`.
- Any marker or placeholder left in the output.
- `--clean` on a folder whose `knowledge/`, `raw/` or `tasks/` hold anything but `.gitkeep`. That would be a real instance, not a template.

## Versioning

The slot set is the core's public API. In `core.json`'s version:

- **patch** — core text changed, no slot changed.
- **minor** — a slot was added (its default keeps every existing extension's build identical).
- **major** — a slot was renamed or removed, or the text around a slot changed enough that a fill written for the old text now reads wrong.

The last case is the one no script can catch: a fill is text, and text can stop making sense next to new neighbours. So an extension that bumps its vendored core **reads the diff of its own build**, not just the build's exit code. brkl-pms's build prints any core slot it leaves unfilled, which is how a new minor-version slot gets noticed.

## The current list

`python tools/build.py --list-slots`. At v1.2.0 there are 180 slots across 10 files. Every one of them was cut from brkl-pms v2.6.1, so brkl-pms fills all of them, and its build reproduced v2.6.1 byte for byte apart from the version line.

Slot names are scoped to their file and say what they hold: `router.capture`, `tasks-todoist`, `c14`. A number slot (`n9`, `layers.n4`) exists because removing an extension step renumbers the steps after it in the KB build.
