# TRMNL Components & Elements Reference

---

## Title Bar
```html
<div class="title_bar">
  <img class="image" src="/images/plugins/trmnl--render.svg">
  <span class="title">Title Text</span>
  <span class="instance">Production</span>  <!-- optional -->
</div>
```
- **MUST be SIBLING of `layout`** inside a `view` (never nested inside layout)
- In mashups (half/quadrant views): auto-compact styling, reduced height, smaller icon, rounded bottom corners only

---

## Columns
```html
<div class="columns">
  <div class="column">...</div>
  <div class="column">...</div>
</div>
```
- Auto-distributes content across columns
- Manages overflow with "and X more" indicator
- Preserves group headers across columns
- Must be inside `layout`

---

## Rich Text
```html
<div class="richtext richtext--center gap--large">
  <span class="title" data-pixel-perfect="true">Article Title</span>
  <div class="content content--large" data-content-limiter="true">
    <p>Text content here</p>
  </div>
</div>
```
### Container Alignment
- `richtext--left`, `richtext--center`, `richtext--right`

### Content Alignment
- `content--left`, `content--center`, `content--right`

### Content Sizes
- `content--small`
- `content` / `content--base`
- `content--large`
- `content--xlarge`
- `content--xxlarge`
- `content--xxxlarge`

### Responsive
- `sm:`, `md:`, `lg:`, `portrait:`, `1bit:`, `2bit:`, `4bit:`
- Combined: `md:portrait:2bit:content--xlarge`

### Modulations
- `data-content-limiter="true"` - Auto-resize text to fit
- `data-content-max-height="[pixels]"` - Custom max height
- `data-pixel-perfect="true"` - Crisp text on e-ink

---

## Item
```html
<div class="item item--emphasis-2">
  <div class="meta"><span class="index">1</span></div>
  <div class="icon">
    <img src="icon.svg" class="w--[6cqw] h--[6cqw] portrait:w--[10cqw] portrait:h--[10cqw]" />
  </div>
  <div class="content">
    <span class="title title--small">Title</span>
    <span class="description">Description text</span>
    <div class="flex gap--small">
      <span class="label label--small">Label</span>
      <span class="value value--small">Value</span>
    </div>
  </div>
</div>
```
### Emphasis
- `item--emphasis-1` (default) - Standard
- `item--emphasis-2` - Medium
- `item--emphasis-3` - High

### Sections
- `.meta` - Left side metadata (index numbers)
- `.icon` - Icon container
- `.content` - Main content area
- `.index` - Index number within meta

### Deprecated
- `.list` class (use column/flex/grid instead)

---

## Table
```html
<table class="table table--small" data-table-limit="true">
  <thead>
    <tr><th><span class="title">Header</span></th></tr>
  </thead>
  <tbody>
    <tr><td><span class="label">Content</span></td></tr>
  </tbody>
</table>
```
### Sizes
- `table--xsmall` - Most compact
- `table--small` - Compact
- `table--condensed` - Alias for small
- `table` / `table--base` - Default
- `table--large` - Spacious

### Modifiers
- `table--indexed` - Adds index column with meta block

### Modulations
- `data-table-limit="true"` - Overflow engine with "and X more" row
- `data-table-max-height="240"` or `"auto"` - Max height
- `data-clamp="1"` - Single-line text truncation per cell

---

## Chart
Uses **Highcharts** (v12.3.0) and **Chartkick** (v5.0.1).

### Script Sources
```html
<script src="https://code.highcharts.com/highcharts.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chartkick@5.0.1/dist/chartkick.min.js"></script>
```

### CRITICAL Settings
```javascript
plotOptions: {
  series: {
    animation: false,
    enableMouseTracking: false,
    states: { hover: { enabled: false } },
    marker: { enabled: false }
  }
},
tooltip: { enabled: false },
legend: { enabled: false },
credits: { enabled: false }
```

### Chart Types
- Line, Multi-Series Line, Bar/Column, Gauge, Area

### Colors
- Primary: `#000000` (black)
- Pattern fills for multi-series: `gray-2.png`, `gray-3.png`, `gray-5.png`, `gray-7.png`
- Pattern URL: `https://trmnl.com/images/grayscale/{file}` (12x12px tiles)

### Grid Styling
- Y-axis: `shortdot` dash style
- X-axis: `dot` dash style
- Font: 16px for labels

### Gauge
- Arc: -150 to +150 degrees
- Uses plotBands for fill/unfill regions

### Chartkick Loading Pattern
```javascript
if ("Chartkick" in window) {
  createChart();
} else {
  window.addEventListener("chartkick:load", createChart, true);
}
```

---

## Progress Bar
```html
<div class="progress-bar progress-bar--large progress-bar--emphasis-2">
  <div class="content">
    <span class="label">Label</span>
    <span class="value value--xxsmall">75%</span>
  </div>
  <div class="track">
    <div class="fill" style="width: 75%"></div>
  </div>
</div>
```
### Sizes
- `progress-bar--xsmall`, `progress-bar--small`, `progress-bar--base`, `progress-bar` (default), `progress-bar--large`

### Emphasis
- `progress-bar--emphasis-2`, `progress-bar--emphasis-3`

---

## Progress Dots
```html
<div class="progress-dots progress-dots--large">
  <div class="track">
    <div class="dot dot--filled"></div>
    <div class="dot dot--current"></div>
    <div class="dot"></div>
  </div>
</div>
```
### Sizes
- `progress-dots--xsmall`, `progress-dots--small`, `progress-dots--base`, `progress-dots` (default), `progress-dots--large`

### Dot States
- `.dot` - Empty/unfilled
- `.dot--filled` - Completed
- `.dot--current` - Active

---

## Value Element
```html
<span class="value value--large value--tnums">48,206.62</span>
```
### Sizes (12 total)
`value--xxsmall`, `value--xsmall`, `value--small`, `value` / `value--base`, `value--large`, `value--xlarge`, `value--xxlarge`, `value--xxxlarge`, `value--mega`, `value--giga`, `value--tera`, `value--peta`

### Modifiers
- `value--tnums` - Tabular numbers for aligned columns

### Responsive
- `sm:`, `md:`, `lg:`, `portrait:` (e.g. `md:value--large lg:value--xlarge`)

---

## Title Element
```html
<span class="title title--small">Title Text</span>
```
### Sizes
- `title--small`
- `title` / `title--base`
- `title--large`
- `title--xlarge`
- `title--xxlarge`

### Responsive
- `sm:`, `md:`, `lg:`, `portrait:`, combined modifiers

---

## Label Element
```html
<span class="label label--large label--outline">Label Text</span>
```
### Style Variants
- Default (no modifier)
- `label--outline` - Outlined
- `label--underline` - Underlined
- `label--gray` - Gray text (deprecated: `label--gray-out`)
- `label--inverted` - Inverted colors

### Sizes
- `label--small`, default, `label--medium`, `label--large`, `label--xlarge`, `label--xxlarge`

### Responsive
- `sm:`, `md:`, `lg:`, `portrait:`, `1bit:`, `2bit:`, `4bit:`
- Example: `1bit:label--inverted 2bit:label--outline 4bit:label--underline`

---

## Description Element
```html
<span class="description" data-clamp="2">Description text</span>
```
### Sizes
- `description` (default)
- `description--large`
- `description--xlarge`
- `description--xxlarge`

### Responsive
- `sm:`, `md:`, `lg:`, `portrait:`

---

## Divider Element
```html
<div class="divider"></div>
<div class="divider--v"></div>
```
- `divider` / `divider--h` - Horizontal
- `divider--v` - Vertical
- `divider--on-white`, `divider--on-light`, `divider--on-dark`, `divider--on-black` - Background-specific
- Auto-detects background shade when no manual modifier given

---

## Modulations (Runtime Engines)

### Overflow Engine
```html
<div class="columns" data-overflow="true" data-overflow-counter="true" data-overflow-max-cols="3">
  <div class="column">...</div>
</div>
```
| Attribute | Default | Purpose |
|---|---|---|
| `data-overflow="true"` | false | Enable on non-columns containers |
| `data-overflow-max-height="N"` | auto | Height budget (px or "auto") |
| `data-overflow-counter="true"` | false | Show "and N more" label |
| `data-overflow-max-cols="N"` | unset | Best-fit columns up to N |
| `data-overflow-cols="N"` | unset | Force exactly N columns |

Responsive suffixes: `-sm`, `-md`, `-lg`, `-portrait`, `-md-portrait`
Example: `data-overflow-max-cols-lg="4"`

Group headers: `data-group-header="true"` on elements with class `group-header`

### Clamp Engine
```html
<span class="description" data-clamp="2" data-clamp-md="4" data-clamp-portrait="1">Text</span>
```
- `data-clamp="N"` - Truncate to N lines
- Responsive: `data-clamp-sm`, `data-clamp-md`, `data-clamp-lg`, `data-clamp-portrait`, `data-clamp-md-portrait`
- Legacy classes: `clamp--none`, `clamp--1` through `clamp--50`

### Format Value
```html
<span class="value" data-value-format="true" data-value-locale="en-US">1234567</span>
```
- `data-value-format="true"` - Auto abbreviate (K, M, B suffixes)
- `data-value-locale="en-US"` (or `de-DE`, `fr-FR`, `en-GB`, `ja-JP`)
- Recognized currencies: `$`, `EUR`, `GBP`, `JPY`, `UAH`, `INR`, `ILS`, `KRW`, `VND`, `PHP`, `RUB`, `BTC`

### Fit Value
```html
<span class="value value--xxxlarge" data-value-fit="true">$1,000,000</span>
<span class="value value--xxxlarge" data-value-fit="true" data-value-fit-max-height="340">Long text</span>
```
- `data-value-fit="true"` - Auto-resize font to fit container
- `data-value-fit-max-height="[pixels]"` - Max height (required for text, optional for numbers)

### Content Limiter
- `data-content-limiter="true"` - Auto-adjusts text size for overflow
- `data-content-max-height="[pixels]"` - Custom max height
- On overflow: adds `content--small`, uses Clamp Engine, hides subsequent blocks

### Pixel Perfect
- `data-pixel-perfect="true"` - Align text to pixel grid for crisp e-ink rendering

### Framework Runtime
Execution order: Clamp -> Overflow -> Value Formatting -> Fit Value -> Grid Gaps -> Column Gaps -> Pixel-Perfect Fonts -> Content Limiter -> Index Widths

Additional controls:
- `data-adjust-grid-gaps="false"` - Disable grid gap tweaking
- `data-adjust-column-gaps="false"` - Disable column gap normalization
