# Xero Calculator, Template & Data Viz Prototyping

You are helping prototype calculator, template, and data visualisation tools for embedding on xero.com — specifically under `/calculators/`, `/templates/`, and data/insights pages. These are used by the Xero marketing team and handed off to the dev team for production implementation.

## Before you build anything

**Calculators / generators:**
1. Read `xero-calculator-brand-guidelines.md` — exact colours, fonts, layout patterns, and component CSS
2. Use `xero-calculator-template.html` as your starting point

**Data visualisations / charts:**
1. Read `xero-dataviz-brand-guidelines.md` — chart colours, Chart.js config, stat card/range pill components, all three layout patterns
2. Use `xero-dataviz-template.html` as your starting point

## Output rules

- Every prototype is a **single self-contained HTML file** — all CSS and JS inline, no build step required
- **Calculators / generators** → save to `calcs-generators/`
- **Data viz** → save to `prototypes/`
- Filename format: `xero-[tool-name]-[locale].html` e.g. `xero-gst-calculator-au.html`
- Data viz naming: `xero-xsbi-[locale]-[metric].html`, append `-half` for half-width card variants
- The tool must work as an **embeddable section**, not a full page — use `max-width: 960px`, no `<html>`/`<body>` styling that assumes full-page ownership
- Test that calculate and reset both work before considering it done

## Locale handling

- **AU** → `$` AUD, GST = 10%, tax terminology is Australian
- **NZ** → `$` NZD, GST = 15%, use NZ terminology
- **US** → `$` USD, no GST, use "sales tax" where relevant, spell out "check" not "cheque"
- **UK** → `£` GBP, VAT = 20%, use UK terminology
- If no locale is specified, ask before building — copy and currency symbol differ per region

## When to ask vs just build

**Just build it** if the request includes:
- Tool name
- Key inputs
- What the result(s) should be
- Locale

**Ask first** if any of these are missing and they'd materially affect the output (especially locale and result logic). Keep it to one question, not a list.

## Layout patterns — ask if not specified

There are three layout patterns. If the request doesn't specify, **ask which one** before building.

### Pattern A — Standard body section (default)
- Two-column: form left (~60%), results panel right (~40%)
- Lives mid-page as a content section, `max-width: 960px`
- Has a "Calculate" button — results show on submit
- Results panel: light grey bg, line items with dividers, CTA at bottom
- Best for: tools with many inputs (5+), detailed breakdowns

### Pattern B — Hero, full-width
- Full-viewport-width band with heading + copy left, calculator card right
- Calculator is a white card with box-shadow, `max-width: ~480px`
- **Live calculation** — results update as user types, no Calculate button needed
- Result appears in a shaded band inside the card
- Best for: simple tools (1–4 inputs), when the tool IS the main page feature
- Triggered by: "above the fold", "hero calc", "full-width header"

### Pattern C — Hero, half-width card only
- Render **just the calculator card** at `max-width: 400px`
- Add a comment in the file: "drop this card into the right column of the existing AEM hero section"
- Don't prototype the full hero — the surrounding page is AEM's job
- Best for: tools that sit beside existing hero content (image, copy)
- Triggered by: "half-width header calc", "beside the hero", "card only"

## Calculator copy pattern (all layouts)

- Button (Pattern A): "Calculate your [tool name]" / Reset link beside it
- Results panel / card ends with: *"Get a real time view of your [X] with Xero. Start a 30 day [free trial]."*
- Footnote: `*This calculator provides estimates only and is not financial or accounting advice.`

## Template/generator pattern (single column or stepped)

For tools like invoice generators or contract templates:
- Use a single-column form or a stepped/tabbed layout if there are many fields
- Output section renders a formatted preview (e.g. a styled invoice) below or beside the form
- Include a "Download" or "Copy" button where useful (use the Clipboard API or a print-to-PDF trigger)
- Still follows the same brand tokens and component styles

## Handoff notes to include in every file

Add this HTML comment near the top of every prototype, filled in:

```html
<!--
  XERO PROTOTYPE
  Tool: [name]
  Locale: [AU/NZ/US/UK]
  Built: [date]
  Inputs: [brief list]
  Output/formula: [brief description]
  Notes for devs: [anything non-obvious about the logic or layout]
-->
```

## What the dev team needs to know

- xero.com runs on Adobe Experience Manager (AEM) — server-rendered HTML
- These prototypes are vanilla JS widgets, matching how the existing cash flow calculator is built
- The single HTML file is the spec — devs adapt it to AEM, they don't need a framework handoff
- GT Walsheim is Xero's actual font (licensed); prototypes use DM Sans from Google Fonts as a stand-in — flag this in the handoff comment

## Folder structure

```
xero-prototypes/
├── CLAUDE.md                              ← this file
├── index.html                             ← prototype library index (GitHub Pages homepage)
├── xero-calculator-brand-guidelines.md    ← calc/generator design system
├── xero-calculator-template.html          ← calc/generator starter
├── xero-dataviz-brand-guidelines.md       ← chart/data viz design system
├── xero-dataviz-template.html             ← chart/data viz starter
├── calcs-generators/
│   └── xero-[tool-name]-[locale].html
└── prototypes/
    └── xero-xsbi-[locale]-[metric].html   ← data viz files
```

## GitHub / deployment

- **Repo:** https://github.com/jakemorganxero/xero-prototypes
- **GitHub Pages:** https://jakemorganxero.github.io/xero-prototypes/
- **Index page:** https://jakemorganxero.github.io/xero-prototypes/ (filterable by type + locale)
- Calculators/generators live in `calcs-generators/`; data viz live in `prototypes/`
- Push with PAT token embedded in remote URL — token stored separately, ask user if expired
- Work directly on `main` branch — no PR flow needed for this repo
- After pushing, GitHub Pages rebuilds automatically (takes ~1–2 min to go live)

## Current prototype library (70 files)

### Calculators (Pattern B hero, live calculation)
| Tool | Locales |
|---|---|
| GST Calculator | AU, NZ |
| VAT Calculator | UK |
| Sales Tax Calculator | US (address-based state detection, Avalara-ready) |
| Income Tax Calculator | AU, NZ (Pattern A), UK (Pattern B) |
| Markup Calculator | AU, NZ, US, UK |
| Net Profit Margin Calculator | AU, NZ, US, UK |
| Gross Margin Calculator | AU, NZ, US, UK |
| Business Loan Calculator | AU, NZ, US, UK (PMT formula, frequency options) |
| Depreciation Calculator | AU, NZ, US, UK (live schedule table, straight-line + declining balance, CSV download) |
| Timesheet Calculator | AU, NZ, US, UK (inline time-entry table, OT threshold, CSV download) |

### Generators (two-column form + sticky live preview)
| Tool | Locales |
|---|---|
| Invoice Generator | AU, NZ, US (preview variant), UK |
| Quote Generator | AU, NZ, US, UK |
| Receipt Generator | AU, NZ, US, UK |
| Purchase Order Generator | AU, NZ, US, UK |
| Payslip / Pay Stub Generator | AU, NZ, US, UK |
| Expense Report Generator | AU, NZ, US, UK |
| Timesheet Generator | AU, NZ, US, UK |

## Key technical patterns

### Generators
- `lineItems[]` array as source of truth
- `renderLineItems()` only on add/remove; `updateItem()` on keystroke
- `buildDocument()` / `updatePreview()` for live preview
- Downloads: PDF via `window.print()`, Word via HTML blob `.doc`, Excel via HTML table `.xls`, CSV

### Calculators
- PMT formula (business loan): `P × r(1+r)^n / ((1+r)^n - 1)`
- Depreciation: straight-line `(cost-salvage)/life`, declining balance `opening × rate`
- US state detection: regex on address input field → RATES object with 50 states + DC

### Modified B layout (depreciation / timesheet calculators)
```css
.xero-hero__inner {
  display: grid;
  grid-template-columns: 480px 1fr; /* 400px 1fr for timesheet */
  gap: 48px;
  max-width: 1200px;
  margin: 0 auto;
}
@media (max-width: 900px) {
  .xero-hero__inner { grid-template-columns: 1fr; }
}
```

## Strategic context

This prototype library is part of Xero's Q1 FY27 US organic growth programme — closing the gap with QuickBooks on utilitarian content (templates, calculators, generators). Key points:
- Existing Outgrow-powered calculators are being replaced with these native AEM-compatible prototypes
- Native prototypes unlock generators as a net-new content type (Outgrow can't produce them)
- AirOps automates content production at scale; prototypes are the dev-ready specs
- Each tool concept → up to 4 publishable pages (AU/NZ/US/UK)

## Data viz layout patterns — ask if not specified

Three patterns (documented in full in `xero-dataviz-brand-guidelines.md`):

- **Pattern 1 — Full section**: header copy + eyebrow + meta badges + stat selector cards + chart. Default for standalone data index pages.
- **Pattern 2 — Chart only**: no header, starts at stat cards. Drop into an AEM editorial section alongside existing copy. Also used for single-metric **standalone** embeds (full-width, 960px).
- **Pattern 3 — Compact cards**: single-metric mini-cards, 150px chart, delta badge, 480px max-width. For sidebars, dashboards, AEM column grids, or **half-width** embeds beside editorial copy.

Uses Chart.js 4.x from cdnjs. Data embedded inline as JS arrays for prototypes; note where the API call goes in production.

### Existing XSBI US data viz files (`prototypes/`)

Data source: Xero Small Business Insights, US national data, March 2026 release (Jan 2017–Mar 2026).
Three metrics: Sales YoY %, Time to be paid (days, seasonally adjusted), Late payment days (seasonally adjusted).

| File | Pattern | Description |
|---|---|---|
| `xero-xsbi-data-us.html` | 1 – Full section | All 3 metrics, stat selector cards, range pills |
| `xero-xsbi-data-us-chart-only.html` | 2 – Chart only | No header, stat cards + chart — AEM embed |
| `xero-xsbi-data-us-compact.html` | 3 – Compact | Three side-by-side mini-cards, one per metric |
| `xero-xsbi-us-sales.html` | 2 – Standalone | Sales YoY only, full-width (960px) |
| `xero-xsbi-us-sales-half.html` | 3 – Half-width | Sales only, 480px card — for 50% AEM column |
| `xero-xsbi-us-ttp.html` | 2 – Standalone | Time to be paid only, full-width |
| `xero-xsbi-us-ttp-half.html` | 3 – Half-width | Time to be paid, 480px card |
| `xero-xsbi-us-late-payments.html` | 2 – Standalone | Late payments only, full-width |
| `xero-xsbi-us-late-payments-half.html` | 3 – Half-width | Late payments, 480px card |

All charts: COVID shading band Apr–Dec 2020, hover tooltips, 1Y/3Y/5Y/All range pills.
Half-width files: x-axis tick labels hidden (too cramped), tooltip still shows month + value on hover.

### When to use standalone vs multi-metric

- **Multi-metric dashboard** (`xero-xsbi-data-us.html` variants): landing pages, XSBI hub, press/media pages
- **Single-metric standalone** (full-width): embed on a related content page e.g. sales chart on a "growing your revenue" article
- **Single-metric half-width**: embed beside editorial copy or an image in an AEM two-column layout

### Updating the index page

When adding new data viz prototypes, update `index.html`:
1. Add a card to the Data Visualisations section (copy existing card structure, `data-type="dataviz"`)
2. Update the total count in the stats bar
3. Commit and push — GitHub Pages rebuilds in ~1–2 min
