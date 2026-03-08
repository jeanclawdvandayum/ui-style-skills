---
name: western-ui
description: Old Western textbook aesthetic for web interfaces — warm parchment backgrounds, calligraphic serif typography with tight display tracking, amber accent color, canvas/linen texture overlays on images, zero border-radius, generous whitespace, and 19th century surveyor's manual elegance. Inspired by Relace.ai. Includes a canvas-texture image filter that makes any photo look like a plate from a vintage geography textbook.
---

# Western UI — Old Textbook Aesthetic

Build web interfaces that feel like a 19th century surveyor's field manual or a plate from an old Western geography textbook. Warm parchment tones, calligraphic serif type with tight tracking, amber accents, canvas-textured imagery, zero border-radius, and generous whitespace that breathes like the frontier.

**Reference:** [relace.ai](https://relace.ai/) — the defining implementation of this aesthetic.

---

## Design Tokens

### Color Palette

```css
:root {
  /* Surfaces */
  --parchment:       #FFFEF2;  /* Primary background — warm cream */
  --parchment-mid:   #F3F2E7;  /* Secondary surface — slightly darker cream */
  --parchment-dark:  #E8E7D9;  /* Tertiary/hover surface */

  /* Text */
  --ink:             #191918;  /* Primary text — near-black, never pure #000 */
  --ink-60:          rgba(25, 25, 24, 0.6);  /* Secondary/muted text */
  --ink-30:          rgba(25, 25, 24, 0.3);  /* Tertiary/placeholder */

  /* Accent */
  --amber:           #FCAA2D;  /* Primary accent — CTA buttons, highlights */
  --amber-dark:      #A36404;  /* Dark amber — accent text (italic emphasis) */
  --amber-hover:     #E89A20;  /* Amber hover state */

  /* Neutral */
  --warm-gray:       #BAB9B0;  /* Borders, dividers, rules */
  --warm-gray-light: rgba(209, 208, 198, 0.4);  /* Subtle dividers */

  /* Dark panels (code blocks, dark cards) */
  --obsidian:        #191918;  /* Same as ink — dark card backgrounds */
  --obsidian-soft:   #141414;  /* Slightly darker variant */
}
```

**Rules:**
- Background is ALWAYS `--parchment` (never white, never gray)
- Text is ALWAYS `--ink` (never pure `#000000`)
- Accent is ALWAYS `--amber` (never blue, never purple, never red)
- Dark panels use `--obsidian` with `--parchment` text (inverted)
- Borders use `--warm-gray` at 1px — thin, warm, never cold gray

### Typography

**Primary font: Fraunces** (Google Fonts — closest free match to Parabole)
- Has optical size axis (display ↔ text), wonk axis (irregular letterforms), soft axis
- Old-style numerals, calligraphic stroke variation
- The "wonk" axis is key — it adds the handwritten/antique irregularity

**Fallback stack: Imbue → Playfair Display → Georgia → serif**

**Secondary font: Karla** (Google Fonts — clean humanist sans)

**Monospace: IBM Plex Mono** (Google Fonts — warm, readable mono)

```html
<!-- Google Fonts import -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght,WONK@0,9..144,100..900,0..1;1,9..144,100..900,0..1&family=Karla:wght@400;500;700&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet">
```

```css
:root {
  --font-display: 'Fraunces', 'Imbue', 'Playfair Display', Georgia, serif;
  --font-body:    'Fraunces', Georgia, serif;
  --font-sans:    'Karla', system-ui, sans-serif;
  --font-mono:    'IBM Plex Mono', 'Source Code Pro', 'Courier New', monospace;
}
```

### Type Scale

| Role | Font | Size | Weight | Letter-spacing | Line-height | Transform |
|------|------|------|--------|----------------|-------------|-----------|
| **Display H1** | `--font-display` | `clamp(2.5rem, 5vw, 4rem)` | 400 | `-0.05em` | 1.1 | none |
| **H2** | `--font-display` | `clamp(1.8rem, 3vw, 2.25rem)` | 400 | `-0.04em` | 1.1 | none |
| **H3** | `--font-display` | `clamp(1.3rem, 2vw, 1.5rem)` | 400 | `-0.03em` | 1.2 | none |
| **Body** | `--font-body` | `clamp(1rem, 1.2vw, 1.125rem)` | 400 | `normal` | 1.5 | none |
| **Body small** | `--font-body` | `1rem` | 400 | `normal` | 1.5 | none |
| **Label** | `--font-body` | `0.75rem` | 500 | `0.04em` | 1.2 | `uppercase` |
| **Nav link** | `--font-body` | `0.75rem` | 400 | `0.04em` | 1.2 | `uppercase` |
| **Feature title** | `--font-body` | `1.25rem` | 500 | `-0.03em` | 1.06 | none |
| **Meta/caption** | `--font-body` | `0.875rem` | 500 | `-0.03em` | 1.5 | none |
| **Code** | `--font-mono` | `0.875rem` | 400 | `normal` | 1.6 | none |
| **Figure label** | `--font-sans` | `0.6875rem` | 400 | `0.06em` | 1.2 | `uppercase` |

**Critical rules:**
- Display headings use TIGHT negative letter-spacing (`-0.05em` at H1, `-0.04em` at H2). This is NOT optional — it's the signature look.
- Body text uses NORMAL letter-spacing. Don't tighten body text.
- Labels and nav use WIDE positive letter-spacing (`0.04em`) + uppercase.
- Weight 400 for almost everything. Use 500 only for emphasis labels, never bold for headings.
- The Fraunces `WONK` axis should be set to 1 for display headings to get the calligraphic irregularity:
  ```css
  h1, h2, h3 { font-variation-settings: 'WONK' 1, 'opsz' 48; }
  ```

### Spacing

```css
:root {
  --space-xs:   0.5rem;   /* 8px */
  --space-sm:   1rem;     /* 16px */
  --space-md:   1.5rem;   /* 24px */
  --space-lg:   2.5rem;   /* 40px */
  --space-xl:   6rem;     /* 96px — section padding */
  --space-xxl:  12.5rem;  /* 200px — hero top padding */
  --gutter:     2.5rem;   /* 40px — horizontal page padding */
}
```

**Rules:**
- Generous whitespace is MANDATORY. Sections get `--space-xl` (96px) vertical padding minimum.
- Hero gets `--space-xxl` (200px) top padding.
- Page horizontal padding: `--gutter` (40px). On mobile: 20px.
- Max content width: `1200px`, centered with `margin: 0 auto`.
- Between elements within a section: `--space-md` to `--space-lg`.

### Borders & Corners

```css
/* ZERO border-radius everywhere. This is non-negotiable. */
* { border-radius: 0 !important; }

/* Borders are 1px warm gray */
.bordered { border: 1px solid var(--warm-gray); }

/* Horizontal rules */
hr {
  border: none;
  height: 1px;
  background: var(--warm-gray-light);
  margin: var(--space-xl) 0;
}
```

**CRITICAL: Zero border-radius on EVERYTHING.** Buttons, cards, inputs, images, tags, modals — all sharp corners. This is the single most defining rule of the aesthetic. Rounded corners = destroyed.

---

## Image Filter — "Old Textbook Plate"

The signature look: any photograph rendered as if it were a color plate from a 1950s-1970s geography textbook, printed on canvas/linen stock.

### Effect Components
1. **Canvas/linen texture overlay** — warp-and-weft fabric weave pattern
2. **Warm color shift** — desaturated, sepia-adjacent, yellowed highlights
3. **Compressed tonal range** — lifted shadows, dulled highlights
4. **Fine grain** — subtle noise layer beneath the canvas texture

### CSS-Only Approach (Quick)

```css
.western-plate {
  position: relative;
  overflow: hidden;
  display: inline-block;
}

.western-plate img {
  display: block;
  width: 100%;
  filter: sepia(0.35) saturate(0.65) contrast(1.05) brightness(1.02);
}

/* Canvas/linen texture overlay via SVG */
.western-plate::after {
  content: '';
  position: absolute;
  inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='4' height='4'%3E%3Crect width='4' height='4' fill='%23d4c9a8'/%3E%3Crect x='0' y='0' width='2' height='1' fill='%23c8bc99' opacity='0.4'/%3E%3Crect x='2' y='1' width='2' height='1' fill='%23c8bc99' opacity='0.3'/%3E%3Crect x='0' y='2' width='2' height='1' fill='%23bfb490' opacity='0.35'/%3E%3Crect x='2' y='3' width='2' height='1' fill='%23bfb490' opacity='0.25'/%3E%3Crect x='0' y='0' width='1' height='2' fill='%23d0c49e' opacity='0.2'/%3E%3Crect x='2' y='2' width='1' height='2' fill='%23d0c49e' opacity='0.15'/%3E%3C/svg%3E");
  background-size: 4px 4px;
  mix-blend-mode: multiply;
  opacity: 0.5;
  pointer-events: none;
}

/* Optional: subtle grain via SVG noise */
.western-plate::before {
  content: '';
  position: absolute;
  inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.08'/%3E%3C/svg%3E");
  background-size: 256px 256px;
  mix-blend-mode: multiply;
  pointer-events: none;
  z-index: 1;
}
```

### JavaScript Canvas Approach (Proper)

For higher fidelity, use this canvas-based filter that generates a real woven canvas texture:

```javascript
/**
 * Renders an image with the old-textbook plate treatment.
 * @param {HTMLImageElement|string} source — img element or URL
 * @param {HTMLCanvasElement} canvas — target canvas
 * @param {Object} opts — { warmth: 0.35, saturation: 0.65, weaveOpacity: 0.4, grainOpacity: 0.06 }
 */
function westernPlate(source, canvas, opts = {}) {
  const { warmth = 0.35, saturation = 0.65, weaveOpacity = 0.4, grainOpacity = 0.06 } = opts;

  const img = typeof source === 'string' ? (() => { const i = new Image(); i.crossOrigin = 'anonymous'; i.src = source; return i; })() : source;

  const render = () => {
    const W = img.naturalWidth || img.width;
    const H = img.naturalHeight || img.height;
    canvas.width = W;
    canvas.height = H;
    const ctx = canvas.getContext('2d');

    // 1. Draw original image
    ctx.drawImage(img, 0, 0, W, H);

    // 2. Warm color shift + desaturation
    const imageData = ctx.getImageData(0, 0, W, H);
    const d = imageData.data;
    for (let i = 0; i < d.length; i += 4) {
      let r = d[i], g = d[i+1], b = d[i+2];
      // Desaturate
      const gray = 0.299 * r + 0.587 * g + 0.114 * b;
      r = r + (gray - r) * (1 - saturation);
      g = g + (gray - g) * (1 - saturation);
      b = b + (gray - b) * (1 - saturation);
      // Warm shift (sepia-like)
      d[i]   = Math.min(255, r + (255 - r) * warmth * 0.3);  // R up
      d[i+1] = Math.min(255, g + (200 - g) * warmth * 0.15); // G slightly up
      d[i+2] = Math.max(0, b - b * warmth * 0.2);             // B down
      // Compress tonal range (lift shadows, dull highlights)
      d[i]   = 20 + d[i] * 0.88;
      d[i+1] = 18 + d[i+1] * 0.87;
      d[i+2] = 15 + d[i+2] * 0.86;
    }
    ctx.putImageData(imageData, 0, 0);

    // 3. Canvas/linen weave texture (multiply blend)
    const weave = ctx.createImageData(W, H);
    const wd = weave.data;
    for (let y = 0; y < H; y++) {
      for (let x = 0; x < W; x++) {
        const i = (y * W + x) * 4;
        // Warp threads (horizontal emphasis on even rows)
        const warp = (y % 4 < 2) ? 0.92 : 1.0;
        // Weft threads (vertical emphasis on even cols)
        const weft = (x % 4 < 2) ? 0.94 : 1.0;
        const thread = warp * weft;
        // Slight randomness for organic feel
        const jitter = 0.97 + Math.random() * 0.06;
        const val = Math.floor(thread * jitter * 255);
        wd[i] = val; wd[i+1] = val; wd[i+2] = val;
        wd[i+3] = Math.floor(weaveOpacity * 255);
      }
    }
    // Apply weave as multiply
    const tempCanvas = document.createElement('canvas');
    tempCanvas.width = W; tempCanvas.height = H;
    const tctx = tempCanvas.getContext('2d');
    tctx.putImageData(weave, 0, 0);
    ctx.globalCompositeOperation = 'multiply';
    ctx.drawImage(tempCanvas, 0, 0);

    // 4. Fine grain noise
    ctx.globalCompositeOperation = 'overlay';
    const grain = ctx.createImageData(W, H);
    const gd = grain.data;
    for (let i = 0; i < gd.length; i += 4) {
      const v = Math.random() * 255;
      gd[i] = v; gd[i+1] = v; gd[i+2] = v;
      gd[i+3] = Math.floor(grainOpacity * 255);
    }
    const grainCanvas = document.createElement('canvas');
    grainCanvas.width = W; grainCanvas.height = H;
    grainCanvas.getContext('2d').putImageData(grain, 0, 0);
    ctx.drawImage(grainCanvas, 0, 0);

    ctx.globalCompositeOperation = 'source-over';
  };

  if (img.complete) render();
  else img.onload = render;
}
```

**Usage:**
```html
<canvas id="plate"></canvas>
<script>
  westernPlate('/photos/landscape.jpg', document.getElementById('plate'));
</script>
```

### Figure Labels

Always label images with textbook-style figure references:

```html
<figure class="western-plate">
  <img src="landscape.jpg" alt="Mountain vista">
  <figcaption><span class="fig-num">fig 001</span> — The eastern approach, circa 1872</figcaption>
</figure>
```

```css
figcaption {
  font-family: var(--font-sans);
  font-size: 0.6875rem;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--ink-60);
  margin-top: var(--space-sm);
}
.fig-num {
  font-family: var(--font-mono);
  color: var(--ink-30);
}
```

---

## Component Recipes

### Navigation

```css
.nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-md) var(--gutter);
  background: var(--parchment);
  border-bottom: 1px solid var(--warm-gray-light);
  position: sticky;
  top: 0;
  z-index: 100;
  backdrop-filter: blur(8px);
  background: rgba(255, 254, 242, 0.8);
}
.nav-logo {
  font-family: var(--font-display);
  font-size: 1.25rem;
  font-weight: 400;
  letter-spacing: -0.03em;
  color: var(--ink);
  text-decoration: none;
}
.nav-links {
  display: flex;
  gap: var(--space-lg);
  list-style: none;
}
.nav-link {
  font-family: var(--font-body);
  font-size: 0.75rem;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--ink);
  text-decoration: none;
  transition: color 0.2s;
}
.nav-link:hover { color: var(--ink-60); }
.nav-link.active { text-decoration: underline; text-underline-offset: 4px; }
```

### Buttons

```css
/* Primary CTA — amber */
.btn-primary {
  display: inline-flex;
  align-items: center;
  gap: 0.5em;
  padding: 0.75em 1.5em;
  background: var(--amber);
  color: var(--ink);
  font-family: var(--font-body);
  font-size: 0.75rem;
  font-weight: 400;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  text-decoration: none;
  border: none;
  cursor: pointer;
  transition: background 0.2s;
}
.btn-primary:hover { background: var(--amber-hover); }
.btn-primary .arrow { font-size: 0.9em; }

/* Secondary CTA — dark */
.btn-secondary {
  display: inline-flex;
  align-items: center;
  gap: 0.5em;
  padding: 0.75em 1.5em;
  background: var(--obsidian);
  color: var(--parchment);
  font-family: var(--font-body);
  font-size: 0.75rem;
  font-weight: 400;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  text-decoration: none;
  border: none;
  cursor: pointer;
}

/* Ghost CTA — outlined */
.btn-ghost {
  display: inline-flex;
  align-items: center;
  gap: 0.5em;
  padding: 0.75em 1.5em;
  background: transparent;
  color: var(--ink);
  font-family: var(--font-body);
  font-size: 0.75rem;
  font-weight: 400;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  text-decoration: none;
  border: 1px solid var(--ink);
  cursor: pointer;
}
```

**Arrow icon:** Use `↗` (U+2197) for external/CTA arrows: `<span class="arrow">↗</span>`

### Hero Section

```css
.hero {
  padding: var(--space-xxl) var(--gutter) var(--space-xl);
  max-width: 1200px;
  margin: 0 auto;
}
.hero h1 {
  font-family: var(--font-display);
  font-size: clamp(2.5rem, 5vw, 4rem);
  font-weight: 400;
  letter-spacing: -0.05em;
  line-height: 1.1;
  color: var(--ink);
  font-variation-settings: 'WONK' 1, 'opsz' 72;
  max-width: 10em; /* Keep headlines from stretching too wide */
}
.hero .subtitle {
  font-family: var(--font-body);
  font-size: clamp(1rem, 1.2vw, 1.125rem);
  color: var(--ink-60);
  line-height: 1.5;
  max-width: 32em;
  margin-top: var(--space-md);
}
.hero .cta-group {
  display: flex;
  gap: var(--space-sm);
  margin-top: var(--space-lg);
}
```

### Accent Italic Text

For emphasis words styled like Relace's "minutes" treatment:

```css
.accent-italic {
  font-style: italic;
  color: var(--amber-dark);
  font-variation-settings: 'WONK' 1;
}
```

```html
<h2>Get started in <span class="accent-italic">minutes</span></h2>
```

### Dark Cards (Pricing, Code Blocks)

```css
.dark-card {
  background: var(--obsidian);
  color: var(--parchment);
  padding: var(--space-lg);
  border: 1px solid var(--obsidian);
}
.dark-card h3 {
  font-family: var(--font-mono);
  font-size: 1rem;
  font-weight: 400;
  color: var(--parchment);
  margin-bottom: var(--space-md);
}
.dark-card .price {
  font-family: var(--font-display);
  font-size: 2rem;
  font-weight: 400;
  letter-spacing: -0.03em;
}
.dark-card .price-unit {
  font-family: var(--font-body);
  font-size: 0.875rem;
  color: rgba(255, 254, 242, 0.6);
}
```

### Feature Sections

Two-column layout: text left, visualization right.

```css
.feature-section {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--space-xl);
  align-items: center;
  padding: var(--space-xl) var(--gutter);
  max-width: 1200px;
  margin: 0 auto;
}
.feature-text h2 {
  font-family: var(--font-display);
  font-size: clamp(1.8rem, 3vw, 2.25rem);
  font-weight: 400;
  letter-spacing: -0.04em;
  line-height: 1.1;
  margin-bottom: var(--space-md);
}
.feature-text p {
  color: var(--ink-60);
  max-width: 28em;
}

@media (max-width: 768px) {
  .feature-section { grid-template-columns: 1fr; }
}
```

### Tags / Category Labels

```css
.tag {
  display: inline-block;
  padding: 0.35em 0.8em;
  background: var(--obsidian);
  color: var(--parchment);
  font-family: var(--font-body);
  font-size: 0.6875rem;
  font-weight: 500;
  letter-spacing: 0.04em;
  text-transform: uppercase;
}
.tag.amber {
  background: var(--amber);
  color: var(--ink);
}
```

### Decorative Curves

Thin amber curved lines used as section decoration:

```css
.deco-curve {
  position: absolute;
  pointer-events: none;
  z-index: 0;
}
.deco-curve svg {
  width: 100%;
  height: 100%;
}
.deco-curve path {
  fill: none;
  stroke: var(--amber);
  stroke-width: 1.5;
  opacity: 0.3;
}
```

```html
<!-- Example: sweeping curve behind content -->
<div class="deco-curve" style="top: -10%; right: -5%; width: 60%; height: 80%;">
  <svg viewBox="0 0 400 400" preserveAspectRatio="none">
    <path d="M 50 380 Q 200 50, 380 200 Q 300 350, 150 250" />
  </svg>
</div>
```

### Footer

```css
.footer {
  padding: var(--space-lg) var(--gutter);
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid var(--warm-gray-light);
  max-width: 1200px;
  margin: 0 auto;
}
.footer-links {
  display: flex;
  gap: var(--space-lg);
}
.footer-link {
  font-family: var(--font-body);
  font-size: 0.75rem;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--ink);
  text-decoration: none;
}
.footer-copy {
  font-size: 0.75rem;
  color: var(--ink-30);
}
```

### Landscape Banner (Footer / Section Break)

Full-width old-textbook treated landscape as a section divider:

```html
<div class="landscape-banner western-plate">
  <img src="western-landscape.jpg" alt="Mountain range at sunset">
</div>
```

```css
.landscape-banner {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  aspect-ratio: 3 / 1;
  overflow: hidden;
}
.landscape-banner img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```

---

## Layout Patterns

### Page Structure
```
┌─ nav (sticky, blur backdrop) ──────────────────────────┐
│ Logo          DOCS  BLOG  PRICING  ABOUT      APP  CTA │
├────────────────────────────────────────────────────────┤
│                                                         │
│  [200px top padding]                                    │
│                                                         │
│  Display Heading           Subtitle paragraph           │
│  tight-tracked             body text, muted             │
│                            color, max-width 32em        │
│  [CTA] [Ghost CTA]                                     │
│                                                         │
│  ┌─ Hero Image ──────────────────────────────┐         │
│  │  (western-plate treated landscape)         │         │
│  │  with optional dark code terminal overlay  │         │
│  └────────────────────────────────────────────┘         │
│  fig 001 — descriptive caption                          │
│                                                         │
│  ─────────── warm gray rule ──────────────              │
│                                                         │
│  [96px section padding]                                 │
│                                                         │
│  Section Heading     │   Visualization/Chart/Image      │
│  body text           │   (treated with plate filter)    │
│                      │                                  │
│  ─────────── warm gray rule ──────────────              │
│                                                         │
│  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐                   │
│  │ dark │ │ dark │ │ dark │ │ dark │  ← pricing cards   │
│  │ card │ │ card │ │ card │ │ card │    sharp corners    │
│  └──────┘ └──────┘ └──────┘ └──────┘                    │
│                                                         │
│  ─────────── warm gray rule ──────────────              │
│                                                         │
│  Get started in *minutes*    [CTA ↗] [Ghost ↗]         │
│                                                         │
│  ┌─ landscape banner (plate-treated) ────────┐         │
│  └───────────────────────────────────────────┘         │
│                                                         │
│  Logo    LINK  LINK  LINK              © 2026           │
└────────────────────────────────────────────────────────┘
```

### Key Layout Rules
1. **Max-width 1200px**, centered. Never full-bleed text.
2. **Two-column features:** Left text, right visual. Alternate sides for variety.
3. **Dark cards in 4-column grid** for pricing/feature comparisons.
4. **Landscape banners** as section separators and footer treatments.
5. **Decorative curves** (thin amber SVG arcs) placed behind content sections for visual interest.

---

## Imagery Guidelines

### Subject Matter
- **19th century American West landscape paintings** (Albert Bierstadt, Thomas Moran, Frederic Church style)
- Mountain vistas, canyons, prairies, desert formations
- Golden hour light, dramatic skies, frontier wilderness
- The kind of scene that would appear as a lithograph plate in a geological survey report

### Treatment
- ALL photographic/painted images get the `western-plate` filter treatment
- Use figure labels (`fig 001`) for standalone images
- Landscape images should be wide aspect ratio (3:1 or wider for banners)
- Portrait/detail images: keep modest size, let whitespace breathe around them

### Overlays
- Code terminals / dark panels can be overlaid on landscape images
- Use thin geometric lines (circles, arcs) over images for a "diagram annotation" feel — like a cartographer marking features on a survey plate

---

## Anti-Patterns — What NOT to Do

| ❌ Don't | ✅ Do |
|----------|-------|
| Border-radius on ANYTHING | Sharp corners everywhere, 0px always |
| Blue/purple/red accent colors | Amber `#FCAA2D` only |
| Pure white `#FFFFFF` background | Warm parchment `#FFFEF2` |
| Pure black `#000000` text | Near-black `#191918` |
| Bold headings (weight 700+) | Weight 400 for headings, 500 max for labels |
| Tight body text spacing | Only tighten display headings, leave body normal |
| Sans-serif headings | Calligraphic serif (Fraunces) always for headings |
| Stock photos of people/offices | Western landscape paintings, textbook plates |
| Gradient backgrounds | Flat parchment, never gradients |
| Box shadows on cards | Borders only (1px warm-gray), or no border at all |
| Small padding between sections | Minimum 96px section padding, 200px hero |
| Lots of colors | Monochrome parchment + ink, amber accent only |
| Rounded pill buttons | Rectangular buttons, no radius |
| Lowercase nav labels | Uppercase + wide letter-spacing for all labels/nav |

---

## Quick Start Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Western UI</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght,WONK@0,9..144,100..900,0..1;1,9..144,100..900,0..1&family=Karla:wght@400;500&family=IBM+Plex+Mono:wght@400&display=swap" rel="stylesheet">
  <style>
    :root {
      --parchment: #FFFEF2; --parchment-mid: #F3F2E7;
      --ink: #191918; --ink-60: rgba(25,25,24,0.6); --ink-30: rgba(25,25,24,0.3);
      --amber: #FCAA2D; --amber-dark: #A36404; --amber-hover: #E89A20;
      --warm-gray: #BAB9B0; --warm-gray-light: rgba(209,208,198,0.4);
      --obsidian: #191918;
      --font-display: 'Fraunces', Georgia, serif;
      --font-body: 'Fraunces', Georgia, serif;
      --font-sans: 'Karla', system-ui, sans-serif;
      --font-mono: 'IBM Plex Mono', monospace;
    }
    *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; border-radius: 0 !important; }
    body { background: var(--parchment); color: var(--ink); font-family: var(--font-body); font-size: 18px; line-height: 1.5; }
    h1, h2, h3 { font-family: var(--font-display); font-weight: 400; font-variation-settings: 'WONK' 1; }
    h1 { font-size: clamp(2.5rem, 5vw, 4rem); letter-spacing: -0.05em; line-height: 1.1; }
    h2 { font-size: clamp(1.8rem, 3vw, 2.25rem); letter-spacing: -0.04em; line-height: 1.1; }
    /* ... add remaining styles per recipes above ... */
  </style>
</head>
<body>
  <nav class="nav"><!-- ... --></nav>
  <main>
    <section class="hero">
      <h1>Your Heading Here</h1>
      <p class="subtitle">Descriptive subtitle in muted ink.</p>
      <div class="cta-group">
        <a href="#" class="btn-primary">Get Started <span class="arrow">↗</span></a>
        <a href="#" class="btn-ghost">Learn More <span class="arrow">↗</span></a>
      </div>
    </section>
  </main>
</body>
</html>
```

---

## Accessibility Notes

- Parchment/ink contrast ratio: ~18:1 (exceeds WCAG AAA)
- Amber on parchment: ~2.8:1 — **fails** for small text. Use amber ONLY for large buttons/decorative text, never body text.
- Amber-dark (#A36404) on parchment: ~5.2:1 — passes AA for normal text.
- Dark cards (parchment on obsidian): ~18:1 (exceeds AAA)
- Always provide text alternatives for decorative landscape images
- Ensure figure labels are associated with images via `<figcaption>` within `<figure>`
