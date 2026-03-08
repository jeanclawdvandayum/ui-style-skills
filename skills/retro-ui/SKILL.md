---
name: retro-ui
description: Retro VHS tape / industrial manufacturing label aesthetic for web interfaces — ultra-condensed bold sans-serif type, strict black + white + vermillion color system, diagonal stripe patterns, dot grids, dense modular grid layouts, registration marks, technical code labels, warning badges, oversized background typography, and bottom runner bars. Inspired by 1970s-80s industrial graphics and VHS tape packaging design by Kyle Anthony Miller.
---

# Retro UI — VHS Industrial Label Aesthetic

Build web interfaces that look like they belong on a 1980s VHS tape label for a robotics program or an industrial safety manual. Ultra-condensed bold type, strict 3-color system (black, white, vermillion), dense modular grid layouts, diagonal stripe patterns, dot grids, technical code labels, and an aggressive information density that says "this machine does not care about your feelings."

**Reference:** [@kyleanthony](https://x.com/kyleanthony) — VHS tape-inspired design work.

---

## Design Tokens

### Color Palette

```css
:root {
  /* Core triad — STRICT. No other colors. */
  --black:          #0A0A0A;   /* Primary — near-black (not pure #000) */
  --white:          #F5F5F0;   /* Off-white — slight warmth, like aged paper */
  --vermillion:     #E8432D;   /* THE accent color — red-orange, VHS label red */

  /* Extended (use sparingly) */
  --pure-black:     #000000;   /* For ultra-dense backgrounds only */
  --pure-white:     #FFFFFF;   /* For high-contrast text on black */
  --vermillion-dark:#C4321E;   /* Hover/pressed state */
  --gray-80:        #333333;   /* Dark gray panels */
  --gray-60:        #666666;   /* Muted technical text */
  --gray-30:        #B0B0A8;   /* Borders, subtle rules */
  --gray-10:        #E8E8E2;   /* Light panel backgrounds */
}
```

**RULES:**
- Maximum 3 colors per composition: black + white + vermillion. NEVER add blue, green, purple, or any other hue.
- Vermillion is the ONLY accent color. It should occupy 15-25% of visual weight. Too little = boring. Too much = fire truck.
- Use vermillion for: one major panel per section, warning labels, active states, accent bars.
- Grayscale for everything else. When in doubt, go darker.

### Typography

**Display: Bebas Neue** (Google Fonts — condensed, bold, industrial)
- All-caps oriented. Tight tracking. Born for VHS labels.

**Wide Display: Archivo Black** (Google Fonts — wider bold for emphasis variation)
- Use when headlines need width, not just height.

**Technical/Labels: IBM Plex Mono** (Google Fonts — warm industrial mono)
- All technical codes, labels, warnings, runner bars.

**Body: Space Grotesk** (Google Fonts — clean geometric sans)
- For the rare paragraph text. Keep it minimal.

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Archivo+Black&family=Bebas+Neue&family=IBM+Plex+Mono:wght@400;500;600&family=Space+Grotesk:wght@400;500;700&display=swap" rel="stylesheet">
```

```css
:root {
  --font-display:   'Bebas Neue', 'Impact', sans-serif;
  --font-wide:      'Archivo Black', 'Arial Black', sans-serif;
  --font-mono:      'IBM Plex Mono', 'Courier New', monospace;
  --font-body:      'Space Grotesk', system-ui, sans-serif;
}
```

### Type Scale

| Role | Font | Size | Weight | Letter-spacing | Transform | Notes |
|------|------|------|--------|----------------|-----------|-------|
| **Hero display** | `--font-display` | `clamp(4rem, 10vw, 8rem)` | 400* | `-0.02em` | `uppercase` | *Bebas Neue is already bold at 400 |
| **Section display** | `--font-display` | `clamp(2.5rem, 5vw, 4.5rem)` | 400 | `-0.01em` | `uppercase` | |
| **Wide headline** | `--font-wide` | `clamp(2rem, 4vw, 3.5rem)` | 400 | `-0.02em` | `uppercase` | Use for wider compositions |
| **Panel title** | `--font-display` | `clamp(1.5rem, 3vw, 2.5rem)` | 400 | `0em` | `uppercase` | |
| **Technical label** | `--font-mono` | `0.6875rem` | 500 | `0.08em` | `uppercase` | The VHS label text |
| **Code/version** | `--font-mono` | `0.875rem` | 600 | `0.05em` | `uppercase` | R9, S-MAX, TRACK CODE |
| **Warning text** | `--font-mono` | `0.75rem` | 600 | `0.06em` | `uppercase` | Bold, spaced, imperative |
| **Body text** | `--font-body` | `0.9375rem` | 400 | `normal` | `none` | Rare — keep minimal |
| **Small print** | `--font-body` | `0.75rem` | 400 | `0.02em` | `none` | Fine print, credits |
| **Oversized BG** | `--font-display` | `clamp(8rem, 20vw, 16rem)` | 400 | `-0.04em` | `uppercase` | Background watermark text |

**Critical rules:**
- Almost everything is **uppercase**. Lowercase is reserved for body paragraphs only.
- Display type uses TIGHT negative letter-spacing. Pack it in.
- Technical labels use WIDE positive letter-spacing. Space it out.
- Contrast: massive display type + tiny technical labels. No middle ground.
- Line height on display: 0.9–1.0. Headlines should feel stacked and dense.

### Spacing

```css
:root {
  --space-xs:  0.25rem;   /* 4px — tight internal padding */
  --space-sm:  0.5rem;    /* 8px */
  --space-md:  1rem;      /* 16px */
  --space-lg:  1.5rem;    /* 24px */
  --space-xl:  2rem;      /* 32px */
  --gap:       2px;       /* Grid gap — TIGHT */
  --panel-pad: 1.5rem;    /* Internal panel padding */
}
```

**RULES:**
- Grid gaps are TIGHT: 1-4px between panels. This is NOT a breathing design.
- Internal panel padding is modest: 1–2rem.
- Dense is the goal. If it feels like there's too much whitespace, there is.
- The only generous spacing is inside hero sections and between major sections.

### Borders & Corners

```css
/* ZERO border-radius. Industrial doesn't do curves. */
* { border-radius: 0 !important; }

/* Borders are either invisible (panel edges defined by color contrast)
   or thick and structural */
.border-thick { border: 3px solid var(--black); }
.border-thin  { border: 1px solid var(--gray-30); }
```

---

## Pattern Library

These CSS patterns are the visual DNA of the aesthetic. Use them as panel backgrounds, overlays, and decorative elements.

### Diagonal Stripes

```css
/* Vermillion + white stripes (45°) */
.stripe-vermillion {
  background: repeating-linear-gradient(
    45deg,
    var(--vermillion),
    var(--vermillion) 8px,
    var(--white) 8px,
    var(--white) 16px
  );
}

/* Black + white stripes */
.stripe-bw {
  background: repeating-linear-gradient(
    45deg,
    var(--black),
    var(--black) 6px,
    var(--white) 6px,
    var(--white) 12px
  );
}

/* Thin stripes (tighter pitch for smaller elements) */
.stripe-thin {
  background: repeating-linear-gradient(
    -45deg,
    transparent,
    transparent 3px,
    var(--vermillion) 3px,
    var(--vermillion) 5px
  );
}
```

### Horizontal Lines

```css
/* Dense horizontal rules — scan-line / VHS tracking feel */
.hlines {
  background: repeating-linear-gradient(
    0deg,
    transparent,
    transparent 2px,
    var(--black) 2px,
    var(--black) 3px
  );
}

/* Wide horizontal bars */
.hbars {
  background: repeating-linear-gradient(
    0deg,
    var(--black) 0px,
    var(--black) 4px,
    transparent 4px,
    transparent 8px
  );
}
```

### Dot Grid

```css
/* Halftone dot pattern — circles in a grid */
.dot-grid {
  background-image: radial-gradient(circle, var(--black) 2px, transparent 2px);
  background-size: 12px 12px;
}

/* Vermillion dots on white */
.dot-grid-vermillion {
  background-image: radial-gradient(circle, var(--vermillion) 3px, transparent 3px);
  background-size: 16px 16px;
}

/* Large sparse dots (like the Adaptive Manufacturing card) */
.dot-grid-large {
  background-image: radial-gradient(circle, currentColor 5px, transparent 5px);
  background-size: 24px 24px;
}
```

### Vertical Barcode Lines

```css
/* Barcode-like vertical lines */
.barcode {
  background: repeating-linear-gradient(
    90deg,
    var(--black) 0px,
    var(--black) 2px,
    transparent 2px,
    transparent 4px,
    var(--black) 4px,
    var(--black) 5px,
    transparent 5px,
    transparent 9px,
    var(--black) 9px,
    var(--black) 12px,
    transparent 12px,
    transparent 14px
  );
}
```

### Registration Marks

```css
/* Print registration crosshair — use as ::before/::after */
.reg-mark::before {
  content: '';
  position: absolute;
  width: 16px; height: 16px;
  border: 1px solid var(--black);
  border-radius: 50% !important; /* Exception: reg marks ARE circular */
  top: 8px; left: 8px;
}
.reg-mark::after {
  content: '+';
  position: absolute;
  font-family: var(--font-mono);
  font-size: 18px;
  line-height: 1;
  color: var(--black);
  top: 4px; left: 11px;
}
```

---

## Component Recipes

### Modular Grid Layout

The core layout pattern: dense grid of mixed-size panels.

```css
.vhs-grid {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: var(--gap);
  background: var(--black); /* Gap color = black */
}
.vhs-grid > * {
  padding: var(--panel-pad);
  position: relative;
  overflow: hidden;
}
/* Panel sizing helpers */
.span-3  { grid-column: span 3; }
.span-4  { grid-column: span 4; }
.span-5  { grid-column: span 5; }
.span-6  { grid-column: span 6; }
.span-7  { grid-column: span 7; }
.span-8  { grid-column: span 8; }
.span-12 { grid-column: span 12; }
.row-2   { grid-row: span 2; }
```

```html
<div class="vhs-grid">
  <div class="span-5 panel-white">...</div>
  <div class="span-7 panel-vermillion">...</div>
  <div class="span-4 panel-black">...</div>
  <div class="span-8 panel-white hlines">...</div>
</div>
```

### Panel Variants

```css
.panel-black {
  background: var(--black);
  color: var(--white);
}
.panel-white {
  background: var(--white);
  color: var(--black);
}
.panel-vermillion {
  background: var(--vermillion);
  color: var(--black);
}
.panel-gray {
  background: var(--gray-10);
  color: var(--black);
}
```

### Version/Code Labels

Small technical identifiers placed in corners of panels:

```css
.code-label {
  display: inline-flex;
  align-items: center;
  gap: 0.4em;
  padding: 0.3em 0.6em;
  font-family: var(--font-mono);
  font-size: 0.6875rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  border: 1.5px solid currentColor;
}
.code-label.inverted {
  background: var(--black);
  color: var(--white);
  border-color: var(--black);
}
```

```html
<span class="code-label">R9</span>
<span class="code-label inverted">USA</span>
<span class="code-label">S-MAX</span>
```

### Warning Labels

```css
.warning {
  font-family: var(--font-mono);
  font-size: 0.6875rem;
  font-weight: 600;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  padding: 0.5em 0;
  border-top: 2px solid currentColor;
}
.warning::before {
  content: 'WARNING: ';
  color: var(--vermillion);
}
/* Full-width warning bar */
.warning-bar {
  display: flex;
  align-items: center;
  gap: 0.8em;
  padding: 0.6em var(--panel-pad);
  background: var(--black);
  color: var(--vermillion);
  font-family: var(--font-mono);
  font-size: 0.625rem;
  font-weight: 600;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}
.warning-bar .icon { font-size: 1em; }
```

```html
<div class="warning">Do not interrupt cycle</div>
<div class="warning-bar"><span class="icon">⚠</span> Human presence: Not required</div>
```

### Bottom Runner Bar

The ticker/info bar at the bottom of VHS labels:

```css
.runner-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.6em var(--panel-pad);
  background: var(--black);
  color: var(--white);
  font-family: var(--font-mono);
  font-size: 0.625rem;
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  gap: 2rem;
}
.runner-bar .separator {
  color: var(--gray-60);
}
.runner-bar .highlight {
  color: var(--vermillion);
}
```

```html
<div class="runner-bar">
  <span>5-WEST</span>
  <span>Actuator Sync and Payload Transitions</span>
  <span class="separator">/</span>
  <span class="highlight">TRACK CODE: 09-C3</span>
</div>
```

### Oversized Background Text

Giant watermark-style text behind content:

```css
.bg-text {
  position: absolute;
  font-family: var(--font-display);
  font-size: clamp(8rem, 20vw, 16rem);
  line-height: 0.85;
  letter-spacing: -0.04em;
  text-transform: uppercase;
  color: var(--black);
  opacity: 0.06;
  pointer-events: none;
  z-index: 0;
  user-select: none;
}
/* Vermillion background text (on white panels) */
.bg-text.vermillion {
  color: var(--vermillion);
  opacity: 0.12;
}
/* Outlined version */
.bg-text.outline {
  -webkit-text-stroke: 2px currentColor;
  color: transparent;
  opacity: 0.15;
}
```

```html
<div style="position:relative;">
  <span class="bg-text" style="top:-10%;right:-5%;">5</span>
  <span class="bg-text vermillion" style="bottom:0;left:0;">2026</span>
  <!-- Actual content sits above -->
</div>
```

### Hero Section

```css
.vhs-hero {
  position: relative;
  padding: 4rem 2rem;
  min-height: 70vh;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  overflow: hidden;
}
.vhs-hero h1 {
  font-family: var(--font-display);
  font-size: clamp(4rem, 10vw, 8rem);
  line-height: 0.9;
  letter-spacing: -0.02em;
  text-transform: uppercase;
  position: relative;
  z-index: 1;
}
.vhs-hero .tech-subtitle {
  font-family: var(--font-mono);
  font-size: 0.75rem;
  font-weight: 500;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  margin-top: 1.5rem;
  color: var(--gray-60);
  position: relative;
  z-index: 1;
}
```

### Buttons

```css
.btn-vhs {
  display: inline-flex;
  align-items: center;
  gap: 0.5em;
  padding: 0.7em 1.4em;
  font-family: var(--font-mono);
  font-size: 0.6875rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  text-decoration: none;
  border: 2px solid currentColor;
  cursor: pointer;
  transition: all 0.15s;
  position: relative;
}
.btn-vhs.primary {
  background: var(--vermillion);
  color: var(--black);
  border-color: var(--vermillion);
}
.btn-vhs.primary:hover {
  background: var(--vermillion-dark);
  border-color: var(--vermillion-dark);
}
.btn-vhs.dark {
  background: var(--black);
  color: var(--white);
  border-color: var(--black);
}
.btn-vhs.ghost {
  background: transparent;
  color: var(--black);
  border-color: var(--black);
}
.btn-vhs.ghost:hover {
  background: var(--black);
  color: var(--white);
}
/* Arrow indicator on buttons */
.btn-vhs .arrow { font-size: 1.1em; }
```

### Decorative Asterisk / Starburst

```css
.starburst {
  display: inline-block;
  width: 2em; height: 2em;
  background: var(--vermillion);
  -webkit-mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M12 0l2.5 8.5L24 12l-9.5 2.5L12 24l-2.5-9.5L0 12l9.5-2.5z'/%3E%3C/svg%3E");
  mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='M12 0l2.5 8.5L24 12l-9.5 2.5L12 24l-2.5-9.5L0 12l9.5-2.5z'/%3E%3C/svg%3E");
  mask-size: contain;
  -webkit-mask-size: contain;
}
```

### Registered Trademark Symbol

```css
.reg-symbol {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 1.2em; height: 1.2em;
  border: 1.5px solid currentColor;
  border-radius: 50% !important; /* Exception */
  font-size: 0.6em;
  vertical-align: super;
  margin-left: 0.2em;
}
```

```html
<h2>Factory Autonomy<span class="reg-symbol">®</span></h2>
```

---

## Layout Patterns

### The VHS Label Grid

The signature layout: dense, asymmetric panels that feel like a VHS tape insert.

```
┌─────────────────────┬───┬─────────────────────────────┐
│                     │R9 │  MULTI ROBOT COORDINATION    │
│  Robotics           │   │  TEST                       │
│  Program         5  ├───┤                             │
│                     │░░░│  Adaptive                   │
│  R-100 ☐           │░░░│  Manufacturing  ● ● ● ●    │
├─────────────────────┤░░░│                ● ● ● ●     │
│                     │   │  POWER DOWN ONLY BY         │
│  Factory     ®      │   │  AUTHORIZED STAFF           │
│  Autonomy           ├───┴─────────────────────────────┤
│                     │                                 │
│  Human presence:    │  HIGH OUTPUT                    │
│  Not required       │  ║║║║║║║║║║║║║║║║║║║║║║║║║     │
│  S-Max              │  TRACK CODE: 09-C3             │
├─────────────────────┴─────────────────────────────────┤
│ WARNING: DO NOT INTERRUPT CYCLE    ✱     2026    ↗    │
└───────────────────────────────────────────────────────┘

Legend:
░░░ = diagonal stripe pattern
║║║ = horizontal line pattern
● = dot grid
☐ = barcode element
```

### Information Hierarchy

1. **One massive headline** per panel (Bebas Neue, huge)
2. **One code label** per panel corner (R9, S-MAX)
3. **One technical subtitle** (IBM Plex Mono, small, wide-spaced)
4. **Pattern fills** on 20-30% of panels (stripes, dots, lines)
5. **Runner bar** at the bottom with tracking codes
6. **One oversized background number/word** bleeding across panels

### Composition Rules

- **Asymmetric balance.** Never center everything. Push headlines to edges.
- **Color blocking.** Each panel is a single solid color (black, white, or vermillion). No gradients.
- **Pattern + Content.** Patterns share space with text. Text overlaps stripes. Headlines bleed over dot grids.
- **Scale contrast.** Massive display type next to tiny technical labels. No medium.
- **Horizontal dominance.** Layouts read left-to-right like a tape label. Runner bars reinforce this.
- **Bleed.** Oversized text and numbers should clip at panel edges. Don't contain everything neatly.

---

## Image Treatment

Photos in retro-ui get a high-contrast duotone treatment:

```css
.retro-image {
  filter: grayscale(1) contrast(1.3) brightness(1.1);
  mix-blend-mode: multiply;
}
/* Vermillion duotone */
.retro-image-container {
  position: relative;
  background: var(--vermillion);
}
.retro-image-container img {
  filter: grayscale(1) contrast(1.4);
  mix-blend-mode: multiply;
  opacity: 0.85;
}

/* High-contrast BW */
.retro-image-bw {
  filter: grayscale(1) contrast(1.8) brightness(0.9);
}
```

---

## Micro-Interactions

Keep animations minimal and mechanical:

```css
/* No ease — use linear or step transitions for industrial feel */
.vhs-transition {
  transition: all 0.12s linear;
}

/* Flicker on hover (VHS glitch) */
@keyframes vhs-flicker {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.85; }
}
.flicker:hover {
  animation: vhs-flicker 0.1s steps(2) 3;
}

/* Typewriter reveal for technical text */
@keyframes type-reveal {
  from { width: 0; }
  to { width: 100%; }
}
.type-reveal {
  overflow: hidden;
  white-space: nowrap;
  animation: type-reveal 1.5s steps(30) forwards;
}
```

---

## Anti-Patterns — What NOT to Do

| ❌ Don't | ✅ Do |
|----------|-------|
| Border-radius on anything | Sharp corners, always |
| More than 3 colors | Black + white + vermillion only |
| Blue, green, purple accent | Vermillion (red-orange) only |
| Rounded sans-serif (Inter, Nunito) | Condensed bold (Bebas Neue) + Mono (IBM Plex) |
| Generous whitespace | Dense, tight grid gaps (2-4px) |
| Centered-everything layouts | Asymmetric, edge-pushing compositions |
| Gradients | Flat color blocks |
| Drop shadows | Borders or raw color contrast |
| Lowercase headlines | UPPERCASE almost everything |
| Medium-sized type | Extreme scale contrast (massive + tiny) |
| Smooth bezier animations | Linear / step transitions |
| Decorative illustrations | Patterns (stripes, dots, lines, barcodes) |
| Thin, light typefaces | Bold, condensed, heavy |
| Serif fonts | Sans-serif display + monospace labels |
| Wide letter-spacing on headlines | TIGHT on headlines, WIDE on labels only |
| Cute/friendly tone | Industrial, imperative, mechanical |

---

## Quick Start Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>RETRO UI</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Archivo+Black&family=Bebas+Neue&family=IBM+Plex+Mono:wght@400;500;600&family=Space+Grotesk:wght@400;500;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --black: #0A0A0A; --white: #F5F5F0; --vermillion: #E8432D;
      --gray-60: #666; --gray-30: #B0B0A8; --gray-10: #E8E8E2;
      --font-display: 'Bebas Neue', Impact, sans-serif;
      --font-wide: 'Archivo Black', 'Arial Black', sans-serif;
      --font-mono: 'IBM Plex Mono', monospace;
      --font-body: 'Space Grotesk', system-ui, sans-serif;
    }
    *, *::before, *::after { margin:0; padding:0; box-sizing:border-box; border-radius:0!important; }
    body { background: var(--white); color: var(--black); font-family: var(--font-body); }
    /* Add remaining tokens and components as needed */
  </style>
</head>
<body>
  <div class="vhs-grid">
    <div class="span-5 panel-white" style="padding:2rem;">
      <span class="code-label">R-100</span>
      <h1 style="font-family:var(--font-display);font-size:4rem;line-height:0.9;text-transform:uppercase;margin-top:1rem;">
        Your Headline Here
      </h1>
    </div>
    <div class="span-7 panel-vermillion" style="padding:2rem;">
      <h2 style="font-family:var(--font-wide);font-size:3rem;text-transform:uppercase;">
        Section Title
      </h2>
    </div>
  </div>
  <div class="runner-bar">
    <span>SECTOR-01</span>
    <span>PAYLOAD ACTIVE</span>
    <span style="color:var(--vermillion);">TRACK CODE: 09-C3</span>
  </div>
</body>
</html>
```

---

## Accessibility Notes

- Black on white: ~18:1 (exceeds AAA)
- White on black: ~18:1 (exceeds AAA)
- Black on vermillion: ~4.0:1 — **passes AA for large text only.** Keep body text on vermillion panels large (18px+) or switch to white text on vermillion.
- White on vermillion: ~3.9:1 — same constraint. Large text only.
- Vermillion on white: ~3.9:1 — use for decorative elements, NOT body text.
- All-uppercase at small sizes hurts readability. Keep uppercase to headings and labels; body text stays sentence case.
- High contrast patterns (stripes, dots) can cause visual discomfort. Use `prefers-reduced-motion` to simplify animations and reduce pattern density.

```css
@media (prefers-reduced-motion: reduce) {
  .flicker:hover { animation: none; }
  .type-reveal { animation: none; width: 100%; }
}
@media (prefers-contrast: more) {
  .panel-vermillion { color: var(--white); }
}
```
