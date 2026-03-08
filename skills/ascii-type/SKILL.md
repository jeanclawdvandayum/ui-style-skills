# ascii-type — ASCII Character Rendering Engine

## What It Is
A single-file HTML rendering engine that displays large text as **fields of ASCII characters** on a dark background. Text is rasterized through a 6×8 bitmap font, subdivided into a grid, then each cell becomes an ASCII character chosen from density-mapped pools. The result looks like text made of living, breathing ASCII art — with glow compositing, CRT scanlines, vignette, ambient drift, and glitch pulses.

**Canvas 2D only.** No Three.js. No WebGL. No dependencies. No build tools.

## When to Use
- Slide decks / presentations with dramatic text reveals
- Landing pages / hero sections with cinematic typography
- Data visualizations where text IS the visual
- Any project wanting a retro-terminal-meets-art aesthetic

## Core Architecture

```
Bitmap Font (6×8 per char)
    ↓
Text → Grid (with subdivision multiplier)
    ↓
Dilation (thicken strokes by 1 cell)
    ↓
Cell Generation:
  - Edge detection (8-neighbor) → size falloff
  - Directional lighting → color index (0-3)
  - Color index → character density pool
  - Assembly animation params (angle, distance, stagger)
    ↓
Render Loop:
  - Assembly interpolation (easeOutBack)
  - Ambient drift (sin/cos)
  - Character cycling (~3-8% of cells per tick)
  - Glitch pulses (4-10s cooldown, accent color)
    ↓
Composite:
  - Draw to offscreen canvas via fillText()
  - Glow layer: blur(3px) at 0.35 alpha
  - Sharp layer: direct composite
  - CRT scanlines (every 3px, 0.04 alpha)
  - Radial vignette (0.3→0.85 radius)
```

## Key Design Rules

### Subdivision & Cell Sizing
- **Adaptive subdivision**: tries sub=5→2, picks highest where cell ≥ `minCell` px
- `minCell = 10` — ASCII chars need ~10px+ to be legible (unlike voxels which work at 3px)
- Short text (e.g. "V3") gets huge cells; long text gets proportionally smaller
- Text occupies 90% width × 72% height of viewport

### Character Density Pools
Characters are chosen by **color index** (0=darkest, 3=brightest). Denser characters = brighter visual weight:

```javascript
// ci 0 (sparse/dark):  . , ' `
// ci 1 (light):        : - ; ~ = +
// ci 2 (medium):       / \ | * x < >
// ci 3 (dense/bright): @ # % & $ W M
// Edge chars:          . , : ; -
// Glitch chars:        ! @ # $ % ^ & * ( ) [ ] { } | < > ? ~
```

### Color Palettes
Every palette has exactly **4 colors** (indexed 0-3, dark→bright) plus **1 accent** for glitch pulses:

```javascript
{ c: ['#5a3a22', '#9b6a40', '#d8a568', '#f5e0c0'], a: '#40e0d0' }
//     ci:0        ci:1        ci:2        ci:3      accent
```

**Critical:** ASCII chars have ~30% visual fill vs 100% for solid squares. Colors must be MUCH brighter than you'd use for filled rectangles. Darkest color should be around `#5a3a22` (not `#1a0e08`).

### Font Rendering
- **Direct `fillText()`** — no pre-rendered bitmap cache. Browser font hinting gives better results at small sizes.
- Font: `bold ${fontSize}px "Courier New", Consolas, "Liberation Mono", monospace`
- Font size multiplier: **0.82×** cell size (prevents chars from touching)
- Cells sorted by size descending → large cells draw first, small edge cells overlay

### Edge Detection & Size Falloff
8-neighbor edge detection. More exposed edges = smaller cell = sparser character:

```javascript
const EDGE_SIZES = [1, 0.85, 0.70, 0.55, 0.40];
// [interior, 1 edge, 2 edges, 3 edges, 4+ edges]
```

### Directional Lighting
Fake top-left light source maps grid position to color index:
```javascript
const light = 1 - (nx * 0.35 + ny * 0.55); // nx,ny normalized 0-1
const noise = (Math.random() - 0.5) * 0.22; // dithering
const ci = clamp(0, 3, floor((light + noise) * 3.99));
```

### Animation
- **Assembly**: cells fly in from random angles (400-750px away), easeOutBack, staggered by distance from center
- **Ambient drift**: subtle sin/cos oscillation after assembly (1.5px amplitude)
- **Character cycling**: 1.5-3% of cells swap characters per tick (every 80ms)
- **Glitch pulses**: random cells in right 20% flash accent-colored glitch chars for 100-200ms, cooldown 4-10s
- **Mouse parallax**: perspective(1200px) rotateX/Y based on cursor position

### CRT Effects
- **Scanlines**: 1px black every 3px at 4% opacity
- **Vignette**: radial gradient, transparent at 30% radius → rgba(0,0,0,0.5) at 85% radius
- **Background**: `#050505` (not pure black — allows vignette to register)

## Template: Slideshow

The canonical use case. Each slide has text, a 4+1 color palette, and a subtitle:

```javascript
const SLIDES = [
  {
    text: 'HELLO\nWORLD',         // Multi-line supported via \n
    pal: {
      c: ['#5a3a22', '#9b6a40', '#d8a568', '#f5e0c0'],  // 4 colors dark→bright
      a: '#40e0d0'                                        // accent for glitches
    },
    sub: 'Subtitle with <strong>bold</strong> support.'
  },
  // ... more slides
];
```

### Navigation (included in template)
- **Click**: left 30% = prev, right 70% = next
- **Keyboard**: ←/→ arrows, spacebar
- **Touch**: swipe left/right (50px threshold)
- **Scroll**: wheel up/down (600ms debounce)
- **Progress bar**: top of viewport, 2px height

### Subtitle Styling
```css
.sub {
  color: rgba(255,255,255,0.72);
  font: italic clamp(13px, 2.2vw, 20px)/1.65 Georgia, serif;
  max-width: 560px;
  transition: opacity 0.8s ease 0.5s;  /* 500ms delay after slide transition */
}
.sub strong { color: rgba(255,255,255,0.92); font-style: normal; }
```

## Template: Static Hero

For a single text display (no slides):

```javascript
const text = 'YOUR\nTEXT';
const pal = { c: ['#5a3a22','#9b6a40','#d8a568','#f5e0c0'], a: '#40e0d0' };
const cells = buildCells(text, pal, W, H);
// Start render loop — no slide navigation needed
```

## Bitmap Font Reference

The engine includes a complete 6×8 bitmap font covering:
- **Uppercase A-Z**
- **Digits 0-9**
- **Symbols:** ↑ ↓ space
- **Character width:** 6 pixels, **height:** 8 pixels

To add custom characters:
```javascript
def('!', '..##..','..##..','..##..','..##..','......','..##..','..##..','......');
//        row 0    row 1    row 2    row 3    row 4    row 5    row 6    row 7
// '#' = filled pixel, '.' = empty pixel, always 6 chars wide × 8 rows
```

## Palette Gallery

Proven palettes from the V3 explainer:

| Name | Colors | Accent | Mood |
|------|--------|--------|------|
| Copper | `#5a3a22 #9b6a40 #d8a568 #f5e0c0` | `#40e0d0` | Warm alchemical |
| Ocean | `#2a4870 #4888b8 #78c0e8 #c0e8ff` | `#ff6600` | Deep blue cool |
| Royal | `#3258a0 #5888c8 #88b8e8 #d0e8ff` | `#ff44aa` | Bright authority |
| Amber | `#6a3818 #a86830 #e89848 #ffd898` | `#00ffee` | Warm golden |
| Forest | `#1e6838 #40a060 #70d890 #b8f8d0` | `#ffee00` | Growth/nature |
| Violet | `#5838a0 #7858c8 #a880e8 #d0b8ff` | `#00ff88` | Mystical |

## Anti-Patterns

❌ **Don't use dark palettes** — `#1a0e08` as darkest color makes ASCII invisible. Start at `#5a3a22` minimum.
❌ **Don't use Three.js** — this aesthetic is flat/orthographic. Canvas 2D is the right tool.
❌ **Don't pre-render fonts to bitmap cache** — `fillText()` gives better hinting at small sizes.
❌ **Don't set minCell below 8** — ASCII becomes unreadable mush.
❌ **Don't use lowercase** — the bitmap font is uppercase only. Convert input: `text.toUpperCase()`.
❌ **Don't skip the glow layer** — without it, chars look flat and lifeless. The blur+alpha composite is what gives depth.
❌ **Don't make scanlines too strong** — 0.04 alpha is the sweet spot. Higher and it dominates.

## Full Working Template

The complete engine is available at:
```
~/Desktop/projects/ascii-type/v3-explainer.html
```

Copy this file as your starting point. It's a single HTML file (~22KB) with zero dependencies. Modify the `SLIDES` array and palettes to match your content.

### Quick Start
1. Copy the template file
2. Edit the `SLIDES` array — change `text`, `pal`, and `sub` for each slide
3. Open in browser. Done.

### Customization Points
- `minCell` in `calcSub()` — lower = denser packing, less legible
- `EDGE_SIZES` array — controls falloff aggressiveness
- `0.82` font multiplier — controls character spacing within cells
- `cycleRate` — higher = more character churn
- `glitchCooldown` range — controls glitch frequency
- Glow blur radius and alpha — controls depth/atmosphere
- Scanline spacing and alpha — controls CRT intensity
