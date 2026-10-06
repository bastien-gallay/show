# Page patterns by genre

The genre's format defines the slots; the grammar in `SKILL.md` §1 fills
them. Each pattern names its budget and the visual that survives when room
runs out. Where a team has its own template for a genre, the template wins
and this file only fills its slots.

## Multi-item decision page

Budget: a page. Reference: the D1–D8 page, `worked-example.md`.

| Part | Content |
| --- | --- |
| Header | title = what is being decided; one sub line: scope, the rule in force, the colour legend once |
| Overview matrix | `Ref` · subject · current state (red, signed) · proposal (green, signed) · basis |
| One block per item | handle + heading phrased as the reader's question · one or two visuals · source line · verdict strip |
| Footer | sources with commit and date read |

Chips on a block head say which visual family it carries (data, schema,
structure, conditional) so a reader scanning for "what touches the data"
finds it without reading.

## Jira ticket, Decision type

Budget: one ticket. Format: the team's ticket template; Markdown through the
MCP, or ADF through REST v3 where MCP writes fail (`render-targets.md` [15]).
ADF carries colour; Markdown does not.

```text
## Context            2–3 lines: where, shortest path to the gap
## Decision           table: current ✗ · options · recommendation ⭐
## Expected benefits  bullets
```

- The Decision table is the overview matrix with one row per option. A
  rejected option keeps its row and one line on why.
- No mermaid: a table of the same relation, or a pre-rendered image attached.
- Signs over colour. No `>` at the start of a cell; no inline code inside
  italics.

## MR description

Budget: 4–6 lines. Format: the team's MR template.

| Line | Content |
| --- | --- |
| Title | conventional commit, ticket key in parentheses when one exists |
| Subtitle | the goal or the problem solved |
| Body | impact bullets, bold key term first |
| Visual | at most one: a `diff` block when the MR changes a contract, a config or a behaviour; or one table |

Do not paraphrase the code diff; the visual shows the behaviour before and
after, which the code diff does not.

## Release note, Confluence

Budget: a page, facts not narrative. Format: the team's release-note template.

- A bold status line on top.
- The *Prod before → Prod after* columns are the red/green: the sign or the
  arrow carries the change; a cell colour adds to it (✅ cell background,
  `render-targets.md` [14]).
- No mermaid: an attached PNG with its caption in the text.

## Terminal answer

Budget: one screen.

| Content | Visual |
| --- | --- |
| the result | first line |
| three or more items the reader will pick from | overview table with handles |
| a flow | ASCII diagram in a fenced block |
| old vs new | ✅ a `diff` block, coloured (`render-targets.md` [1]) |
| more than a screen | the detail goes to a file or an Artifact; the answer keeps the verdict and the pointer |

## Slack message

Budget: one message, no scroll.

- One verdict line, then three lines or bullets at most, `label: value`.
- One visual: a small table, a `diff` block, or a pre-rendered image.
- ✅ Tables and coloured `diff` blocks render when sent through the MCP
  (probe 2026-10-06); mermaid and GitLab inline diff do not.
- Red/green: a `diff` block for lines, emoji plus words in a table cell
  (🟥 before · 🟩 after).
- The rest behind a link.

## Slide

Budget: one full-frame visual. Format: the deck template.

- Subtitle = an assertion, never a label: the verdict rule.
- One visual per slide, never a long bullet list.
- A projection is a cone bounded by two named rates, never a single line.
- Every projected figure traces to a measurement date and a file.
