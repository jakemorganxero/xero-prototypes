# Xero Calculator & Template Prototyping — Brand Guidelines

## What these files are for
Use this as the reference doc whenever prototyping a new calculator or template tool for xero.com/au/calculators/ or xero.com/au/templates/. The goal is for prototypes to be close enough to the real site that devs can lift and adapt them with minimal rework.

---

## Colours

```css
/* Core palette — extracted from xero.com */
--xero-navy:       #1B3A4B;   /* Headings, primary button bg, result panel text */
--xero-blue:       #13B5EA;   /* Xero brand blue — links, hover accents */
--xero-blue-dark:  #0D8FBD;   /* Hover state for blue elements */
--xero-panel-bg:   #EBF0F4;   /* Results/sidebar panel background */
--xero-border:     #C2CDD6;   /* Input borders, dividers */
--xero-bg:         #FFFFFF;   /* Page / form background */
--xero-text:       #1B3A4B;   /* Body text (same as navy) */
--xero-text-muted: #4A6070;   /* Secondary/helper text */
--xero-input-bg:   #FFFFFF;
--xero-green:      #1AB394;   /* Positive cash flow / success states */
--xero-red:        #E05A5A;   /* Negative cash flow / error states */
```

---

## Typography

Xero.com uses **GT Walsheim** (licensed, loaded via their CDN). For prototypes, use **"DM Sans"** from Google Fonts as the closest freely available match — similar geometric warmth and weight range.

```html
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
```

```css
font-family: 'DM Sans', -apple-system, BlinkMacSystemFont, sans-serif;

/* Heading scale */
h1: 2rem / 700 / navy
h2 (section): 1.25rem / 700 / navy
label: 0.8125rem / 700 / navy  (uppercase-ish, tight tracking)
body: 0.9375rem / 400 / navy
helper text: 0.875rem / 400 / muted
```

---

## Layout — Three Patterns

There are three layout patterns. Specify which one you want when requesting a prototype.

---

### Pattern A — Standard (body section)

The default. Used for the existing cash flow calculator. Lives mid-page as a content section.

Xero calculators use a **two-column layout** on desktop, stacked on mobile:

```
┌─────────────────────────────┬───────────────────────┐
│  FORM (left, ~60%)          │  RESULTS PANEL (~40%) │
│                             │  light grey bg        │
│  Section heading            │  "Your X is:"         │
│  ──────────────────         │  Big result value     │
│  Label    [ input ]         │  ─────────────────    │
│  Label    [ input ]         │  Line item            │
│                             │  ─────────────────    │
│  Section heading            │  Line item            │
│  ──────────────────         │                       │
│  Label    [ input ]         │  CTA → free trial     │
│                             │                       │
│  [Calculate]  Reset         │                       │
└─────────────────────────────┴───────────────────────┘
```

- Form section headings: bold navy, `font-size: 1.1rem`, bottom border divider
- Input rows: 2-col grid inside each section (some sections have only 1 wide input)
- Inputs: full-width, border `1px solid --xero-border`, `border-radius: 4px`, `padding: 10px 14px`, `$` prefix via wrapper div
- Button: `background: --xero-navy`, white text, `border-radius: 4px`, `padding: 12px 24px`, bold
- Reset: plain text link, underlined, navy

---

### Pattern B — Hero (full-width, above the fold)

Inspired by the Wise VAT calculator pattern. The tool IS the hero — it sits at the top of the page with a headline and description on the left, compact calculator widget on the right. **Full viewport width** with a light background colour, no max-width constraint on the outer band.

```
┌──────────────────────────────────────────────────────────────┐
│  HERO BAND (full width, bg: --xero-panel-bg or white)        │
│  padding: 80px 5%                                            │
│                                                              │
│  ┌──────────────────────┐   ┌──────────────────────────────┐ │
│  │ H1: Tool name        │   │  CALCULATOR CARD             │ │
│  │                      │   │  white bg, box-shadow        │ │
│  │ Body copy explaining │   │                              │ │
│  │ what it does and     │   │  [ inputs — compact ]        │ │
│  │ who it's for.        │   │                              │ │
│  │                      │   │  ○ Option A                  │ │
│  │ Optional: bullet     │   │  ○ Option B                  │ │
│  │ points or trust      │   │                              │ │
│  │ signals              │   │  ┌────────────────────────┐  │ │
│  │                      │   │  │ Result: $0.00          │  │ │
│  │                      │   │  │ Net: $0.00             │  │ │
│  │                      │   │  └────────────────────────┘  │ │
│  └──────────────────────┘   └──────────────────────────────┘ │
└──────────────────────────────────────────────────────────────┘
```

Key differences from Pattern A:
- **No separate "Calculate" button** — results update live as user types (real-time calculation)
- **Calculator is a card** — white background, `border-radius: 8px`, `box-shadow: 0 4px 24px rgba(0,0,0,0.08)`
- **Compact inputs** — fewer fields, simpler formula. Not suitable for tools with 10+ inputs
- **Radio/toggle options** — use styled radio buttons for "include/exclude" type choices (see component below)
- **Result appears inside the card** in a shaded band at the bottom, not in a separate panel
- **H1 is large** — `2.5rem` to `3rem`, bold, navy. This is a page heading, not a section heading
- **Outer band background**: use `--xero-panel-bg` (`#EBF0F4`) for a subtle wash, or white if the page already has colour
- **Max-width on inner content**: `1200px` centred, but the band itself is full-width

CSS structure:
```css
.xero-hero {
  background: var(--xero-panel-bg);
  padding: 72px 5%;
}
.xero-hero__inner {
  max-width: 1200px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1fr 480px;
  gap: 64px;
  align-items: center;
}
.xero-hero__title {
  font-size: clamp(2rem, 4vw, 3rem);
  font-weight: 700;
  color: var(--xero-navy);
  margin-bottom: 20px;
}
.xero-hero__card {
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 4px 24px rgba(0,0,0,0.08);
  padding: 32px;
}
.xero-hero__result-band {
  background: var(--xero-panel-bg);
  border-radius: 4px;
  padding: 20px;
  margin-top: 24px;
}
@media (max-width: 900px) {
  .xero-hero__inner { grid-template-columns: 1fr; }
}
```

---

### Pattern C — Hero (half-width, beside page content)

Same as Pattern B but the tool only occupies roughly half the page width — the other half is used for marketing copy, an image, or other page content managed by AEM. The calculator card is narrower (`360px` max) and more compact.

Use this when:
- The page has existing hero content (image, illustration) that shouldn't be displaced
- The tool is supplementary to the page rather than the primary focus
- Requested as "half-width header calc"

```
┌──────────────────────────────────────────────────────────────┐
│  PAGE HERO (managed by AEM — not part of the prototype)      │
│  ┌──────────────────────────────┐  ┌──────────────────────┐  │
│  │  Heading + copy (AEM)        │  │  CALCULATOR CARD     │  │
│  │                              │  │  (prototype)         │  │
│  │                              │  │  narrower, ~360px    │  │
│  └──────────────────────────────┘  └──────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
```

For Pattern C prototypes: render just the **calculator card** at `max-width: 400px` with a note in the file saying "drop this card into the right column of the existing hero section". Don't prototype the full hero — that's AEM's job.

---

## Results Panel (Pattern A only)

```css
background: var(--xero-panel-bg);   /* #EBF0F4 */
padding: 32px;
border-radius: 4px;

.result-label    { font-size: 1rem; font-weight: 700; }
.result-value    { font-size: 2rem; font-weight: 700; }  /* "To be calculated" or the $ amount */
.result-positive { color: var(--xero-green); }
.result-negative { color: var(--xero-red); }
```

Each line item in the panel has:
- Bold label
- Value (dash `—` until calculated)
- Short helper text below in muted colour
- `border-bottom: 1px solid --xero-border` divider

---

## Component — Currency Input

```html
<div class="xero-input-wrap">
  <span class="xero-input-prefix">$</span>
  <input type="number" min="0" step="0.01" placeholder="0.00" class="xero-input">
</div>
```

```css
.xero-input-wrap {
  display: flex;
  align-items: center;
  border: 1px solid var(--xero-border);
  border-radius: 4px;
  background: var(--xero-input-bg);
  overflow: hidden;
}
.xero-input-prefix {
  padding: 10px 10px 10px 14px;
  color: var(--xero-text-muted);
  font-weight: 500;
  user-select: none;
}
.xero-input {
  border: none;
  outline: none;
  padding: 10px 14px 10px 4px;
  font-family: inherit;
  font-size: 0.9375rem;
  width: 100%;
  background: transparent;
}
.xero-input:focus-within { border-color: var(--xero-blue); }
```

---

## Component — Styled Radio Buttons (Pattern B hero calc)

Used for include/exclude type choices in hero calculators. Each option is a full-width bordered row.

```html
<div class="xero-radio-group">
  <label class="xero-radio">
    <input type="radio" name="gst-mode" value="include" checked>
    <span class="xero-radio__box">
      <span class="xero-radio__dot"></span>
    </span>
    <span class="xero-radio__label">Add GST to amount</span>
  </label>
  <label class="xero-radio">
    <input type="radio" name="gst-mode" value="exclude">
    <span class="xero-radio__box">
      <span class="xero-radio__dot"></span>
    </span>
    <span class="xero-radio__label">Subtract GST from amount</span>
  </label>
</div>
```

```css
.xero-radio-group { display: flex; flex-direction: column; gap: 10px; }

.xero-radio {
  display: flex;
  align-items: center;
  gap: 12px;
  border: 1px solid var(--xero-border);
  border-radius: var(--radius);
  padding: 12px 16px;
  cursor: pointer;
  transition: border-color 0.15s;
}
.xero-radio:has(input:checked) { border-color: var(--xero-navy); }

.xero-radio input { position: absolute; opacity: 0; width: 0; height: 0; }

.xero-radio__box {
  width: 18px; height: 18px;
  border: 2px solid var(--xero-border);
  border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0;
  transition: border-color 0.15s;
}
.xero-radio:has(input:checked) .xero-radio__box { border-color: var(--xero-navy); }

.xero-radio__dot {
  width: 8px; height: 8px;
  border-radius: 50%;
  background: var(--xero-navy);
  opacity: 0;
  transition: opacity 0.15s;
}
.xero-radio:has(input:checked) .xero-radio__dot { opacity: 1; }

.xero-radio__label { font-size: 0.9375rem; font-weight: 500; color: var(--xero-navy); }
```

```html
<div class="xero-section">
  <h3 class="xero-section-title">What is your projected monthly money in?</h3>
  <div class="xero-fields-grid">
    <!-- inputs -->
  </div>
</div>
```

```css
.xero-section {
  padding-bottom: 24px;
  border-bottom: 1px solid var(--xero-border);
  margin-bottom: 24px;
}
.xero-section-title {
  color: var(--xero-navy);
  font-size: 1.1rem;
  font-weight: 700;
  margin-bottom: 16px;
}
.xero-fields-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
}
/* Single-wide fields */
.xero-fields-grid .full-width { grid-column: 1 / -1; }
```

---

## Tech stack notes

From inspecting xero.com:
- The **marketing site** (xero.com) is an AEM (Adobe Experience Manager) build — server-rendered HTML with vanilla JS for interactive components
- The cash flow calculator appears to be a **standalone vanilla JS widget** embedded as a section inside an AEM page template
- **For prototypes**: produce **self-contained HTML files** (single file, vanilla JS, no build step). This is the easiest for:
  - Previewing locally (just open in browser)
  - Handing to devs (they can see exactly what's needed and adapt to AEM/their stack)
  - Embedding via iframe if needed (Outgrow replacement)
- If devs want a React version later, the HTML prototype is still a clear spec

---

## Prototype file conventions

When Claude Code creates a new prototype:

1. **Filename**: `xero-[tool-name]-calculator.html` or `xero-[tool-name]-generator.html`
2. **Self-contained**: All CSS and JS inline in the single HTML file
3. **No external dependencies** except: Google Fonts + (optionally) a charting lib like Chart.js from cdnjs
4. **Section-component mindset**: The calculator/tool div should work standalone at any width (not assume full-page). Use a max-width container (`max-width: 960px`) so it embeds naturally as a page section.
5. **Mobile responsive**: Stack to single column below 768px
6. **Accessibility basics**: labels linked to inputs, sufficient colour contrast, keyboard navigable
7. **Trailing CTA**: Results panel should end with the Xero free trial CTA matching the live site

---

## Tone / copy style

- Section questions phrased as: *"What is your [X]?"*
- Helper text is concise, plain English: *"Cash flow is the net amount of money being transferred into and out of your business."*
- Button: *"Calculate your [tool name]"* / Reset link: *"Reset"*
- Disclaimer footnote: `*This calculator provides estimates only and is not financial advice.`

### Pattern B hero copy pattern (confirmed against xero.com)

Use this structure for the left-column copy in Pattern B hero calculators:

- **H1:** *"Xero's free [tool name]"* — possessive, no subtitle needed
- **Body:** Action-led, 2–3 sentences. Lead with the outcome ("Nail your numbers every time…"), explain what it does, end with a trust signal ("no downloads required").
- **Bullets:** 3 tick-mark bullets (✓), plain navy, no icon circles. Phrase as actions: *"Calculate the GST to add to your prices"* not *"Add GST to a price"*

Example (NZ GST):
```
H1:   Xero's free GST calculator
Body: Nail your numbers every time with Xero's goods and services tax (GST)
      calculator. Get fast, accurate GST figures for your invoices and
      expenses – no downloads required.
✓ Apply the right GST rate instantly
✓ Calculate the GST to add to your prices
✓ Calculate the GST to claim on your expenses
```

---

## How to request a new prototype

Tell Claude Code:

> **"Build a Xero [tool name] calculator/generator prototype"**
> 
> Include:
> - What inputs are needed
> - What outputs/results to show
> - Any locale (AU/NZ/US affects currency symbol and tax terminology)
> - Any specific sections or groupings

Claude Code will produce a single `.html` file matching the Xero calculator brand system above, ready to preview in browser and hand off to devs.
