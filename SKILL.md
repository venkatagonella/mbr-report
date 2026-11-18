---
name: mbr-report
description: >-
  Builds Phenom Weekly/Monthly Business Review (WBR/MBR) HTML decks that match
  the CRM Core MBR shell exactly — Poppins, sidebar nav, CY/PY Chart.js slides,
  fiscal Feb–Jan tables, AE exec summary / three-blocks, badges, note-boxes, and
  optional section comments. Use when the user asks for an MBR, WBR, business
  review deck, RX/AE review prototype, or to update slides in this HTML shell.
  Do not add password protection.
---

# MBR / WBR Report Shell

Build self-contained HTML business-review decks that reuse **exactly** the styles,
DOM shell, color tokens, Chart.js renderers, and slide data schema from
[assets/mbr-template.html](assets/mbr-template.html).

**Never add password protection** (no `#pw-gate`, no `checkPassword`, no session auth).

## When to use

- New or updated WBR / MBR / “business review” HTML for RX, Automation Engine, or sibling decks
- Adding/editing slides, charts, tables, or decks inside this shell
- Prototyping with simulated FY data (Feb → Jan)

## Workflow

1. **Start from the canonical shell** — copy `assets/mbr-template.html` (or an existing deck derived from it). Do not rewrite CSS/layout from scratch.
2. **Update brand chrome** only as needed: `#sidebar-brand`, `#deck-select` options, `#proto-banner`, `<title>`.
3. **Edit slide data** in `const SLIDES` / `AE_SLIDES` (then `SLIDES.push(...AE_SLIDES)`). Keep fiscal months `['Feb'…'Jan']` (12 values).
4. **Preserve the engine** — navigation, `render()`, chart helpers, table helpers, AE layouts, keyboard arrows. Change data, not structure.
5. **Comments (optional)** — Supabase section threads are already wired. Leave as-is unless the user asks to remove or re-point them.
6. **Deliver** a single `.html` file the user can open in a browser. For a minimal demo, see [samples/sample-wbr.html](samples/sample-wbr.html).

## Hard rules (match template exactly)

| Rule | Detail |
|------|--------|
| Font | Google Fonts Poppins 400/500/600/700 |
| Charts | Chart.js 4.4.0 + chartjs-plugin-datalabels 2.2.0 (CDN) |
| Layout | `#sidebar` (260px) + `#main` card; `#proto-banner` + `#topbar` + `#slide-body` |
| Colors | Use `C` token object — do not invent brand colors |
| Months | Always 12 fiscal months Feb→Jan |
| Password | **Forbidden** — strip if present in source material |
| Selectors | Keep existing IDs/classes (`nav-item`, `card`, `tcard`, `badge-*`, etc.) |

Color tokens (from template `C`):

```
heading #27282C · body #353B46 · secondary #464F5E · muted #7A818C
accent #7c3aed · border #e2e8f0 · bg #F1F5F9 · widget #F4F6FA
currentYear #27282C · previousYear #cbd5e1 · goal rgba(124,58,237,0.35)
emerald #10b981 · amber #f59e0b · red #ef4444 · sky #0ea5e9
```

## Slide object schema

Required fields on every slide:

```js
{
  id: 1,                          // nav number (per deck)
  deck: 'rx' | 'ae',              // filter key for deck switcher
  section: 'Section 1 — …',       // full section title
  sectionShort: 'Customer & User Health',
  title: 'Customer Base — Account Lifecycle',
  question: 'How many customers are truly live…?',  // italic callout
  gap: false,                     // amber “Data Gap” badge + nav dot
  isNew: true,                    // green “New Metric” badge
  // optional: isOld, isPhase2
  chartType: 'line-cy-py',        // see chart types below
  // …type-specific fields
}
```

Optional shared fields: `notes`, `tableTitle`, `tableNote`, `tableRows`,
`secondaryTable`, `tertiaryTable`, `metricLabel`, `goalValue`, `goalLabel`,
`chartUnit`, `cohortCols`, `cohortLabels`.

### `tableRows` row shape

```js
{ label:'Active rate (%)', highlight:true, data:[/* 12 nums */], fmt:'%' }
// placeholder row (no data yet):
{ label:'Active customers', ph:true, highlight:false }
// null cells render as '-'
```

### Chart types → renderer

| `chartType` | Fields | Renderer |
|-------------|--------|----------|
| `line-cy-py` | `currentYear`, `previousYear`, optional `goalValue`/`goalLabel`/`chartUnit` | `renderCYPYCard` |
| `stacked-area` | `stackSeries[{label,color,data}]` | `renderStackedAreaCard` |
| `stacked-bar` | same as stacked-area | `renderStackedBarCard` |
| `jtbd` | `stageData`, special `tableRows` | `renderJTBD` + `renderJTBDTable` |
| `defects` | `tabs`, `activeTab`, `tabData` (stacks + optional `sla`) | `renderDefectsCard` |
| `sr` | same as defects, no SLA line | `renderDefectsCard` |
| `exec-summary` | `metrics[]` (single KPI or `rows` card) | `renderExecSummary` |
| `three-blocks` | `blocks[{variant,title,bullets}]` — variants: `highlights` \| `lowlights` \| `support` | `renderThreeBlocks` |
| `plain-table` | `columns`, `rows` | `renderPlainTable` |

Full field examples and AE patterns: [reference.md](reference.md).

## DOM shell (do not invent a new one)

```html
<body>
  <div id="sidebar"> … brand · deck-select · #slide-nav … </div>
  <div id="main">
    <div id="proto-banner">…</div>
    <div id="topbar">… section/title · Prev / counter / Next …</div>
    <div id="slide-body"></div>
  </div>
  <script> /* Chart defaults → SLIDES → buildNav/render → helpers → optional Supabase */ </script>
</body>
```

Slide body order (from `render()`): badges → question → note-box → chart/layout → primary table → secondary → tertiary. Comment pills wrap chart/table sections via `wrapSection`.

## Editing checklist

- [ ] Copied from `assets/mbr-template.html` (no password gate)
- [ ] CSS block unchanged unless fixing a true bug
- [ ] Every series has **12** fiscal months
- [ ] `deck` set; RX slides default via `SLIDES.forEach(s => { if (!s.deck) s.deck = 'rx'; })`
- [ ] Headline metric row uses `highlight:true`
- [ ] Data gaps: `gap:true` + honest `notes` (prefix `DATA GAP` when applicable)
- [ ] Opens in browser: sidebar nav, Prev/Next, keyboard arrows, charts render

## Additional resources

- Canonical shell (password stripped): [assets/mbr-template.html](assets/mbr-template.html)
- Slide / chart field reference: [reference.md](reference.md)
- Minimal working sample: [samples/sample-wbr.html](samples/sample-wbr.html)
