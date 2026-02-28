# TRMNL Hardware Specifications & Best Practices

---

## Device Models

### TRMNL OG (Original)
- **Display**: 7.5" EPD, 800x480 pixels
- **Bit-depth**: 1-bit (black & white, 2 shades)
- **Dithering**: Floyd-Steinberg, simulates ~14 additional grays
- **MCU**: ESP32-C3
- **Battery**: 1800-2500 mAh
- **Housing**: Injection-molded ABS soft touch plastic
- **Orientation**: Landscape (default)
- **CSS class**: `screen--og`, `screen--md`, `screen--1bit`

### TRMNL OG V2
- **Display**: 7.5" EPD, 800x480 pixels
- **Bit-depth**: 2-bit (4 shades of gray)
- **Dithering**: ~12 additional simulated grays
- **CSS class**: `screen--og`, `screen--md`, `screen--2bit`

### TRMNL X / TRMNL V2
- **Display**: 10.3" HD E-Ink, 1872x1404 pixels, 227 PPI
- **Bit-depth**: 4-bit (16 shades of gray, no dithering needed)
- **MCU**: ESP32-S3 + ESP32-C5
- **RAM**: 16MB + 8MB
- **Battery**: 6000-12000 mAh (3-6 months typical)
- **WiFi**: 802.11 b/g/n/ac (2.4/5 GHz), WPA/WPA2
- **Charging**: USB-C
- **Dimensions**: 233x193x12mm, 365g
- **Colors**: Black, White, Transparent, Sage, Wood, Grey
- **Orientations**: Portrait + Landscape, wall-mount + kickstand
- **Price**: $219 USD
- **CSS class**: `screen--v2`, `screen--lg`, `screen--4bit`

### Amazon Kindle Paperwhite 6th Gen (BYOD)
- **Display**: 1024x768 pixels
- **Bit-depth**: 8-bit (256 shades)
- **Rotation**: 90-degree
- **CSS class**: `screen--amazon_kindle_2024`, `screen--4bit`

### Other BYOD Devices (30+ supported)
Kobo, Inkplate, Inky Impression, Waveshare, Boox, Nook, Palma, Seeed, Xteink

---

## Color Palettes

| Name | Bit-depth | Shades | Use Case |
|------|----------|--------|----------|
| Black & White | 1-bit | 2 | Max battery life, TRMNL OG |
| 4 Shades of Gray | 2-bit | 4 | TRMNL OG V2, first-party hardware |
| 16 Shades of Gray | 4-bit | 16 | TRMNL X/V2, Kindle |
| 256 Shades of Gray | 8-bit | 256 | Some BYOD screens |

---

## Breakpoints (Mobile-First, Progressive)

| Prefix | CSS Class | Min Width | Devices |
|--------|----------|-----------|---------|
| (none) | (base) | 0px | All devices |
| `sm:` | `screen--sm` | 600px | Kindle 2024 (718px) |
| `md:` | `screen--md` | 800px | TRMNL OG, OG V2 (800px) |
| `lg:` | `screen--lg` | 1024px | TRMNL V2 (1872px landscape) |

**Mobile-first**: Base styles apply to all, `sm:` overrides at 600px+, `md:` at 800px+, `lg:` at 1024px+.

---

## Bit-Depth Prefixes (Non-Progressive)

| Prefix | CSS Class | Colors | Devices |
|--------|----------|--------|---------|
| `1bit:` | `screen--1bit` | 2 shades | TRMNL OG |
| `2bit:` | `screen--2bit` | 4 shades | TRMNL OG V2 |
| `4bit:` | `screen--4bit` | 16 shades | TRMNL V2, Kindle |

**Non-progressive**: `2bit:` does NOT apply to 4-bit devices. Each targets its specific depth only.

---

## Orientation

| Prefix | Behavior |
|--------|----------|
| (none) | Landscape (default) |
| `portrait:` | Portrait mode only |

---

## Combined Prefix Pattern
`size:orientation:bit-depth:utility`

Examples:
- `md:portrait:4bit:gap--large`
- `lg:flex--row`
- `1bit:hidden`
- `md:portrait:flex--col`

---

## Dithering

### How It Works
- Floyd-Steinberg dithering algorithm
- Creates illusion of additional gray levels using dot patterns
- 1-bit: ~14 simulated grays from black/white dots
- 2-bit: ~12 additional simulated grays from 4 base shades

### Implications for Design
- Fine detail can be lost in dithering
- Use `image-dither` class for images on 1-bit displays
- Background utilities (`bg--gray-*`) use pre-built dither patterns
- Text remains sharp (not dithered)
- 4-bit displays don't need dithering (16 native shades)

---

## Widget Dimensions (for content planning)
- Full screen: ~780x460 usable (800x480 minus padding)
- Half vertical: ~390x460
- Half horizontal: ~780x230
- Quadrant: ~390x230 (community tip: 400x235)

---

## Battery & Refresh Trade-offs
- Lower refresh rate = longer battery life
- Default refresh: configurable per plugin (e.g. 15 min, 1440 min)
- Device sleeps between refreshes
- 1-bit rendering is fastest (least processing)
- Larger displays (TRMNL X) have bigger batteries

---

## Image Formats
- **BMP3** and **PNG** supported (firmware v1.5.2+)
- Floyd-Steinberg dithering applied to images
- `-strip` parameter removes metadata
- Use `image-dither` CSS class for proper rendering

---

## Dark Mode
- `screen--dark-mode` CSS class
- Inverts entire screen EXCEPT images
- Images remain unchanged
- Plugin setting: `dark_mode: 'yes'`

---

## Communication Architecture
1. Device wakes from sleep every N minutes
2. Sends GET request to TRMNL server: API key, MAC, firmware version, battery voltage, WiFi RSSI
3. Server returns JSON: `{ image_url, filename, update_firmware, refresh_rate }`
4. Device downloads and renders 1/2/4-bit PNG
5. Device returns to sleep
- **One-way**: Device pings server, server never initiates
- **No location or identity data** collected

---

## Best Practices for E-Ink Design

### Do
- Use high contrast (black on white)
- Keep text large and readable
- Use `data-pixel-perfect="true"` for crisp text
- Test on actual device bit-depth
- Use `image-dither` for photos/graphics
- Leverage `bg--gray-*` utilities for visual hierarchy
- Use `data-content-limiter` and `data-clamp` for overflow
- Design for landscape first (default orientation)

### Don't
- Don't use subtle gray differences on 1-bit (lost in dithering)
- Don't use animations (e-ink can't display them)
- Don't rely on real-time updates (refresh is periodic)
- Don't use too many fine details (dithering reduces clarity)
- Don't forget to disable chart animations (`animation: false`)
- Don't use color (all displays are grayscale)
- Don't over-pack content (readability matters on e-ink)

### Responsive Strategy
1. Start with base styles for smallest display
2. Add `sm:` overrides for medium screens
3. Add `md:` overrides for TRMNL OG size
4. Add `lg:` overrides for TRMNL V2 size
5. Use `1bit:`/`2bit:`/`4bit:` for bit-depth-specific styling
6. Use `portrait:` for orientation-specific adjustments
