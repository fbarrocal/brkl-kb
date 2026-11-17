# Graph Schema

The **closed vocabulary** of this Knowledge Base: which kinds of thing may exist, and which kinds of relationship may connect them. Every operation reads this file. Nothing outside these lists may be written — propose an addition here (via the setup skill) rather than inventing one.

This is one of exactly two mutable **rules** files (the agent also maintains the index, templates, and content, but none of those define rules). The other is `meta/SETTINGS.md`, which holds this KB's *state and configuration* (purpose, language, setup flag). The split is deliberate: this file is a vocabulary that grows and is diffed when it changes; the other is configuration, written once and rarely again. The static rules live in `CLAUDE.md` and are never edited.

## Nodes

Node types active in this KB. Every page in `knowledge/` declares one in its `type:` field. Ships with two generic defaults: `entity` and `concept`. First-run setup adds KB-specific ones. For each type: its name, what it represents, its fields, and its template file (if any).

- **`entity`** — a specific thing (a person, place, work, object, event). Fields: `title`, `aliases`, `tags`, typed-edge fields, `sources`, `see_also`, `created`, `updated`, optional `curator_notes`. Template: none (follows the generic seed `meta/templates/_node.template.md` shape).
- **`concept`** — an idea, theme, or topic. Fields: same as `entity`. Template: none (same seed shape).

<!-- Add KB-specific node types here during setup, e.g.:
- `person` — a human relevant to this KB. Fields: title, aliases, role, org, edges, sources, see_also. Template: meta/templates/person.md
- `meeting` — a meeting worth remembering. Fields: title, date, edges, sources, see_also. Template: meta/templates/meeting.md
Each new type: (1) copy meta/templates/_node.template.md to meta/templates/<type>.md, (2) register it here. -->

## Edges

The predicates (typed-edge field names) active in this KB. Setup may add KB-specific ones. Edges are directional; only the forward field is stored on the origin page (inverses are for reading). A predicate marked **(symmetric)** in the inverse column is the exception — it is written on both pages. That marker is load-bearing: it is what makes the mirroring rule in `CLAUDE.md` fire, so mark any new symmetric predicate accordingly. Do not invent predicates outside this list — propose additions here instead.

| predicate (field) | meaning (A → B) | inverse |
|---|---|---|
| `is_a` | A is an instance/subtype of B | `has_instance` |
| `part_of` | A is a component of B | `has_part` |
| `based_on` | A is built on / derived from B | `basis_for` |
| `authored_by` | A was created by person/org B | `authored` |
| `uses` | A makes use of B | `used_by` |
| `depends_on` | A requires B to function | `depended_on_by` |
| `introduced_by` | A was surfaced to me via source or person B | `introduced` |
| `about` | A (a page/note) is about topic B | `subject_of` |
| `supports` | A is evidence/argument for B | `supported_by` |
| `contradicts` | A conflicts with B | `contradicts` (symmetric) |
| `contrasts_with` | A is compared against B | `contrasts_with` (symmetric) |
| `precedes` | A comes before B in time/sequence | `follows` |
| `member_of` | A belongs to group/collection B | `has_member` |
| `located_in` | A is situated in B | `contains` |

<!-- Add KB-specific predicates here during setup, e.g. for a book: `ally_of`, `enemy_of`;
for a work KB: `owns`, `assigned_to`. Keep the list closed and small. -->
