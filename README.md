# show

A [Claude Code](https://claude.com/claude-code) reference skill: which visual
a piece of content calls for, how that visual renders or falls back on each
target, and the page rules that make the result scannable.

No workflow and no slash command (`user-invocable: false`). Other skills, and
a reflex in `CLAUDE.md`, load it by name before choosing a visual.

## What it holds

| File | Content |
| --- | --- |
| `skills/show/SKILL.md` | page rules, content → visual table, render and budget, render ladder, routing to other skills |
| `skills/show/references/render-targets.md` | capability matrix per target, with the source of every ✅, and the declared fallbacks |
| `skills/show/references/chart-chooser.md` | whether a table earns a chart, which one, and the mermaid chart traps |
| `skills/show/references/page-patterns.md` | one pattern per genre (decision page, ticket, MR, release note, terminal answer, Slack, slide) |
| `skills/show/references/worked-example.md` | one decision page taken apart rule by rule |

## Related skills

- [`shape`](https://github.com/bastien-gallay/shape): improves a document for
  its reader and proves nothing was lost; `show`'s proof gates live there.
- [`glance`](https://github.com/bastien-gallay/glance): terminal answers
  sized to be read at a glance.
- [`flow-lean`](https://github.com/FlorianBruniaux/flow-lean), a third-party
  plugin, sets terminal density; `dataviz`, `artifact-design` and
  `impeccable`, also routed to from `SKILL.md` §4, come with the host or their
  own plugins. `show` works without any of them.

## Install

```sh
ln -s "$PWD/skills/show" ~/.claude/skills/show
```

## Suggested `CLAUDE.md` reflex

```markdown
- Show rather than tell, for anything a reader will read: pick the visual from the content. Data touched → table; structure that changes → class or ER diagram; multi-component process → sequence diagram; branching algorithm → flowchart; old vs new → red/green, always with a −/+ sign.
- Several decisions → overview matrix first, handles D1…Dn; a one-line verdict per block; the source under each visual; unverified flagged where it sits; a status marker (✅ ⚠️ ❓) opens its line or cell.
- Rendering depends on the target (fenced ASCII, mermaid, SVG, interactive): load the `show` skill before writing outside the terminal, for the render matrix and the declared fallbacks.
```

## Status

0.3.3, 2026-10-06. Every ✅ in the render matrix points to a numbered source
under it: a dated probe or capture, daily use, an indirect or private
observation, or `shape`'s own matrix; a ✓? or ❓ is a probe still to run.
The worked example retells a real decision page on a fictional product
catalogue.
