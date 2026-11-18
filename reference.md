# MBR slide & chart reference

Companion to [SKILL.md](SKILL.md). Schema mirrors `assets/mbr-template.html`.

## Fiscal calendar

```js
const MONTHS = ['Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec','Jan'];
const MONTH_IDX = [0,1,2,3,4,5,6,7,8,9,10,11];
```

Every monthly `data` array must length 12 in this order.

## Badges & nav dots

| Flag | UI |
|------|-----|
| `isNew: true` | Green `New Metric` badge |
| `isOld: true` | Grey `Legacy — for reference` badge |
| `isPhase2: true` | Purple `Phase 2` badge + purple nav dot |
| `gap: true` | Amber `Data Gap` badge + amber nav gap-dot |

## `line-cy-py` example

```js
{
  id: 1, deck: 'rx',
  section: 'Section 1 — Customer & User Health',
  sectionShort: 'Customer & User Health',
  title: 'Customer Base — Account Lifecycle',
  question: 'How many customers are truly live, and are we growing or shrinking?',
  gap: false, isNew: true,
  chartType: 'line-cy-py',
  metricLabel: 'Active Rate (%)',
  goalValue: 90, goalLabel: 'Goal: 90%',
  // chartUnit: '%' (default) | '' | ' min' | etc. — uses lineRawOpts when not '%'
  currentYear:  { label: 'FY26 (Feb 25 – Jan 26)', data: [/*12*/] },
  previousYear: { label: 'FY25 (Feb 24 – Jan 25)', data: [/*12*/] },
  tableTitle: 'Optional primary table title',
  tableRows: [
    { label: 'Live accounts', highlight: false, data: [/*12*/] },
    { label: 'Active rate (%)', highlight: true, data: [/*12*/], fmt: '%' },
  ],
  secondaryTable: {
    title: 'Breakdown title',
    note: 'Same definition as chart · Full fiscal year (Feb – Jan)',
    tableRows: [ /* same row shape */ ],
  },
  tertiaryTable: { /* same shape as secondaryTable */ },
  notes: 'Optional amber note-box copy. Prefix DATA GAP when applicable.',
}
```

## `stacked-area` / `stacked-bar`

```js
{
  chartType: 'stacked-area', // or 'stacked-bar'
  metricLabel: 'Message Distribution by Hiring Stage (%)',
  stackSeries: [
    { label: 'Lead / Sourcing', color: '#27282C', data: [/*12 % shares*/] },
    { label: 'Screening', color: '#7A818C', data: [/*12*/] },
  ],
  tableRows: [ /* optional */ ],
}
```

Series percentages should sum ~100 per month for area/bar stacks.

## `jtbd`

```js
{
  chartType: 'jtbd',
  metricLabel: 'JTBD stage coverage',
  stageData: [
    { name: 'Sourcing', total: 68, goal: 75 },
  ],
  tableRows: [
    { label: 'Sourcing', oneway: '52%', twoway: '78%', phire: '70%', total: '68%', goal: '75%' },
  ],
}
```

## `defects` / `sr`

```js
{
  chartType: 'defects', // or 'sr'
  metricLabel: 'Defect Volume with Root Cause Stack (SLA Compliance % on right axis)',
  tabs: ['Core', 'Automations', 'Profile', 'Messaging'],
  activeTab: 0,
  tabData: [
    {
      // defects only:
      sla: [89,93,94,92,95,96,97,96,97,98,97,98],
      goalMax: 20,
      stacks: [
        { label: 'Product bug', color: C.red, data: [/*12*/] },
        { label: 'Knowledge gap', color: C.amber, data: [/*12*/] },
      ],
    },
  ],
}
```

`sr` omits `sla` (no right-axis line).

## `exec-summary` (AE)

```js
{
  chartType: 'exec-summary',
  metrics: [
    {
      cardTitle: 'Recruiter value — hours saved & cost',
      rows: [
        { label: 'Recruiter hours saved / month', value: '3,420', delta: '+6.5% MoM', tone: 'up' },
      ],
    },
    { label: 'Live customers on AE', value: '84', delta: '+5 MoM', tone: 'up' },
  ],
}
```

`tone`: `up` | `down` | `neutral` → `.exec-up` / `.exec-down` / `.exec-neutral`.

## `three-blocks` (AE HLR)

```js
{
  chartType: 'three-blocks',
  blocks: [
    { variant: 'highlights', title: 'Highlights', bullets: ['…'] },
    { variant: 'lowlights', title: 'Lowlights', bullets: ['…'] },
    { variant: 'support', title: 'Support Required', bullets: ['…'] },
  ],
}
```

Variant colors (hardcoded in renderer):

- highlights → green bg `#ECFDF5` / border `#a7f3d0` / title `#065F46`
- lowlights → red bg `#FEF2F2` / border `#fecaca` / title `#991b1b`
- support → orange bg `#FFF7ED` / border `#fed7aa` / title `#92400E`

## `plain-table`

```js
{
  chartType: 'plain-table',
  tableTitle: 'Title',
  columns: ['Col A', 'Col B'],
  rows: [['a', 'b'], ['c', 'd']],
}
```

## Deck switcher

```html
<select id="deck-select" onchange="switchDeck(this.value)">
  <option value="rx">RX — Recruiter Experience</option>
  <option value="ae">Automation Platform</option>
</select>
```

`activeDeck` filters `SLIDES` by `s.deck`. Global comment IDs: RX uses `currentIdx`; AE uses `100 + currentIdx`.

## What was removed vs source template

Source: CRM Core `MBR template.html`.

Removed intentionally:

- `#pw-gate` markup
- Password-gate CSS
- Password IIFE (`checkPassword`, `wbr_auth`, hardcoded access code)

Everything else (CSS, shell, Chart engine, Supabase comments) is preserved in `assets/mbr-template.html`.
