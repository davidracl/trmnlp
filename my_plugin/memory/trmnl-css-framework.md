# TRMNL CSS Framework Reference

## Required Includes
```html
<link rel="stylesheet" href="https://trmnl.com/css/latest/plugins.css">
<script src="https://trmnl.com/js/latest/plugins.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;350;375;400;450;600;700&display=swap" rel="stylesheet">
```

---

## Screen
- `screen` - Device container
- `screen--sm`, `screen--md`, `screen--lg` - Size classes (auto-applied)
- `screen--1bit`, `screen--2bit`, `screen--4bit` - Bit-depth classes (auto-applied)
- `screen--portrait` - Swaps to portrait dimensions
- `screen--no-bleed` - Removes default padding
- `screen--dark-mode` - Inverts colors, preserves images
- `screen--backdrop` - Patterned bg for mashups (1-bit: pattern, 2-4bit: gray)
- Device: `screen--og`, `screen--v2`, `screen--amazon_kindle_2024`, etc.

### CSS Variables
| Variable | Purpose | Default |
|---|---|---|
| `--screen-w` | Container width | 800px |
| `--screen-h` | Container height | 480px |
| `--full-w` | Width minus padding | `calc(--screen-w - --gap * 2)` |
| `--full-h` | Height minus padding | `calc(--screen-h - --gap * 2)` |
| `--ui-scale` | Scaling factor | 1 |
| `--gap-scale` | Gap scaling | 1 |
| `--color-depth` | Display bits | 1 |

---

## View
- `view` - Base content container
- `view--full` - Entire screen
- `view--half_vertical` - Half width (side-by-side)
- `view--half_horizontal` - Half height (stacked)
- `view--quadrant` - Quarter screen

---

## Layout
- `layout` - Main content container (one per view)
- `layout--row` - Horizontal arrangement
- `layout--col` - Vertical arrangement
- `layout--left`, `layout--center-x`, `layout--right` - Horizontal alignment
- `layout--top`, `layout--center-y`, `layout--bottom` - Vertical alignment
- `layout--center` - Both axes centered
- `layout--stretch` - Children expand both directions
- `layout--stretch-x` - Horizontal expansion
- `layout--stretch-y` - Vertical expansion
- Child-level: `stretch-x`, `stretch-y`

---

## Mashup (Multi-View)
- `mashup` - Multi-view container
- `mashup--1Lx1R` - 2x `view--half_vertical`
- `mashup--1Tx1B` - 2x `view--half_horizontal`
- `mashup--1Lx2R` - 1x half_vertical + 2x quadrant
- `mashup--2Lx1R` - 2x quadrant + 1x half_vertical
- `mashup--2Tx1B` - 2x quadrant + 1x half_horizontal
- `mashup--1Tx2B` - 1x half_horizontal + 2x quadrant
- `mashup--2x2` - 4x `view--quadrant`

---

## Columns
- `columns` - Auto-distribution container
- `column` - Individual column
- Handles overflow with "and X more" indicator
- Preserves group headers across columns

---

## Flex
### Direction
- `flex` / `flex--row` - Horizontal
- `flex--col` - Vertical
- `flex--row-reverse`, `flex--col-reverse` - Reversed

### Alignment (Container)
- `flex--left`, `flex--center-x`, `flex--right` - Horizontal
- `flex--top`, `flex--center-y`, `flex--bottom` - Vertical

### Stretch (Container)
- `flex--stretch` - Both axes
- `flex--stretch-x`, `flex--stretch-y` - Single axis

### Stretch (Item)
- `stretch`, `stretch-x`, `stretch-y`
- `no-shrink` - Prevent compression

### Self Alignment (Item)
- `self--start`, `self--center`, `self--end`, `self--stretch`

### Growth/Shrink
- `grow` - Flex grow
- `shrink-0` - No shrink
- `flex-none` - No flex
- `flex-initial` - Initial flex

### Flex Basis
- `basis--{value}` (e.g. `basis--20`, `basis--24`, `basis--36`)

### Order
- `order--first`, `order--last`, `order--{number}` (e.g. `order--2`, `order---1`)

### Wrapping
- `flex--wrap`, `flex--nowrap`, `flex--wrap-reverse`

### Distribution
- `flex--between` - space-between
- `flex--around` - space-around
- `flex--evenly` - space-evenly

### Multi-line Spacing (align-content)
- `flex--content-start`, `flex--content-center`, `flex--content-end`
- `flex--content-between`, `flex--content-around`, `flex--content-evenly`, `flex--content-stretch`

### Display Type
- `inline-flex`

### Responsive
- `portrait:flex--col`, `md:flex--row`, `md:portrait:flex--col`

---

## Grid
- `grid` - Grid container
- `grid--cols-{N}` - Column count (e.g. `grid--cols-3`, `grid--cols-4`)
- `grid--wrap` - Enable responsive wrapping
- `grid--min-{size}` - Min track size (e.g. `grid--min-32`, `grid--min-56`)
- `col` - Column layout
- `col--span-{N}` - Span columns (e.g. `col--span-1`, `col--span-2`, `col--span-3`, `col--span-6`)
- `col--start`, `col--center`, `col--end` - Column alignment
- `row` - Row layout
- `row--start`, `row--center`, `row--end` - Row alignment

---

## Gap / Spacing
### Predefined
- `gap--none` - 0
- `gap--xsmall` - Extra small
- `gap--small` - Small
- `gap` / `gap--base` - Default
- `gap--medium` - Medium
- `gap--large` - Large
- `gap--xlarge` - Extra large
- `gap--xxlarge` - Double extra large

### Distribution
- `gap--auto` - `justify-content: space-evenly`
- `gap--distribute` - `justify-content: space-between`
- `gap--space-between` - Legacy (behaves like gap--auto)

### Arbitrary
- `gap--[Npx]` - 0-50px (NO responsive support)

### Responsive
- `md:gap--large`, `lg:gap--xlarge`, `portrait:gap--medium`, `md:portrait:gap--xlarge`

---

## Spacing (Margin & Padding)
### Margin
- `m--{size}` - All sides
- `mt--{size}`, `mr--{size}`, `mb--{size}`, `ml--{size}` - Single side
- `mx--{size}`, `my--{size}` - Horizontal/vertical axis

### Padding
- `p--{size}` - All sides
- `pt--{size}`, `pr--{size}`, `pb--{size}`, `pl--{size}` - Single side
- `px--{size}`, `py--{size}` - Horizontal/vertical axis

### Responsive
- `md:my--{size}`, `portrait:mx--{size}`, `lg:portrait:mt--{size}`

Uses same size scale as Size utility.

---

## Size
### Fixed Sizes (w/h)
Values: `0, 0.5, 1, 1.5, 2, 2.5, 3, 3.5, 4, 5, 6, 7, 8, 9, 10, 11, 12, 14, 16, 20, 24, 28, 32, 36, 40, 44, 48, 52, 56, 60, 64, 72, 80, 96`

Pixel equivalents: `0, 2, 4, 6, 8, 10, 12, 14, 16, 20, 24, 28, 32, 36, 40, 44, 48, 56, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 384px`

Usage: `w--{size}`, `h--{size}` (e.g. `w--16`, `h--24`)

### Dynamic
- `w--full`, `h--full` - 100%
- `w--auto`, `h--auto` - Auto

### Arbitrary
- `w--[Npx]`, `h--[Npx]` - N = 0-800 (NO responsive support for arbitrary)

### Container Query
- `w--[Ncqw]`, `h--[Ncqh]` - N = 0-100 (% of `.layout` container)

### Min/Max
- Fixed: `w--min-{size}`, `w--max-{size}`, `h--min-{size}`, `h--max-{size}`
- Arbitrary: `w--min-[Npx]`, `w--max-[Npx]`, `h--min-[Npx]`, `h--max-[Npx]`
- Dynamic: `w--min-full`, `w--max-full`, `h--min-auto`, `h--max-auto`
- Container: `w--min-[Ncqw]`, `w--max-[Ncqw]`, `h--min-[Ncqh]`, `h--max-[Ncqh]`

### Responsive
- `md:w--16`, `lg:h--24`, `md:portrait:w--16`

---

## Background
- `bg--black` - Pure black
- `bg--gray-10` through `bg--gray-75` (5-point increments: 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75)
- `bg--white` - Pure white
- Deprecated: `bg--gray-1` through `bg--gray-7` (still functional)
- Uses dither patterns for grayscale on 1-bit displays

---

## Border (v2)
- `border--h-1` through `border--h-7` - Horizontal (1=black, 7=white)
- `border--v-1` through `border--v-7` - Vertical
- Uses dither patterns for grayscale illusion

---

## Rounded
### Base Sizes
- `rounded--none` (0px)
- `rounded--xsmall` (5px)
- `rounded--small` (7px)
- `rounded` / `rounded--base` (10px)
- `rounded--medium` (15px)
- `rounded--large` (20px)
- `rounded--xlarge` (25px)
- `rounded--xxlarge` (30px)
- `rounded--full` (9999px, pill)

### Arbitrary
- `rounded--[Npx]` - 0 to 50px

### Individual Corners
- `rounded-tl--{size}`, `rounded-tr--{size}`, `rounded-br--{size}`, `rounded-bl--{size}`

### Sides
- `rounded-t--{size}`, `rounded-r--{size}`, `rounded-b--{size}`, `rounded-l--{size}`

### Responsive
- `md:rounded--xlarge`, `portrait:rounded--small`

---

## Outline
- `.outline` - Pixel-perfect rounded border
- 1-bit: border-image with dithered 9-slice images
- 2-bit/4-bit: `border: 1px solid var(--black); border-radius: 10px`

---

## Text
### Color
- `text--black`, `text--white`
- `text--gray-10` through `text--gray-75` (5-point increments)
- Deprecated: `text--gray-1` through `text--gray-7`

### Alignment
- `text--left` (default), `text--center`, `text--right`, `text--justify`

### Responsive
- All support: `sm:`, `md:`, `lg:`, `portrait:`, `1bit:`, `2bit:`, `4bit:`
- Combined: `md:portrait:text--right`, `2bit:text--right`

---

## Image
- `image` - Base class
- `image-dither` - Grayscale dithering on 1-bit displays
- `image--fill` - Stretch/squish to fill
- `image--contain` - Fit within, maintain ratio
- `image--cover` - Fill and clip to fit

---

## Image Stroke
- `image-stroke` - Default 1.5px white stroke
- `image-stroke--small` (1px), `image-stroke--base` (1.5px), `image-stroke--medium` (2px), `image-stroke--large` (2.5px), `image-stroke--xlarge` (3px)
- `image-stroke--black` - Black stroke color

---

## Text Stroke
- `text-stroke` - Default 3.5px white
- `text-stroke--small` (2px), `text-stroke--base` (3.5px), `text-stroke--medium` (4.5px), `text-stroke--large` (6px), `text-stroke--xlarge` (7.5px)
- `text-stroke--{shade}` - Shade matching bg grayscale (black, gray-10 through gray-75, white)
- Only works with pure black or pure white text

---

## Visibility
| Class | CSS |
|---|---|
| `hidden` | `display: none` |
| `visible` / `block` | `display: block` |
| `inline` | `display: inline` |
| `inline-block` | `display: inline-block` |
| `flex` | `display: flex` |
| `grid` | `display: grid` |
| `table` | `display: table` |
| `table-row` | `display: table-row` |

Responsive: `sm:`, `md:`, `lg:`, `1bit:`, `2bit:`, `4bit:`. Combined: `md:1bit:block`, `lg:4bit:grid`

---

## Scale (4-bit only, applied to `screen`)
- `screen--scale-xsmall` - 0.75 (75%)
- `screen--scale-small` - 0.875 (87.5%)
- `screen--scale-regular` - 1.0 (100%)
- `screen--scale-large` - 1.125 (112.5%)
- `screen--scale-xlarge` - 1.25 (125%)
- `screen--scale-xxlarge` - 1.5 (150%)

---

## Aspect Ratio
- `aspect--auto` - No constraints
- `aspect--1/1` - Square
- Landscape: `aspect--4/3`, `aspect--3/2`, `aspect--16/9`, `aspect--21/9`
- Portrait: `aspect--3/4`, `aspect--2/3`, `aspect--9/16`, `aspect--9/21`

---

## Responsive Prefix Pattern
Format: `size:orientation:bit-depth:utility`
- Size: `sm:`, `md:`, `lg:` (mobile-first, progressive)
- Orientation: `portrait:` (landscape default)
- Bit-depth: `1bit:`, `2bit:`, `4bit:` (non-progressive)
- Examples: `md:portrait:4bit:gap--large`, `lg:flex--row`, `1bit:hidden`
