# Xero Prototype Design System — Canonical Spec

**Single source of truth** for every calculator, generator, and data-viz prototype.
Last updated: 2026-06-10 (codifies the June 5 2026 design updates).

> If a prototype disagrees with this file, the prototype is wrong. All shared UI uses the
> `xero-` class prefix — no bespoke per-tool prefixes (`xpog-`, `xts-`, `xtax-`, `inv-`, `ps-`, `xsbi-` etc.).

---

## 1. Design tokens (`:root`) — identical in every file

```css
:root {
  /* Brand */
  --xero-navy:        #000856;  /* Midnight — all text, headings, table headers, primary button */
  --xero-text:        #000856;
  --xero-blue:        #13B5EA;  /* accent / links / focus ring */
  --xero-blue-dark:   #0D8FBD;  /* hover for blue links/accents ONLY */
  --xero-green:       #186241;  /* Pine — accessible positive (NEVER #1AB394) */
  --xero-red:         #E05A5A;  /* negative values only (gross/net margin, deductions) */

  /* Surfaces */
  --xero-panel-bg:    #F2F1EE;  /* Warm Grey — calculator CARD bg + generator download strip */
  --xero-bg:          #FFFFFF;
  --xero-input-bg:    #FFFFFF;

  /* Lines */
  --xero-input-border:      #000856;  /* input stroke ON WHITE bg */
  --xero-input-border-grey: #CBCAC9;  /* input stroke ON WARM-GREY card */
  --xero-border:            #C2CDD6;  /* dividers + table rules ONLY — never an input stroke */

  /* Text */
  --xero-text-muted:  #4A6070;  /* helper / secondary text */
  --xero-placeholder: #6E7481;  /* input placeholder text */

  /* Geometry + type */
  --radius:        6px;          /* every card, input, band, button, download strip */
  --font:          'National2', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-heading:  'National2Condensed', var(--font);  /* H1 / page title ONLY */
}
```

Banned values (search-and-destroy): `#1AB394`, `#1B3A4B`, `#213B55`, `#EBF0F4`, `#ECF2F6`,
raw greys `#f8fafb`/`#f5f8fa`/`#f0f5f8`/`#f7f9fb`/`#F5F7FA`/`#F5F7F9`, `DM Sans`, `Plus Jakarta Sans`, `GT Walsheim` (except the one handoff comment noting it's the production font).

---

## 2. Document scaffold — every file is a valid standalone page AND an embeddable section

```html
<!--
  XERO PROTOTYPE
  Tool: …   Locale: …   Built: …   Pattern: …
  Notes for devs: National2 / National2Condensed are stand-ins loaded from xero.com CDN +
  inline base64; GT Walsheim is the licensed production font.
-->
<!DOCTYPE html>
<html lang="en-AU">            <!-- en-AU | en-NZ | en-GB | en-US -->
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Xero's free … – AU</title>
  <style> /* @font-face blocks + all component CSS */ </style>
</head>
<body>
  <!-- ONE wrapper section, never styling <html>/<body> so it lifts cleanly into AEM -->
  <section class="xero-hero"> … </section>
  <script> … </script>
</body>
</html>
```

**Mandatory in every file:** `<!DOCTYPE html>`, `<html lang>`, `<meta charset>`, `<meta viewport>`,
exactly one `<h1>`. Without charset → em-dash mojibake locally; without viewport → mobile ignores all breakpoints.

### `@font-face` (identical in every file)
National2 (400/500/700) from `https://www.xero.com/fonts/…woff2`; National2Condensed (700/800) inline base64.

---

## 3. Typography
- **Exactly one `<h1>`** per file (page/tool title) → `font-family: var(--font-heading); font-weight: 800;`
- Everything else (`h2`, card titles, section labels, body, table, results) → `var(--font)`. Never put `--font-heading` on a sub-heading.
- All text colour `#000856` unless muted (`--xero-text-muted`) or a semantic value (green/red).

---

## 4. Layout patterns

| Pattern | Used by | Inner max-width | Grid | Stack at |
|---|---|---|---|---|
| **B — Hero** | most calculators | `1200px` | `1fr 480px` | 900px |
| **B-modified (table)** | depreciation, timesheet-calc | `1200px` | `480px 1fr` (timesheet `400px 1fr`) | 900px |
| **A — Body** | income-tax AU/NZ | `960px` | `1fr` / 60-40 | 768px |
| **Generator** | all generators | `1200px` | `1fr 400px` (form L, sticky preview R) | 1000px |
| **Data viz full** | xsbi full/standalone | `960px` | — | 768px |
| **Data viz half** | xsbi `-half` | `480px` card | — | n/a |

**Background rule (important):** the outer hero band / page wrapper is **WHITE** (`--xero-bg`) and full-width; the inner is centred at the width above. Only the **calculator card / form panel** is Warm Grey (`--xero-panel-bg`) so it stands out against the white page. Never make the whole page Warm Grey. For table calculators (depreciation, timesheet-calc) the page wrapper is white, the input/form card is Warm Grey, and the schedule/result table is white.

---

## 5. Shared components (canonical CSS)

### Card (calculator)
```css
.xero-hero__card { background: var(--xero-panel-bg); border-radius: var(--radius); /* 6px */ padding: 32px; box-shadow: 0 4px 24px rgba(0,8,86,.08); }
```
Calculator card = **Warm Grey**. Generators use a **white** form area (many inputs — warm grey not required).

### Input field — context-aware border (CRITICAL)
```css
.xero-input-wrap {
  display: flex; align-items: center;
  background: var(--xero-input-bg);
  border: 1px solid var(--xero-input-border);   /* #000856 on WHITE */
  border-radius: var(--radius);
}
.xero-card--warm .xero-input-wrap,                /* on a warm-grey card … */
.xero-hero__card .xero-input-wrap { border-color: var(--xero-input-border-grey); } /* … use #CBCAC9 */
.xero-input-wrap:hover { box-shadow: 0 2px 8px rgba(0,8,86,.12); }   /* drop-shadow on hover */
.xero-input-wrap:focus-within { border-color: var(--xero-blue); }
.xero-input { border: none; outline: none; background: transparent; font: inherit; width: 100%; }
.xero-input::placeholder { color: var(--xero-placeholder); }
```
**Rule:** input stroke is `#000856` on white, `#CBCAC9` on warm-grey. `--xero-border` (#C2CDD6) is **never** an input stroke. Width **1px** (no 1.5px). Adornments: `.xero-input-wrap__prefix` / `__suffix`.

### Button
```css
.xero-btn-primary { background: var(--xero-navy); color:#fff; border:none; border-radius:var(--radius); padding:12px 24px; font-weight:700; cursor:pointer; }
.xero-btn-primary:hover { background:#000640; }
.xero-btn-secondary { background:none; border:none; color:var(--xero-navy); text-decoration:underline; text-decoration-color:var(--xero-blue); }
```

### Radio / toggle (ONE implementation)
Hidden native input + `.xero-radio` row (1px `--xero-border`), `.xero-radio__box` circle (resting `--xero-input-border`), checked state via `:has(input:checked)` → border + `.xero-radio__dot` `--xero-blue`. Inline variant `.xero-radio-group--inline`.

### Result band (calculators — the "flip")
```css
.xero-result-band { background:#fff; border-radius:var(--radius); padding:20px; }   /* WHITE inside the warm-grey card */
.xero-result-row { display:flex; justify-content:space-between; padding:12px 0; border-bottom:1px solid var(--xero-border); }
.xero-result-row:last-child { border-bottom:none; }
.xero-result-row__value--primary  { color:var(--xero-green); font-weight:700; }
.xero-result-row__value--negative { color:var(--xero-red); }
@media (max-width:480px){ .xero-result-row{ flex-direction:column; gap:4px; } }
```
Model: **card = Warm Grey, result band = white.** Never card-white + band-white (no contrast).

### Data table (calc schedule + generator line items)
```css
.xero-table-scroll { overflow-x:auto; }                       /* mobile */
.xero-data-table { width:100%; border-collapse:collapse; min-width:480px; }
.xero-data-table thead th { background:var(--xero-navy); color:#fff; font-weight:700; padding:8px 10px; }
.xero-data-table td { border-bottom:1px solid var(--xero-border); padding:8px 10px; }
.xero-data-table tbody tr:nth-child(even) { background:var(--xero-panel-bg); }  /* warm-grey stripe, not raw grey */
.xero-data-table tfoot td { background:var(--xero-navy); color:#fff; font-weight:700; }
```
Inline cell inputs use `--xero-input-border` (#000856 — they sit on white).

### Generator download bar (ONE canonical)
```css
.xero-preview__dl { background:var(--xero-panel-bg); border:1px solid var(--xero-border); border-top:none; border-radius:0 0 var(--radius) var(--radius); padding:12px 16px; display:flex; align-items:center; gap:10px; flex-wrap:wrap; }
.xero-preview__dl-label { font-size:.75rem; font-weight:600; color:var(--xero-text-muted); }
.xero-dl-btn { display:inline-flex; align-items:center; gap:6px; padding:6px 10px; font:inherit; font-size:.8125rem; font-weight:600; color:var(--xero-navy); background:#fff; border:1px solid var(--xero-border); border-radius:var(--radius); cursor:pointer; }
.xero-dl-btn:hover { border-color:var(--xero-navy); box-shadow:0 2px 6px rgba(0,8,86,.1); }
```
Label "Download as:". Buttons in order **PDF · Word · Excel · CSV · HTML**. "Excel" must emit a real `.xls`.
The strip is a **standalone box** (full `1px solid var(--xero-border)`, full `border-radius:var(--radius)`) — it is **not** fused to the preview, because the CTA sits between them (see order below).

### Preview document (generators)
`.xero-preview__doc` + children `.xero-preview__doc-*`. Replaces all `inv-doc*`, `ps-*`, `exp-*`, `xpog inv-doc-*`.

### CTA + disclaimer

**Generator order (CTA ABOVE the download bar — users see the Xero pitch before downloading):**
```
.xero-preview__doc      ← the live document (own rounded box)
.xero-cta               ← free-trial CTA
.xero-preview__dl       ← "Download as:" strip (standalone box)
.xero-disclaimer        ← footnote (always last)
```

**Calculator order:** result band → `.xero-cta` → `.xero-disclaimer`.

```html
<p class="xero-cta">…with Xero. Start a 30 day <a href="LOCALE_SIGNUP_URL" target="_blank">free trial</a>.</p>
<p class="xero-disclaimer">*This [calculator/tool] provides estimates only and is not financial or accounting advice.</p>
```
- CALC copy: *"Get a real time view of your [X] with Xero. Start a 30 day free trial."*
- **Locale CTA offer wording:**
  - AU / NZ / UK → *"Start a 30 day **free trial**."* (link on "free trial")
  - **US → *"**Get one month free**."*** (link on the whole phrase) — US does not say "30 day free trial".
- `LOCALE_SIGNUP_URL`: `https://www.xero.com/au/signup/` · `/nz/` · `/uk/` · `/us/`. Never bare `xero.com/signup/`.
- One CTA + one disclaimer per file (no duplicates).

---

## 6. Data viz specifics
- Chart.js line; COVID highlight band `#FFC4D2` at **0.5 opacity** via plugin (every chart, incl. compact).
- Stat-selector cards flip grey→white on select. Range pills **1Y / 3Y / 5Y / All** (all four).
- Delta pills: one contract — `.xero-delta.pos` (Pine) / `.xero-delta.neg` (red). Positive fill gradients use Pine `rgba(24,98,65,…)`, never teal.
- `xsbi-` prefix is allowed for chart-internal classes, but shared chrome (cards, pills, wrapper) uses `xero-`.

---

## 7. Mobile
- Viewport meta mandatory (see §2).
- Stack breakpoints per §4. Generators: preview column drops **below** the form (no `order:-1`); `position:sticky` → `static` when stacked.
- No orphan media queries targeting non-existent selectors (e.g. `.xero-hero__inner` in a generator).

---

## 8. Per-family constraints the spec must preserve (don't flatten)
- **business-loan:** loan-term row (number + Years/Months select), repayment-frequency select, $/% adornments, multi-row result band.
- **depreciation / timesheet-calc:** live schedule/time table (B-modified layout), method/period toggle, CSV as a *secondary* action (not the 5-button bar).
- **income-tax:** locale tax engines, per-period table, employment toggle (AU/NZ Pattern A, UK Pattern B).
- **gst/vat:** add/remove mode radio, rate selector, dynamic row relabel.
- **markup:** dual-mode → two result bands (one hidden).
- **sales-tax:** address → state detection, RATES table, detected-state pill, prototype-notice box.
- **generators:** `lineItems[]` editable table, editable tax-rate (locale default + GST/VAT/Sales Tax label), logo upload, live preview, 5-format export.
- **payslip/pay-stub:** earnings + deductions tables, YTD (pay-stub), statutory auto-deductions, sensitive-field masking.
- **roi:** standalone non-AEM page — range sliders, sticky sidebar, print-only PDF report. Lowest convergence priority.
