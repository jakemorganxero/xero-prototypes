# Xero Data Visualisation — Brand Guidelines

Use this alongside `xero-calculator-brand-guidelines.md` when prototyping charts, graphs, and data index pages for xero.com. The same colour tokens, typography, and AEM constraints apply — this doc adds the chart-specific patterns on top.

---

## Colour tokens (same as calculators)

```css
--xero-navy:       #1B3A4B;
--xero-blue:       #13B5EA;
--xero-blue-dark:  #0D8FBD;
--xero-panel-bg:   #EBF0F4;
--xero-border:     #C2CDD6;
--xero-bg:         #FFFFFF;
--xero-text:       #1B3A4B;
--xero-text-muted: #4A6070;
--xero-green:      #1AB394;
--xero-red:        #E05A5A;
```

---

## Chart colour palette

Use these in order for multi-series charts. Never use more than 4–5 series without a legend.

| Role | Hex | Use |
|---|---|---|
| Primary series | `#13B5EA` | First / only data series |
| Secondary series | `#1AB394` | Second series |
| Tertiary series | `#1B3A4B` | Third series |
| Negative / warning | `#E05A5A` | Negative values, alerts |
| Neutral accent | `#7AABCA` | Background / reference lines |

### Fill gradients (line charts)
Apply a top-to-transparent gradient fill under line charts for depth. Opacity at top: 12–15%.

```css
/* Example for primary blue */
gradient.addColorStop(0, 'rgba(19,181,234,0.13)');
gradient.addColorStop(1, 'rgba(255,255,255,0)');
```

---

## Typography (same as calculators)

```html
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
```

| Element | Size | Weight | Colour |
|---|---|---|---|
| Page / section title | `clamp(1.6rem, 3vw, 2.25rem)` | 700 | navy |
| Eyebrow label | `0.75rem` | 700 | blue, uppercase |
| Chart title | `1.1rem` | 700 | navy |
| Chart description | `0.875rem` | 400 | muted |
| Stat card value | `2rem` | 700 | navy / green / red |
| Stat card label | `0.8125rem` | 600 | muted, uppercase |
| Axis tick | `11px` | 400 | muted |
| Tooltip title | `13px` | 600 | white |
| Tooltip body | `13px` | 400 | white 80% |
| Footnote | `0.8125rem` | 400 | muted |

---

## Layout patterns

Three patterns — choose before building. If not specified, ask.

---

### Pattern 1 — Full section (default)
The standard. Header copy above, stat selector cards, chart below.

```
┌───────────────────────────────────────────────────┐
│  Eyebrow — "Xero Small Business Insights"          │
│  H2: Report title + release date                   │
│  Subtitle copy (1–2 sentences)                     │
│  Meta badges: update cadence, date range, scope    │
├───────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌───────────┐  │
│  │ Stat card   │  │ Stat card   │  │ Stat card │  │
│  │ (clickable) │  │ (clickable) │  │           │  │
│  └─────────────┘  └─────────────┘  └───────────┘  │
├───────────────────────────────────────────────────┤
│  Chart card:  Title + desc | Range pills (1Y…All) │
│                                                    │
│  ╭──────────────────────────────────────────────╮  │
│  │  Chart.js line chart                         │  │
│  ╰──────────────────────────────────────────────╯  │
│  Legend row                                        │
├───────────────────────────────────────────────────┤
│  Footnote / data source                            │
└───────────────────────────────────────────────────┘
```

- `max-width: 960px`, centred, `padding: 48px 24px`
- Stat cards are the metric selector — clicking one updates the chart
- Active card has navy border + blue bottom bar
- Range pills: 1Y / 3Y / 5Y / All

---

### Pattern 2 — Chart only (no editorial header)
Drop the top header section. Starts directly with stat cards (or no stat cards for a single-metric chart). Designed to embed mid-page alongside editorial content managed by AEM.

```
┌───────────────────────────────────────────────────┐
│  ┌─────────────┐  ┌─────────────┐  ┌───────────┐  │
│  │ Stat card   │  │ Stat card   │  │ Stat card │  │
│  └─────────────┘  └─────────────┘  └───────────┘  │
├───────────────────────────────────────────────────┤
│  Chart card:  Title + desc | Range pills           │
│  ╭──────────────────────────────────────────────╮  │
│  │  Chart                                       │  │
│  ╰──────────────────────────────────────────────╯  │
└───────────────────────────────────────────────────┘
```

- No eyebrow, no H2, no subtitle, no meta badges
- Footnote optional
- Comment at top of file: "drop into an AEM editorial section — surrounding copy is AEM's job"

---

### Pattern 3 — Compact card
Single-metric, minimal chrome. Suitable for sidebars, dashboards, or alongside a hero. `max-width: 480px`.

```
┌──────────────────────────────────┐
│  Label          Range: 1Y 3Y All │
│  Big value + delta               │
│  ╭──────────────────────────────╮ │
│  │  Chart (height: 160px)       │ │
│  ╰──────────────────────────────╯ │
│  Footnote (optional)             │
└──────────────────────────────────┘
```

- No stat selector cards — one metric per card
- Chart height: 160px (half of standard 320px)
- Tighter padding: `20px`
- Value: `1.75rem` bold navy
- Delta badge: small pill showing MoM or YoY change, green/red

---

## Chart.js setup

Load from cdnjs — no npm required:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
```

### Standard line chart config

```js
{
  type: 'line',
  data: { labels, datasets: [{ borderWidth: 2, pointRadius: 0, pointHoverRadius: 5, tension: 0.3 }] },
  options: {
    responsive: true,
    maintainAspectRatio: false,
    interaction: { mode: 'index', intersect: false },
    plugins: {
      legend: { display: false },
      tooltip: {
        backgroundColor: 'rgba(27,58,75,0.95)',
        titleColor: '#fff',
        bodyColor: 'rgba(255,255,255,0.8)',
        padding: 12,
        cornerRadius: 6,
      }
    },
    scales: {
      x: { grid: { display: false }, border: { display: false } },
      y: { grid: { color: 'rgba(194,205,214,0.5)' }, border: { display: false } }
    }
  }
}
```

### Annotation bands (e.g. COVID, recession)

Use a custom Chart.js plugin — don't use the annotation plugin (extra dep). Draw directly in `beforeDatasetsDraw`:

```js
const shadingPlugin = {
  id: 'shadingBand',
  beforeDatasetsDraw(chart) {
    const { ctx, chartArea, scales } = chart;
    const x0 = scales.x.getPixelForValue(startIndex);
    const x1 = scales.x.getPixelForValue(endIndex);
    ctx.save();
    ctx.fillStyle = 'rgba(224,90,90,0.07)';
    ctx.fillRect(x0, chartArea.top, x1 - x0, chartArea.height);
    ctx.restore();
  }
};
```

---

## Stat card component

Stat cards serve double duty: they display the latest value AND act as metric selectors (clicking switches the chart).

```css
.xsbi-stat {
  background: var(--xero-panel-bg);
  border-radius: 6px;
  padding: 20px 24px;
  border: 2px solid transparent;
  cursor: pointer;
  transition: border-color 0.15s, background 0.15s;
}
.xsbi-stat.active {
  border-color: var(--xero-navy);
  background: #fff;
  box-shadow: 0 2px 12px rgba(27,58,75,0.08);
}
/* Blue bar along bottom of active card */
.xsbi-stat__indicator {
  position: absolute; bottom: 0; left: 0; right: 0;
  height: 3px; border-radius: 0 0 4px 4px;
}
.xsbi-stat.active .xsbi-stat__indicator { background: var(--xero-blue); }
```

Value colouring:
- Positive % values → `var(--xero-green)`
- Negative % values → `var(--xero-red)`
- Absolute values (days, $) → `var(--xero-navy)`

---

## Range pill component

```css
.xsbi-range-btn {
  border: 1px solid var(--xero-border);
  border-radius: 20px;
  padding: 5px 14px;
  font-size: 0.8125rem;
  font-weight: 600;
  color: var(--xero-text-muted);
  background: none;
  cursor: pointer;
}
.xsbi-range-btn.active {
  background: var(--xero-navy);
  border-color: var(--xero-navy);
  color: #fff;
}
```

---

## Delta badge (Pattern 3 compact)

```html
<span class="xsbi-delta positive">▲ 0.5pp</span>
```

```css
.xsbi-delta {
  display: inline-block;
  font-size: 0.75rem;
  font-weight: 600;
  padding: 2px 8px;
  border-radius: 20px;
  margin-left: 8px;
}
.xsbi-delta.positive { background: rgba(26,179,148,0.12); color: var(--xero-green); }
.xsbi-delta.negative { background: rgba(224,90,90,0.10); color: var(--xero-red); }
```

---

## Data conventions

- Embed data inline as JS arrays for prototypes — note where the API call should go in production
- Always note the data release date in the handoff comment
- Label months as `MM/YYYY` internally; display as `Mon YYYY` (e.g. `Mar 2026`)
- Seasonally adjusted figures: note in axis label or stat card sublabel
- YoY %: prefix positive values with `+`

---

## Annotations to consider

When building: check if any notable events fall in the data range and consider shading them:

| Event | Suggested band |
|---|---|
| COVID disruption | Apr 2020 – Dec 2020, `rgba(224,90,90,0.07)` |
| Data anomalies / spikes | Single vertical line or point annotation |
| Recessions | `rgba(27,58,75,0.05)` |

---

## Handoff comment (required in every file)

```html
<!--
  XERO PROTOTYPE
  Tool: [chart/index name]
  Locale: [AU/NZ/US/UK]
  Built: [date]
  Data source: [XSBI / manually entered / API endpoint]
  Metrics: [list]
  Pattern: [1 – Full section / 2 – Chart only / 3 – Compact card]
  Notes for devs: [data update mechanism, any notable logic]
-->
```

---

## How to request a new data viz prototype

> **"Build a Xero data viz prototype"**
>
> Include:
> - Data source / metrics to show
> - Locale (affects currency labels)
> - Pattern (1 / 2 / 3) — or describe the context and we'll pick
> - Any specific time range, annotations, or comparison series needed

If pattern isn't specified, ask before building.
