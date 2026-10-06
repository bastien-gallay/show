# Render targets: capability matrix and declared fallbacks

Fallbacks are declared here, never improvised at write time. An editor who
discovers at write time that mermaid does not render invents a workaround and
gets it wrong. This file extends `shape`'s `references/render-targets.md`,
which stays the source for what its gates check.

## Legend

| Mark | Means |
| --- | --- |
| ✅ | sourced: the source is named in the notes below the matrix |
| ✓? | known, not tested in this setup |
| ❓ | unknown: to test with a probe page |
| ⛔ | not available |

## Matrix

| Target | Diagram | Red/green | Tables | Mono block | Image, SVG | Interactive |
| --- | --- | --- | --- | --- | --- | --- |
| Terminal, Claude Code | ⛔ mermaid shows as source; ✅ ASCII in a block [2] | ✅ `diff` block coloured [1]; ✓? emoji 🟥 🟩 | ✅ width-bound [2] | ✅ [1] [2] | ⛔ | ⛔ |
| `.md` and MR, GitLab | ✅ mermaid [3] [11], `gantt` and `xychart-beta` included [16]; ✓? `classDef` colours | ✅ `diff` block [11]; ✅ inline `{+ +}` `{- -}` `[+ +]` `[- -]` [11] | ✅ [4] [11] | ✅ [4] [11] | ✅ SVG by relative path [16] | ⛔ |
| `.md`, GitHub | ✅ mermaid [5] | ✓? `diff` block; ✓? no inline diff | ✅ [5] | ✓? | ✓? | ⛔ |
| Typst | ✅ pre-rendered SVG, ⚠️ `htmlLabels: false` [12]; ❓ native packages | ✅ free colour, ⚠️ escape a leading `+` [12] | ✅ cell fill [12] | ✓? | ✅ SVG [12] | ⛔ |
| Artifact, HTML | ✅ mermaid from CDN [6] | ✅ [6] | ✅ [6] | ✅ [6] | ✅ SVG, as mermaid draws [6] | ✓? uPlot, ECharts (the Artifact CDN allowlist not checked) |
| Confluence | ⛔ native [5]; ✅ attached image [7] | ✅ `diff` code macro; ✅ status lozenge; ✅ coloured text; ✅ cell background [14] | ✅ [5]; ✅ emoji [14] | ✅ code macro, numbered lines [14] | ✅ [7] [8]; ✅ `mmdc` SVG with width and height [14]; ⛔ a REST draft published from the editor [14] | ⛔ native; ❓ marketplace apps |
| Jira, ADF via REST v3 | ⛔ mermaid stays code [15] | ✅ `diff` code block; ✅ status lozenge; ✅ coloured text; ✅ cell background [15] | ✅ emoji [15] | ✅ code block, numbered lines [15] | ❓ | ⛔ |
| Jira, MCP markdown | ⛔ [5] | ⛔ no colour syntax; ✓? `diff` block, if mapped to an ADF code block [15]; ✅ emoji [5] | ✅ [5] [9], ⚠️ leading `>` eaten | ✅ code block [5] | ❓ through MCP | ⛔ |
| Slack message, via the MCP | ⛔ mermaid stays a code block [13] | ✅ `diff` block coloured [13]; ✅ emoji [13]; ✅ `~~strike~~` [13]; ⛔ inline `{+ +}` [13] | ✅ [13] | ✅ [13] | ✓? upload [10] | ⛔ |
| Slack canvas | ❓ | ❓ | ✓? | ❓ | ❓ | ⛔ |

### Sources

1. A capture, 2026-10-06: `-` lines red, `+` lines green, `#` lines grey.
2. Daily use: review tables in the terminal, copied with `/copy`.
3. Indirect: a broken mermaid block pushed to a docs repository was spotted
   by reading it on GitLab, so GitLab draws mermaid. Not probed directly.
4. Docs-as-code documents, read on GitLab daily.
5. `shape` `references/render-targets.md`.
6. The decision page of `worked-example.md`, an Artifact, 2026-10-06.
7. Pages published by CI with attached images, in a private project.
8. Confluence Cloud drops `ac:alt` and `ac:title` on `<ac:image>`: put the
   caption in the text. Uploading an SVG attachment needs a classic API
   token; a scoped token reaches API v2 only, which has no attachment
   endpoint.
9. Measured 2026-07-30: a cell `>0` rendered as `0`, the inverse (`shape`).
   Also, an italic span holding inline code is cut at the first backtick,
   measured 2026-09-18 on a ticket description.
10. Upload tools exist in the Slack MCP (`slack_get_file_upload_url`,
    `slack_complete_file_upload`); rendering not checked. Every message sent
    through the MCP carries a footer ("Envoyé avec Claude" in a French locale)
    that the call cannot remove.
11. Probe, 2026-10-06: `POST /api/v4/markdown` (`gfm=true`, no write) on
    gitlab.com returned `js-render-mermaid` for mermaid blocks, `gd` / `gi`
    spans in a `diff` block, `idiff deletion` / `idiff addition` spans for
    both inline syntaxes, and a `<table>`. Mermaid draws client-side, so the
    `classDef` stroke colours were not seen; GitLab's asset proxy listed the
    style strings as URLs (`data-proxied-urls`), effect on the drawing unknown.
12. Probe, 2026-10-06, `typst 0.15.1`, compiled and looked at: cell fill,
    coloured text and an SVG image render. Two traps, both fixed and
    re-checked: `mmdc`'s default SVG puts labels in `foreignObject`, which
    Typst drops (empty boxes); render with
    `-c '{"flowchart":{"htmlLabels":false}}'` for text labels. In Typst
    markup a leading `+` starts a numbered list item (`[+ new]` printed
    `1. new`); write `[\+ new]`.
13. Probe, 2026-10-06, a message to the author's own DM through the Slack
    MCP, judged on a capture: the table renders, emoji included; the `diff`
    block is labelled "Diff" and coloured red/green; a mermaid block stays a
    code block labelled "Mermaid"; GitLab inline diff stays literal;
    `~~strike~~` renders. Read back through the API, the fences lose their
    language tag and the table is absent from the text field: judge Slack on
    the render, never on the read-back. A message typed in the Slack composer
    was not tested.
14. Probe, 2026-10-06: a draft in a personal space, created by a script the
    author ran, then published privately; judged on a capture and on the
    `body.view` HTML. The `diff` code macro colours `-` lines red and `+`
    lines green, with line numbers; status lozenges, coloured text (with a
    dark-mode variant) and `data-highlight-colour` cells render; emoji
    survive. Images, re-probed the same day (A2, A3): a PNG, an `mmdc` SVG
    with `width` and `height` set on its root, and the raw `mmdc` SVG
    (`width="100%"`, no `height`), attached by REST. ⛔ The draft opened and
    published from the editor showed none of the three: its body came out
    with `UNKNOWN_MEDIA_ID` in every media node, so the first probe's
    *Preview unavailable* on the SVG blamed the file for the editor's fault.
    ✅ The storage body written again through REST (`PUT`, status
    `current`, as the CI does [7]) and the page only viewed: all three show.
    ⚠️ The raw SVG still renders as a broken image in `body.view`, the
    server-side HTML; exports and e-mail were not tested, so set `width` and
    `height` on the SVG root. Jira not tried.
15. Probe, 2026-10-06: a throwaway Bug in a dormant project, created through
    REST v3 with an ADF body (the Rovo MCP's `createJiraIssue` answered 403),
    judged on a capture of the issue view, then cancelled. The view
    renders the `diff` code block red/green with line numbers, status
    lozenges, `textColor` text, `tableCell` backgrounds, emoji, strike, bold
    and inline code; a `mermaid` code block stays code. `renderedFields`, the
    legacy renderer, drops the code language and the cell backgrounds and
    turns lozenges into bold coloured text: judge Jira on the issue view,
    never on `renderedFields`. Read back through the MCP, the ADF comes out
    as Markdown with fences tagged `diff` and `mermaid`.
16. Probe, 2026-10-06: a page of a docs repository on gitlab.com, judged by
    the author on the rendered page: two hand-written SVG charts linked by
    relative path (`![…](charts/x.svg)`, text kept as `<text>`), an
    `xychart-beta` with two `line` series over a `bar` series, and `gantt`
    charts render. The same page's `pie` and stacked bars were not asked
    about; GitLab's bundled mermaid version was not read.

## Declared fallbacks

| Construct | Unavailable → use, in order |
| --- | --- |
| mermaid diagram | pre-rendered SVG or PNG with the caption in the text (on Confluence an attached image whose body is written through REST, never re-published from the editor [7] [14]) → a table of the same relation → ASCII in a mono block |
| sequence diagram | image → numbered arrows `A → B : message` in a mono block |
| class or ER diagram | image → table field × before / after, rows signed |
| red/green colour | `diff` block where it is coloured → sign alone (`−` / `+`, ✗ / ✓); never colour alone |
| inline diff (GitLab `{+ +}` `[- -]`) | ✓? `~~old~~ → **new**` → `old → new` with signs |
| interactive chart | static SVG or PNG from the same data → table |
| SVG on a target without images | the mermaid or ASCII rung below it |
| table wider than 80 columns on `terminal` or `plain` | definition list |
| comparison symbol at the start of a Jira cell | words: *above*, *one or more*, *at most* |
| emoji marker | its word form in brackets (`shape` `references/markers.md`) |
| footnote | inline parenthetical |

## Width

`terminal` and `plain` assume 80 columns. An ASCII diagram stays under 80
columns on every target.

## Rendering mermaid locally

`shape`'s `check-render.sh` compiles every block. `mmdc` needs a browser:
where a sandbox refuses it, run `mmdc` outside the sandbox, and read each
`mmdc` exit status rather than the pipe's. A diagram never rendered is not a
diagram that renders.

## Write paths on Atlassian Cloud

⚠️ If Rovo MCP writes answer `403 The app is not installed` while reads work
(seen on one site from 2026-09-28 to at least 2026-10-06), fall back to the
REST API with a classic API token. Re-read the stored body after every
write: the API confirms the payload sent, not what the
renderer kept. On Jira, send ADF to REST v3: it carries colour, lozenges and
cell backgrounds [15]; wiki markup through REST v2 was not probed.
