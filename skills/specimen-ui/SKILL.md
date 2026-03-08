---
name: specimen-ui
description: Vintage typography specimen / letterpress aesthetic for web interfaces — mixed type styles across eras (blackletter, wood type, modern sans, script), specimen sheet layouts, ink-on-paper texture, print registration marks, type size callouts, glyph showcases, and the layered history of 500 years of printing. Dark or cream backgrounds with high-contrast type as the primary visual element.
---

# Specimen UI — Vintage Typography / Letterpress Aesthetic

Build web interfaces that feel like a page torn from a 19th century type foundry catalog or a letterpress workshop poster. The TYPOGRAPHY is the design — mixed styles across eras, from blackletter to wood type to modern grotesque, all coexisting on the same surface. Ink texture, specimen-sheet layouts, size callouts, and the physical weight of metal type.

**The text IS the decoration. No icons. No illustrations. Type only.**

---

## Design Tokens

### Color Palette

```css
:root {
  /* Dark variant (workshop poster) */
  --ink-black:      #0A0A08;  /* Rich black — like fresh ink */
  --paper-dark:     #1C1A16;  /* Dark aged paper */
  --ink-brown:      #2A2420;  /* Dark brown-black */

  /* Light variant (specimen sheet) */
  --paper:          #F0EBE0;  /* Cream stock */
  --paper-aged:     #E4DCCC;  /* Yellowed paper */

  /* Ink colors */
  --ink:            #1A1814;  /* Primary text on light */
  --ink-red:        #C42B1C;  /* Second-color ink (red accent) */
  --ink-red-dim:    #8B1E14;  /* Faded red */
  --ink-faded:      #8A8478;  /* Aged/light impression */

  /* On dark backgrounds */
  --type-white:     #E8E2D4;  /* Cream white type */
  --type-dim:       #6A6458;  /* Faded type */
}
```

**Rules:**
- Choose dark OR light variant. Don't mix backgrounds.
- Color is minimal: black + one accent (red). Maximum two ink colors.
- Red accent at ~10% of composition — a specimen sheet accent mark, not a primary color.
- Text should feel like it has physical weight. Thick strokes, deep impressions.

### Typography

This is the ONLY skill where you use 5+ typeface families. That's the point — a specimen sheet shows the full range.

```css
:root {
  --font-blackletter: 'UnifrakturMaguntia', 'Old English Text MT', serif;
  --font-didone:      'Playfair Display', 'Bodoni Moda', serif;       /* High-contrast serif */
  --font-wood:        'Archivo Black', 'Impact', sans-serif;           /* Bold condensed (wood type) */
  --font-grotesque:   'Space Grotesk', 'Helvetica Neue', sans-serif;  /* Modern sans */
  --font-script:      'Dancing Script', 'Brush Script MT', cursive;    /* Calligraphic script */
  --font-slab:        'Roboto Slab', 'Rockwell', serif;               /* Slab serif */
  --font-mono:        'IBM Plex Mono', monospace;                      /* Technical labels */
}
```

```html
<link href="https://fonts.googleapis.com/css2?family=UnifrakturMaguntia&family=Playfair+Display:ital,wght@0,400;0,700;0,900;1,400&family=Archivo+Black&family=Space+Grotesk:wght@400;500;700&family=Dancing+Script:wght@400;700&family=Roboto+Slab:wght@300;400;700&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet">
```

### Type Scale

The point is CONTRAST between sizes. Specimen sheets demonstrate scale.

| Role | Font | Size | Notes |
|------|------|------|-------|
| Display showcase | varies | `clamp(5rem, 14vw, 12rem)` | THE specimen — one massive word |
| Section header | wood or didone | `clamp(2.5rem, 5vw, 4rem)` | Bold, commanding |
| Body specimen | slab or grotesque | `1.125rem` | Showing readable text |
| Script accent | script | `clamp(2rem, 4vw, 3.5rem)` | Italic, flowing, decorative |
| Blackletter drop | blackletter | `clamp(3rem, 6vw, 5rem)` | Historic accent |
| Size callout | mono | `0.5625rem` | "72pt" "48pt" annotations |
| Specimen label | mono | `0.625rem` | Font name, foundry, year |
| Fine print | grotesque | `0.75rem` | Descriptions, notes |

---

## Components

### Specimen Block

The fundamental unit — showing a typeface at display size with metadata:

```css
.specimen {
  padding: 2rem 0;
  border-bottom: 1px solid var(--ink-faded);
  position: relative;
}
.specimen-display {
  font-size: clamp(4rem, 10vw, 8rem);
  line-height: 0.85;
  letter-spacing: -0.02em;
  /* Font family set per-specimen via class or style */
}
.specimen-meta {
  display: flex;
  gap: 2rem;
  margin-top: 1rem;
  font-family: var(--font-mono);
  font-size: 0.5625rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--ink-faded);
}
.specimen-meta .size-callout {
  color: var(--ink-red);
}
```

```html
<div class="specimen">
  <div class="specimen-display" style="font-family:var(--font-didone);font-weight:900;">
    Typography
  </div>
  <div class="specimen-meta">
    <span>Playfair Display</span>
    <span>Black 900</span>
    <span class="size-callout">96pt</span>
    <span>Claus Eggers Sørensen, 2011</span>
  </div>
</div>
```

### Glyph Showcase

```css
.glyph-row {
  display: flex;
  flex-wrap: wrap;
  gap: 0;
  border: 1px solid var(--ink-faded);
}
.glyph-cell {
  width: 60px; height: 70px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border: 1px solid var(--ink-faded);
  font-size: 1.8rem;
  position: relative;
}
.glyph-cell .code {
  position: absolute;
  bottom: 2px;
  font-family: var(--font-mono);
  font-size: 0.4375rem;
  color: var(--ink-faded);
}
```

### Size Scale Display

```css
.size-scale {
  display: flex;
  flex-direction: column;
  gap: 0;
}
.size-scale-row {
  display: flex;
  align-items: baseline;
  gap: 1rem;
  padding: 0.3rem 0;
  border-bottom: 1px solid var(--ink-faded);
}
.size-scale-row .pt {
  font-family: var(--font-mono);
  font-size: 0.5625rem;
  width: 3rem;
  text-align: right;
  color: var(--ink-faded);
  flex-shrink: 0;
}
.size-scale-row .sample {
  white-space: nowrap;
  overflow: hidden;
}
```

### Ornamental Rule

```css
.ornament {
  text-align: center;
  padding: 1.5rem 0;
  font-size: 1.5rem;
  color: var(--ink-faded);
  letter-spacing: 0.5em;
}
/* Use typographic ornaments: ❧ ✦ ✻ ❦ ☙ ※ ◆ ■ */
```

### Section Divider (Thick/Thin Rule)

```css
.rule-thick-thin {
  border: none;
  border-top: 3px solid var(--ink);
  position: relative;
  margin: 2rem 0;
}
.rule-thick-thin::after {
  content: '';
  position: absolute;
  top: 4px;
  left: 0; right: 0;
  height: 1px;
  background: var(--ink);
}
```

### Drop Cap

```css
.drop-cap::first-letter {
  font-family: var(--font-blackletter);
  font-size: 4em;
  float: left;
  line-height: 0.8;
  padding-right: 0.1em;
  color: var(--ink-red);
}
```

### Printer's Mark / Registration

```css
.printers-mark {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 2em; height: 2em;
  border: 1px solid var(--ink-faded);
  border-radius: 50% !important;
  font-family: var(--font-mono);
  font-size: 0.5rem;
  color: var(--ink-faded);
}
```

---

## Layout

### Specimen Sheet

```css
.specimen-sheet {
  max-width: 900px;
  margin: 0 auto;
  padding: 3rem 2rem;
}
/* Vertical rhythm: thick rules separate major sections */
.specimen-sheet > section {
  padding: 2rem 0;
}
```

### Poster Layout (Dark Variant)

```css
.poster-layout {
  background: var(--ink-black);
  color: var(--type-white);
  padding: 3rem 2rem;
  /* Type stacked vertically with dramatic size variation */
}
.poster-layout .hero-type {
  font-size: clamp(6rem, 15vw, 14rem);
  line-height: 0.8;
  letter-spacing: -0.03em;
  text-transform: uppercase;
}
.poster-layout .sub-type {
  font-family: var(--font-script);
  font-size: clamp(2rem, 5vw, 4rem);
  color: var(--type-dim);
  margin-top: -0.5rem;
}
```

### Multi-Column Specimen

```css
.specimen-columns {
  columns: 2;
  column-gap: 3rem;
  column-rule: 1px solid var(--ink-faded);
}
@media (max-width: 600px) { .specimen-columns { columns: 1; } }
```

---

## Composition Rules

1. **Mix type eras freely.** Blackletter next to grotesque next to didone. That's the point.
2. **Size contrast is extreme.** 12rem headline → 0.5rem label. No gradual steps.
3. **Alignment varies.** Left-aligned body, centered display, right-aligned meta. Like a real specimen sheet.
4. **Red ink is punctuation.** Size callouts, drop caps, ornaments. Never backgrounds.
5. **Horizontal rules organize.** Thick/thin rules, ornamental dividers, visible structure.
6. **Let type breathe.** Despite dense information, give display type room to be appreciated.
7. **Show the whole alphabet** where space allows: `ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz 0123456789`

---

## Anti-Patterns

| ❌ Don't | ✅ Do |
|----------|-------|
| One font family | 4-6 contrasting families |
| Consistent type size | Extreme scale contrast (12rem to 0.5rem) |
| Icons and illustrations | Typography IS the visual design |
| Colorful backgrounds | Black or cream only |
| More than 2 ink colors | Black + one accent (red) |
| Digital-clean rendering | Show texture, weight, impression |
| Symmetric/centered everything | Mixed alignment per element |
| Sans-serif only | The full range: blackletter, didone, wood, script, slab, grotesque |

---

## Accessibility

- Dark variant: cream type `#E8E2D4` on black `#0A0A08` = 16:1 ✅
- Light variant: ink `#1A1814` on cream `#F0EBE0` = 13:1 ✅
- Red accent `#C42B1C` on cream: 5.8:1 ✅ (passes AA)
- Blackletter is inherently harder to read — use ONLY for decorative display, never body text
- Script fonts: display only, never below 2rem
- Ensure size callouts and labels are accessible (not just decorative)
- Multi-font pages should have consistent reading order for screen readers
