---
name: show
description: >
  Visual grammar for anything written for a reader: which visual a piece of
  content calls for (data table, class or ER diagram, sequence diagram,
  flowchart, red/green diff, chart), how it renders or falls back on each
  target (terminal, GitLab, GitHub, Typst, Artifact, Confluence, Jira, Slack),
  and the page rules that make it scannable (overview matrix, stable handles,
  one-line verdict, source under each visual, unverified flagged in place).
  Reference skill, no workflow. Load before choosing a visual for a doc,
  ticket, MR, slide, page or Slack message, and before turning prose into a
  figure. Also: « montrer plutôt que dire », « mets-le en diagramme », « plus
  d'images que de mots », « rouge/vert pour l'ancien et le nouveau ».
user-invocable: false
---

# show

> **The one idea.** Pick the visual from what the content says, never from
> where it goes. The target decides two things only: how the visual renders,
> and how much room there is.

## 0. Page rules, every target

1. **Overview first.** Three or more decisions, findings or options → a
   matrix at the top, one row each, stable handles `D1…Dn` (`F`, `O`, `R`,
   `A` for findings, options, risks, actions). Current state in red,
   proposed state in green.
2. **One-line verdict per block**, an assertion led by a bold verb:
   **Normalise** …, **Keep** …. The same rule as a slide's assertion
   subtitle.
3. **Source under every visual**, small: file and line, commit, spec section,
   date read.
4. **Unverified is flagged where it sits**: ⚠️ plus what would verify it.
   Never collected in a footer.
5. **Colour never alone.** Every red/green also carries a sign: `−` / `+`,
   ✗ / ✓, or a word. The target may drop the colour (Markdown on Jira,
   copy-paste, a colour-blind reader); the sign survives.
6. **One fact, one place.** No paragraph walks a figure's edges. The prose
   next to a visual says what the visual cannot: the why, the exception, the
   trap.
7. **Status marker first.** A status marker (✅ ❌ ⚠️ ⛔ ❓) opens its line,
   bullet or table cell: `✅ mermaid`, not `mermaid ✅`. The eye scans the
   left edge; a marker at the end is read last, or not at all.
8. **Prose budget.** Between two visuals, prose carries the verdict and what
   the visual cannot say. The rest takes a form: definitions → a glossary
   table; a reading → one claim per bullet; a computation → a `text` block,
   one step per line; a hypothesis → `⚠️ H: … → verify: …` where it sits; a
   status every row shares → said once, in the caption; a repeated
   conversion (time zone, unit) → one rule at the top; a cell over about 50
   words → a short cell and a note under the table. ⚠️ About 60 words between
   two visuals is the working threshold, not yet calibrated.
9. **Every handle defined before its first use.** A letter or an acronym
   (`G`, `M1`, `SKU`) is defined where it first appears and gathered in a
   glossary under the verdict. A letter handle is mnemonic. A table more than
   a screen away from the glossary repeats its legend in one line.

## 1. Content → visual (invariant)

| The content says | Visual | Fallback without a renderer |
| --- | --- | --- |
| which data is touched | table: field · before · after · rows affected | table; on `plain`, one labelled line per row |
| a data or object structure changes | class or ER diagram, before and after side by side | table field × v1 / v2, rows signed `−` / `+` |
| a process across several components changes | sequence diagram, before and after | numbered arrows `A → B : message` in a mono block |
| an algorithm that branches | flowchart, changed nodes outlined red/green and signed | indented tree in a mono block |
| a linear procedure, no branch | ⛔ not a flowchart: a numbered list | same |
| old vs new: config, schema, rule, wording | red/green diff with signs | `diff` block; else the sign alone |
| numbers whose comparison carries the verdict: shares, a trend, a distribution, ranges | chart chosen in `references/chart-chooser.md`; form and colours through `dataviz` where installed | table, and say what it loses |
| options sharing criteria | option table, recommendation ⭐ | same |
| durations and proportions on a clock | gantt | proportional ASCII bar in a mono block |
| intervals that overlap, such as concurrency | gantt, one section per lane | one ASCII row per lane |
| a chronology, order only | timeline or ordered list | ordered list |

**Combine.** One block often needs two visuals: an algorithm plus the data it
touches, a structure change plus the field diff it causes. The unit is one
visual per question the reader asks, not one per block.

## 2. Same table, two variables: render and budget

**Render**: `references/render-targets.md` holds the capability matrix and
the declared fallbacks. Choose the fallback before writing, never at write
time.

**Budget**: how much room the target gives:

| Target | Budget | What survives first |
| --- | --- | --- |
| Terminal answer | one screen | verdict, overview matrix; detail on request |
| MR description | 4–6 lines | subtitle, one diff block or one table |
| Jira ticket | the sections of the ticket format | the Decision section as a current → options → reco table |
| Slack message | one message, no scroll | verdict, three lines, one image or a link |
| `.md` doc, Confluence page | a page | everything: matrix, one block per item |
| Slide | one full-frame visual | assertion subtitle, one visual |
| Artifact, HTML report | a page, interactive allowed | everything, interactive charts included |

Under budget, cut detail. Never cut the verdict, the source or the ⚠️.

A genre's format comes first: the ticket, MR and release-note conventions
define the slots, and this grammar fills them. Patterns per genre:
`references/page-patterns.md`.

## 3. Render ladder

| Rung | Use when | Targets | Watch |
| --- | --- | --- | --- |
| ASCII in a mono block | the target has no renderer | terminal, Jira code block, Slack code block | ≤ 80 columns; always fenced; a proportional bar stays ASCII while every segment is ≥ 5 columns and holds its label |
| mermaid | relation, sequence, structure that stays legible at its size | GitLab, GitHub, Artifact; elsewhere pre-rendered | compile it before shipping: a `;` in a sequence message breaks it and no lint sees it |
| SVG | mermaid lays it out badly, or the audience is wide and mixed (business, PMO, external) | Artifact, Typst; Jira, Slack as an image; Confluence as an attached image, never an `mmdc` SVG: it shows *Preview unavailable* | keep text as text so facts stay searchable; from mermaid, render with `htmlLabels: false` or Typst shows empty boxes; caption in the surrounding text where the target drops `alt` |
| interactive (uPlot, ECharts) | a data report the reader explores: zoom, hover, series toggle | HTML and Artifact only | design through `dataviz`; ship a static SVG or PNG for every other target |

Climb a rung when the lower one loses information the reader needs, not for
looks. Keep each visual as its source (mermaid text, SVG file, data rows) so
a fallback is generated, not redrawn.

### Two conflicts this skill settles

```diff
# C1 · a diagram on a target with no mermaid
- shape render-targets: "Never ASCII art"
- flow-lean, glance:    "Flow → ASCII diagram"
+ ASCII is allowed inside a monospace block (terminal, Jira code block, Slack
+ code block), never in proportional text. A target with a renderer gets
+ mermaid, SVG or an image instead.
# C2 · ASCII and fences
- glance: "never wrap … diagrams … in a fence"
+ An ASCII diagram is always fenced. Outside a fence, claude.ai and Slack set
+ it in a proportional font and the alignment breaks.
```

Both settled in `shape` and `glance` on 2026-10-06.

## 4. Routing

| Need | Load |
| --- | --- |
| which chart a table earns | `references/chart-chooser.md` |
| form and colours of a numeric chart | `dataviz`, where installed |
| visual polish and accessibility of a page or deck meant for wide diffusion or web/slide output | `impeccable` (`audit`, `polish`) |
| proof the visual version lost nothing and reads at least as well | `shape` (fact survival, cold retrieval) |
| a deck | the Artifact Slides type or a Typst deck template; its assertion subtitle is rule 0.2 |
| an Artifact page | `artifact-design` for the page contract, then this skill for what to show |
| terminal density | `flow-lean` or `glance` choose the words; this skill chooses the visual |

## 5. Proof

The gates live in `shape`, not here:

| Gate | Instrument | State |
| --- | --- | --- |
| fact survival 100 % | `verify-facts.sh` searches the whole file, so text inside a mermaid block counts; a fact carried by colour alone fails, by design | exists |
| render | `check-render.sh`; `mmdc` needs a browser, so run it outside any sandbox that refuses one | exists |
| retrieval at a screen budget | cold subagent with vision on rendered captures, first K screens, before and after; slice with overlap so no block is cut at a boundary | ⚠️ no script; run once by hand, see `references/worked-example.md` |
| form | the operator's one-line verdict (`--operator-verdict`) | exists |

⚠️ A word search sees words, not relations: a fact split across two nodes
survives the gate even when the arrow between them is wrong.

## 6. Anti-patterns

| Symptom | Fix |
| --- | --- |
| a flowchart for a linear list | numbered list |
| red/green with no sign | add `−` / `+` or ✗ / ✓ |
| a status marker at the end of a cell or line | move it to the front |
| a paragraph narrating the diagram | delete it, keep the why |
| a visual with no source line | add it, or flag ⚠️ unverified |
| raw mermaid sent to Jira or Confluence | pre-render it, or use the declared fallback |
| ASCII art outside a fence | fence it |
| a chart where a three-row table would do | the table |
| a mean and a median standing in for a distribution's shape | a histogram |
| two `xychart` bar series, the later one larger somewhere | it hides the earlier one: reorder, or an SVG |
| a letter handle used before its definition | define it at first use; glossary under the verdict |
| a fallback invented at write time | take it from `references/render-targets.md` |

`references/worked-example.md` takes one page apart rule by rule.
