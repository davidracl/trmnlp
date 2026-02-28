# TRMNL Knowledge Base Index

## Project Overview
TRMNL = e-ink dashboard devices. Plugins are HTML/Liquid templates rendered to PNG for display.
CLI tool: `trmnlp` (Ruby gem `trmnl_preview`). Local dev at localhost:4567.

## Hardware Quick Reference
| Device | Resolution | Bit-Depth | Grayscale |
|--------|-----------|-----------|-----------|
| TRMNL OG | 800x480 | 1-bit | 2 shades (dithered ~14) |
| TRMNL OG V2 | 800x480 | 2-bit | 4 shades (dithered ~12 more) |
| TRMNL X/V2 | 1872x1404 | 4-bit | 16 shades, 227 PPI |
| Kindle PW 6th | 1024x768 | 8-bit | 256 shades |

## Breakpoints (mobile-first)
- `sm:` >= 600px (Kindle)
- `md:` >= 800px (TRMNL OG)
- `lg:` >= 1024px (TRMNL V2)
- Bit-depth: `1bit:`, `2bit:`, `4bit:` (non-progressive, targets specific)
- Orientation: `portrait:` (landscape default)
- Combined: `size:orientation:bit-depth:utility`

## Key Design Rules
1. Always set `animation: false` on charts
2. Title bar is SIBLING of layout, not child
3. One `layout` per `view`
4. Use `image-dither` class for images on 1-bit displays
5. Dark mode inverts everything except images
6. No sub-pixel rendering - use `data-pixel-perfect="true"` for crisp text

## CSS/JS Resources
```
CSS: https://trmnl.com/css/latest/plugins.css
JS:  https://trmnl.com/js/latest/plugins.js
Font: https://fonts.googleapis.com/css2?family=Inter:wght@300;350;375;400;450;600;700&display=swap
```

## Knowledge Base Files
- [trmnl-css-framework.md](trmnl-css-framework.md) - All CSS utility classes
- [trmnl-components.md](trmnl-components.md) - Components and elements
- [trmnl-liquid.md](trmnl-liquid.md) - Liquid templating and filters
- [trmnl-examples.md](trmnl-examples.md) - Template patterns and examples
- [trmnl-hardware.md](trmnl-hardware.md) - Hardware specs and best practices
- [trmnl-dev-workflow.md](trmnl-dev-workflow.md) - Development workflow and CLI

## Template Skeleton
```html
<div class="view view--full">
  <div class="layout layout--col gap">
    <!-- Content here -->
  </div>
  <div class="title_bar">
    <img class="image" src="icon.svg">
    <span class="title">Plugin Name</span>
    <span class="instance">Instance</span>
  </div>
</div>
```

## View Types
- `view--full` - Entire screen (one plugin)
- `view--half_vertical` - Left or right half
- `view--half_horizontal` - Top or bottom half
- `view--quadrant` - Quarter screen

## Plugin Strategies
- **polling** - TRMNL fetches from your URL (JSON/RSS/XML/CSV)
- **webhook** - POST data to TRMNL (max 2KB, 12x/hour)
- **static** - Local data only

## Current Project: my_plugin
Weather dashboard using Netatmo + Tomorrow.io forecast data.
Config: `/Users/davidracl/Documents/GitHub/trmnlp/my_plugin/`

## User Preferences
- Language: Czech/French for UI labels
- Primary device target: TRMNL OG (800x480, 1-bit)
