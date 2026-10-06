# Worked example: the D1–D8 decision page

Source: a decision page published as an Artifact on 2026-10-06, eight
decisions on a search-index migration (v1 → v2). Here it is retold on a
fictional product catalogue: the domain, names and identifiers are invented;
the structure, the rules and the measurements are the page's. The author's
verdict on it: « super ». The author's request it answered, verbatim:

> Concis, plus d'images et diagrammes que de mots. Je veux voir :
> quand ça concerne des données, quelles données sont concernées, en table ;
> quand la structure de données/objets change → diagramme ; changement de
> processus → diagramme de séquence si multi-composants, flowchart si
> algorithme ; différences ancien/nouveau → rouge/vert pour montrer les
> changements.

## Block by block

| Block | Content says | Visuals | Rules visible |
| --- | --- | --- | --- |
| Header | scope, rule in force | colour legend, once | 0.5: each colour named in words, before first use |
| Overview | eight decisions | matrix: `Ref` · line · subject · current (red) · recommendation (green) · basis | 0.1, 0.3 (basis column = source) |
| D1 product-code input | an algorithm that branches; data touched | flowchart with red and green paths; table input × legacy system / v1 / ADR / reco | 1, 0.3; "illustrative code, format only" flags an example value in place |
| D2 Begins / Contains | options sharing criteria; a user-visible change | table operator × legacy / plan / reco; two-line diff | 1; the hedge "probably 0 (… not measured)" kept in place, 0.4 |
| D3 full-text flags | old vs new schema; data touched | field diff; operator table from the acceptance tests | 1, 0.3 with file and lines |
| D4 market filter | a process across four components changes | sequence today, sequence proposed | ⚠️ 0.4: unverified path named where it sits |
| D5 v2 write | a process with a failure branch | one sequence with `alt` | 0.2 verdict defers the open question to its ticket |
| D6 one document per product | a structure change | class diagrams v1 / v2; response-field diff | 1: structure → diagram, consequence → diff |
| D7 vector, HNSW | old vs new config; a gate | config diff; gate flowchart; *conditional* chip | 0.2 verdict says who decides: the measurements |
| D8 token case | a pipeline; a divergence | flowchart LR; three-line diff | 1, 0.3 |
| Footer | provenance | sources with commits and date read | 0.3 |

Every block ends on a verdict strip led by a bold verb (rule 0.2).

### Where the page departs from the rules

- Rule 0.5, colour never alone: the D8 flowchart marks *lowercase* with a
  green outline and no sign. D1 and D7 escape because their node labels carry
  the outcome in words (*0 results, no error*, *no*). A fallback adds ✓ / ✗
  to the label.
- Mermaid from a CDN renders on the Artifact only; every other target needs
  the fallbacks below.

## Fallback 1: D4 in the terminal

A sequence diagram becomes numbered arrows. A `diff` block carries both the
change and the colour, and the terminal colours it (`render-targets.md` [1]):

```diff
# D4 · Market filter · today in −, proposed in +
  1 front → catalog-api          filter market = DE
- 2 catalog-api → search-proxy   filters (no market case in search-proxy)
- 3 search-proxy → index         filter without market
- 4 index → front                every product
+ 2 catalog-api → search-proxy   filters + market
+ 3 search-proxy → index v2      offer.markets contains code
+ 4 index v2 → front             products sold in that market
```

⚠️ Not verified yet: if catalog-api already filters by market while building
the results page, the gap is elsewhere. **Wire** the filter down to the
index, after reading the real path of `market` in catalog-api. Source: spec
§4.2, acceptance tests AT-12 to AT-14.

This fallback, like the table above, is taken from the page's first version.
By the next one the ⚠️ had been checked: the results page already filtered by
market, and the D4 verdict flipped from **Wire** to **Reject** the dead
`market` operator field with HTTP 400. Rule 0.4 is what made the flip cheap:
the doubt sat on the block it could overturn.

## Fallback 2: D3 in a Jira ticket

Markdown through an MCP server. Markdown has no colour syntax on Jira
(ADF has one, `render-targets.md` [15]), so the signs carry the change; no
italics around inline code; no `>` at the start of a cell. The
acceptance-test operator table is dropped for budget, behind the source line.

```markdown
**D3 · Seller and brand filters**

| Field (`catalog-index-v2.json`) | Today | Proposed |
| --- | --- | --- |
| `offers.seller.sellerId` | ✗ full-text: off | ✓ full-text: on |
| `offers.seller.vatNumber` | ✗ full-text: off | ✓ full-text: on |
| `offers.seller.name` | ✗ full-text: off | ✓ full-text: on |
| `brand.name` | ✗ full-text: off | ✓ full-text: on |

**Restore full-text** on the 4 fields: behaviour does not change, the v2
index is empty, and the field-scoped match no longer needs testing.

Source: every filter operator runs a field-scoped full-text match, which
requires a full-text field (`filters.py`:40-58). The schema generator
checked the free-text field list only.
```

## Proof: retrieval at a screen budget

Run on 2026-10-06 against a pre-registered protocol. A is the source document
the page was drawn from, at its last commit before the page: no diagram, and
written before the decisions. B is the page. Six cold vision subagents, one
per version × budget, each got only its K captures (1280 × 800) and eight
questions. Scored against a key, then re-scored blind on shuffled sheets: 47
of 48 cells agree, the one split is the range below.

| K screens | A, content ceiling 6 | B, content ceiling 8 |
| --- | --- | --- |
| 1 | 2.5 | 7 |
| 3 | 3 | 7 |
| all | 4.5–5, 9 screens | 8, 7 screens |

- ✅ B's first screen, the overview matrix, answers 7 of 8: rule 0.1 does
  most of the work at small budgets.
- ⚠️ A predates the decisions, so most of the raw gap is content. On the
  three questions A carries in full, B scores 3 of 3 from its first screen;
  A reaches 3 of 3 only after all 9.
- ⚠️ At K = 3 a diff block cut by the slice boundary cost B half a point:
  slice with overlap.
- ⚠️ One reader per cell, a vision model standing in for a person, the same
  model family for page, questions, readers and scorers, and questions drawn
  from B.
