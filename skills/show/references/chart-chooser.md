# Chart chooser: when a table becomes a chart, and which one

`SKILL.md` §1 sends numbers here. This file decides whether a table earns a
chart and which chart; `dataviz`, where it is installed, then sets the form
and the colours.

## Detect: does this table earn a chart?

Ask in order; stop at the first answer that settles it.

| # | Question | Answer → action |
| --- | --- | --- |
| Q1 | Does the block's verdict rest on comparing numbers? | no → the table stays (a config, a list of versions) |
| Q2 | Which relation does the verdict state? | the relation table below |
| Q3 | How many points? | 3 or fewer → a sentence; 4 to 20 categories → bars; over 20 ordered points → a line or a histogram |
| Q4 | Do the values span two orders of magnitude or more? | shares in %, a log scale, or two charts |
| Q5 | Will the reader look up exact values? | the chart comes **in addition**; the table stays, folded in `<details>` where the target allows it |
| Q6 | Can the target draw it? | the mermaid section below; else a pre-rendered SVG |

## Relation → chart

| The verdict says… | Chart | Avoid |
| --- | --- | --- |
| parts of one whole | pie, 5 parts or fewer | a pie for parts of different units |
| parts of several wholes, compared | 100 % stacked bars, one per whole | several pies side by side: angles do not compare |
| a comparison across categories | bars, sorted | — |
| a distribution's shape (*bounded*, *skewed*) | histogram with a reference (the uniform expectation, a bound) | a mean and a median standing in for the shape |
| a change over time | line | bars past 20 points |
| a concentration (*many but negligible*) | share of count vs share of volume, per size class | — |
| a low / central / high range | a point and its interval | three bars, read as three measurements |
| durations and proportions on a clock | gantt (§ Time on an axis) | `timeline`: its spacing is not proportional |

## Do, or propose

- ✅ Do it without asking when the chart re-encodes numbers already in the
  document, the mermaid source keeps those numbers as text (so the
  fact-survival gate still finds them), and the target draws it.
- ❓ Propose it when it needs data the document does not hold (a full series
  behind a summary table), a reconstruction from a few points, or a choice of
  emphasis among several readings of one table.
- ⚠️ A curve the document holds only as a few points (a start, a peak, an
  end per cycle): plot those points at their true x, join them with dashed
  segments, and say the series between them was not read. Never interpolate
  values to fill an evenly spaced axis.
- ⚠️ Before drawing a trend, check the points compare: a series whose last
  point covers a smaller batch shows a decline that is partly an artefact.
- ✅ A chart drawn from a data file ships with that file and its generator,
  next to the document. The generator asserts every value the caption or the
  text states about the chart and refuses to draw otherwise; the caption
  claims only the checks the generator makes. Prove the asserts bite: change
  one stated value in a copy of the document, and the generator must refuse.
  Example: a caption says the line matches the table's peaks; the generator
  recomputes each peak from the series and compares it with the table cell.

## Time on an axis: gantt, timeline or flow

| The axis must carry… | Visual |
| --- | --- |
| durations and proportions on a clock | gantt |
| intervals that overlap, such as concurrency | gantt, one section per lane |
| an order only | `timeline` or an ordered list |
| causality, branches, hand-offs between components | flowchart or sequence |
| parts of a whole, the clock irrelevant | pie or a 100 % bar |

A proportional ASCII bar stays ASCII while every segment is 5 columns wide or
more and holds its own label. Below that, its labels and numbers spill under
the bar and drift out of line: climb to mermaid.

## Mermaid charts: what they draw, and the traps

Probe, 2026-10-06: `mmdc` 11.12.0, each chart compiled and looked at. A
host bundles its own mermaid version: GitLab drew `gantt` and
`xychart-beta` with two lines over bars the same day (`render-targets.md`
[16]); any other construct or host stays ✓? until probed there.

| Construct | Result |
| --- | --- |
| `pie showData` | ✅ parts with their values in the legend |
| `gantt`, `dateFormat HH:mm:ss` | ✅ proportional clock axis; a bar too short for its label prints the label at its right |
| `gantt`, a `:` inside a task label (`… 05:45 to 05:53 :crit, …`) | ⛔ the label is cut at the first colon and the tags after it (`crit`, `done`) are lost; it still compiles. Keep clock times out of labels |
| `xychart-beta`, `line` over `bar` | ✅ a reference line, such as the uniform expectation on a histogram |
| `xychart-beta`, two `bar` series | ⛔ drawn at the same x, the later one on top, and no legend: a later series larger at any category hides the earlier one |
| `xychart-beta`, cumulative `bar` series, largest first | ✅ a stacked bar (whole, whole minus top part, …) or a range (high, central, low) |
| `xychart-beta`, data at uneven x | ⛔ points are spaced evenly whatever their value: use classes, or an SVG |
| `xychart-beta`, a long title | ⛔ clipped at both edges at the default 700 px width: 68 characters fit, 81 do not. A short title; the series legend goes in the caption |
| scatter, error bars, grouped bars | ⛔ no mermaid construct: SVG |

Two consequences:

- Two bar series are readable only when every later series is smaller at
  every category. Ordered on purpose, that is the stacked bar mermaid lacks:
  series 1 = the whole, series 2 = the whole minus the top part, and so on;
  or a range: high, then central, then low.
- `xychart-beta` has no legend, so the caption names each series by its
  position (*back*, *front*) and its meaning; the title is too short to hold
  it. Colour alone is not a name (rule 0.5).
