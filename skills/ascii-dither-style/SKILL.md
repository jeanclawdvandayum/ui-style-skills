# ASCII Dither Style

A design system where character density IS the visual language. Organic shapes rendered through ASCII characters or halftone dots at varying densities. Not decoration — the dithering is the design itself.

## References
- Variant `--sref a7eeb4` aesthetic
- Mural Systems (dark bg, `+`, `-`, `|`, `.` forming organic shapes)
- Colin van Eenige's monochrome work
- SYS Compute Architecture (warm palette, split layout, ASCII matrix)
- Classic halftone printing (circular dots at varying sizes)

## Core Principles

### Characters Are Texture, Not Garnish
**WRONG:** Sprinkling `░▒▓` as decorative borders or dividers.
**RIGHT:** A canvas of characters at varying densities forming abstract organic shapes that take up significant visual real estate (50%+ of a section).

### The Character Ramp
Light to dense: ` .·:;|+*oO#@█`

Map a noise value (0–1) to this ramp. Sparse characters = light areas. Dense characters = dark/filled areas. The shapes emerge from density variation, not from arranging characters into pictures.

### Noise Function (Fast Approximation)
Layered sin waves approximate Perlin noise cheaply:
```js
function noise(x, y, t) {
  const n1 = Math.sin(x * 0.018 + t * 0.4) * Math.cos(y * 0.024 + t * 0.3)
  const n2 = Math.sin(x * 0.035 + y * 0.012 + t * 0.2) * 0.6
  const n3 = Math.cos(x * 0.008 - y * 0.015 + t * 0.15) * Math.sin(y * 0.04 + t * 0.25) * 0.4
  const n4 = Math.sin((x + y) * 0.01 + t * 0.1) * Math.cos((x - y) * 0.02 + t * 0.18) * 0.3
  return (n1 + n2 + n3 + n4) * 0.5 + 0.5
}
```
- `scale` parameter multiplies coordinates → smaller scale = larger shapes
- `speed` parameter multiplies time → slower = more ambient
- 4 layers minimum for organic-looking results

### Canvas Rendering (Not DOM Text)
Render characters to `<canvas>` for performance. A 100x60 grid is 6,000 characters per frame — DOM can't handle that smoothly.

```js
ctx.font = `${charSize}px "JetBrains Mono", monospace`
ctx.textBaseline = 'top'
ctx.fillStyle = color

for (let row = 0; row < rows; row++) {
  for (let col = 0; col < cols; col++) {
    const val = noise(col * scale, row * scale, time)
    const idx = Math.floor(val * (RAMP.length - 1))
    const char = RAMP[idx]
    if (char === ' ') continue
    ctx.globalAlpha = 0.3 + val * 0.7  // depth via opacity
    ctx.fillText(char, col * charSize, row * charSize)
  }
}
```

Key settings:
- `charSize`: 8–12px (smaller = finer detail, more characters)
- Use `devicePixelRatio` (cap at 2x) for crisp rendering
- `ResizeObserver` for responsive canvas sizing
- Animate with `requestAnimationFrame`, slow speed (0.008–0.012 per frame)

## Layout Patterns

### Split Layout (Hero)
Content on one side, dither art on the other. The dither panel should be at least 40% of the width.

```
┌──────────────────┬──────────────────┐
│                  │ ·:;+*oO#@█      │
│  BRAND           │ .·:;|+*oO#      │
│                  │ .:;|+*oO#@█     │
│  Title text      │ .·:;+*oO#@      │
│  here.           │ ·:;|+*oO#@█     │
│                  │ .:;+*oO#@█      │
│  [Button]        │ .·:;|+*oO       │
│                  │                  │
│  LABEL   VALUE   │                  │
│  LABEL   VALUE   │  overlay text    │
└──────────────────┴──────────────────┘
```

### 2×2 Grid (Sections)
Use `gap-px bg-border` on the grid to create 1px dividing lines between cells.

### Metadata Table
Left-aligned labels (uppercase monospace), right-aligned values. Thin border-bottom on rows.

## Typography

- **Body:** Monospace dominant (JetBrains Mono, SF Mono, Consolas)
- **Display headings:** Sans-serif (Outfit, system) for contrast — large, bold, tight tracking
- **Labels:** `10px uppercase tracking-[0.2em] monospace` — the label-mono pattern
- **Section headers:** `// protocol` style comment prefix (optional)
- **Number styling:** `( 01 )` with spaces inside parens

## Color Strategy

### Monochrome or Duo-Tone Only
Pick ONE hue family. All elements — text, characters, accents, borders — live in that hue at different luminance levels.

### 4-Color Limited Palette (for pixel art / constrained UI)
Design with even perceptual luminance spacing:

| Role        | Luminance | Example (Copper)  |
|-------------|-----------|-------------------|
| Background  | 2–5%      | `#1a110c`         |
| Shadow      | 6–10%     | `#6b3a20`         |
| Body        | 25–35%    | `#c8854e`         |
| Highlight   | 70–85%    | `#f2dab8`         |

Verify with relative luminance calculation. Adjacent steps should have 2:1–3:1 contrast ratio. Full range should exceed 10:1.

### Warm Palette (Alchemical)
```
--darkest:  #1a110c  (obsidian bronze)
--dark:     #6b3a20  (burnt copper)
--mid:      #c8854e  (warm copper)
--light:    #f2dab8  (alchemist's gold)
--accent:   #e8a87c  (primary amber)
```

## Buttons
- Square (border-radius: 0 or 2px max)
- Monospace, uppercase, wide letter-spacing (0.15em)
- Padding: 14px 32px
- Filled primary: accent color bg, dark text
- Outline secondary: transparent bg, border, muted text
- Hover: background swap to white or glow

## Cards
- No border-radius (or 2px max)
- 1px solid border in border color
- No blur/glass effects
- Hover: subtle border lighten, optional glow (rgba accent / 0.03)

## What NOT To Do
- Don't use `░▒▓` as decorative borders or dividers — that's not dithering
- Don't put ASCII art inline as text content
- Don't use glass/blur/frosted effects — this aesthetic is opaque and flat
- Don't mix multiple hue families
- Don't use rounded corners > 2px
- Don't use gradient backgrounds
- Don't use emoji

## Page Centering (Critical)

### The Flex-Column Auto-Margin Bug
`mx-auto` (Tailwind) can fail inside `flex flex-col` parents. The flex layout algorithm overrides auto margins.

**WRONG:**
```jsx
<div className="flex flex-col">
  <div className="w-full max-w-[1200px] mx-auto">  {/* margins compute to 0 */}
```

**RIGHT:**
```jsx
<div className="min-h-[100dvh] bg-bg">
  <div style={{ maxWidth: '1200px', marginLeft: 'auto', marginRight: 'auto', paddingLeft: '6%', paddingRight: '6%' }}>
```

### Percentage Padding > Fixed Padding
`px-8` (32px) is invisible on a 1680px screen. Use percentage-based padding (`6%`) so margins scale with viewport and are always visible.

### No Inner Max-Width Caps
Don't put `max-w-[900px] mx-auto` on individual page components inside an already-centered container. It creates an off-center look where content clusters to the left. Let the Layout container handle width constraints.

## File Structure
```
components/DitherCanvas.tsx  — Canvas-rendered ASCII art (configurable color, charSize, speed, scale, opacity)
components/DitherOverlay.tsx — Utility components (DitherDivider, AsciiBox, StatBlock, BlockGradient)
styles.css                   — Design tokens, .glass, .btn-magic, .btn-outline, .label-mono, .text-glow-*
```
