---
name: graffiti-ui
description: Neon street art / graffiti maximalism for web interfaces — explosive neon colors (cyan, magenta, hot yellow) on dark backgrounds, paint splatter textures, drip effects, layered chaos, hand-drawn energy, sticker/tag UI elements, spray-paint gradients, and the raw expressive power of urban art. Anti-minimal. Maximum energy.
---

# Graffiti UI — Neon Street Art Maximalism

Build web interfaces that look like they were bombed onto a wall at 3am. Neon splashes of cyan, magenta, and acid yellow on grimy dark backgrounds. Paint splatters as decorative elements. Sticker-style labels. Spray-paint gradients. Layered, chaotic, alive. The opposite of every clean SaaS template.

**This is anti-minimalism. If it looks neat, you're doing it wrong.**

---

## Design Tokens

### Color Palette

```css
:root {
  /* Backgrounds */
  --wall:           #0C0C0C;  /* Primary — dark concrete */
  --wall-warm:      #1A1410;  /* Warm dark variant */
  --wall-texture:   #161616;  /* Slightly lighter for texture */

  /* Neon spray colors — THE CORE */
  --neon-cyan:      #00F0FF;  /* Electric cyan */
  --neon-magenta:   #FF2EAA;  /* Hot magenta/pink */
  --neon-yellow:    #E8FF00;  /* Acid yellow-green */
  --neon-orange:    #FF6B1A;  /* Spray orange */

  /* Support */
  --chrome:         #E0E0E0;  /* Clean white text */
  --chrome-dim:     #888888;  /* Muted text */
  --drip:           rgba(255, 255, 255, 0.06);  /* Paint drip overlay */
}
```

**Rules:**
- Use 2-3 neon colors max per composition. All 4 at once = mud.
- Colors should feel CLASHING and energetic, not harmonized.
- Neon on dark only. Never neon on white (unreadable and ugly).
- One color dominates (~60%), one supports (~30%), one pops (~10%).

### Typography

```css
:root {
  --font-tag:    'Permanent Marker', 'Marker Felt', cursive;  /* Graffiti tags */
  --font-block:  'Archivo Black', 'Impact', sans-serif;        /* Block letters */
  --font-body:   'Space Grotesk', system-ui, sans-serif;       /* Readable body */
  --font-mono:   'IBM Plex Mono', monospace;                    /* Labels/codes */
}
```

```html
<link href="https://fonts.googleapis.com/css2?family=Permanent+Marker&family=Archivo+Black&family=Space+Grotesk:wght@400;700&family=IBM+Plex+Mono:wght@400;600&display=swap" rel="stylesheet">
```

### Type Scale

| Role | Font | Size | Weight | Notes |
|------|------|------|--------|-------|
| Hero tag | tag | `clamp(4rem, 12vw, 10rem)` | 400 | Rotated slightly (-2° to 3°) |
| Block headline | block | `clamp(2.5rem, 6vw, 5rem)` | 400 | Uppercase, tight |
| Sub-tag | tag | `clamp(1.5rem, 3vw, 2.5rem)` | 400 | Can overlap other elements |
| Body | body | `1rem` | 400 | Clean and readable |
| Sticker label | mono | `0.6875rem` | 600 | Uppercase, wide spacing |

---

## Patterns & Textures

### Paint Splatter (CSS)

```css
/* Splatter dots — scattered radial gradients */
.splatter::before {
  content: '';
  position: absolute;
  inset: 0;
  background:
    radial-gradient(circle at 15% 20%, var(--neon-cyan) 0%, transparent 3%),
    radial-gradient(circle at 72% 35%, var(--neon-magenta) 0%, transparent 2.5%),
    radial-gradient(circle at 45% 80%, var(--neon-yellow) 0%, transparent 4%),
    radial-gradient(circle at 88% 65%, var(--neon-cyan) 0%, transparent 1.8%),
    radial-gradient(circle at 25% 55%, var(--neon-magenta) 0%, transparent 3.5%),
    radial-gradient(circle at 60% 15%, var(--neon-orange) 0%, transparent 2%);
  opacity: 0.5;
  pointer-events: none;
  z-index: 0;
}
```

### Spray Paint Gradient

```css
.spray-gradient {
  background: radial-gradient(
    ellipse at 30% 40%,
    rgba(0, 240, 255, 0.25) 0%,
    rgba(255, 46, 170, 0.15) 40%,
    transparent 70%
  );
}
```

### Drip Effect

```css
.drip-border {
  position: relative;
  overflow: visible;
}
.drip-border::after {
  content: '';
  position: absolute;
  bottom: -20px;
  left: 10%;
  width: 4px;
  height: 20px;
  background: var(--neon-cyan);
  border-radius: 0 0 2px 2px !important;
  filter: blur(0.5px);
  box-shadow: 
    30px 5px 0 var(--neon-magenta),
    65px -3px 0 var(--neon-cyan),
    120px 8px 0 var(--neon-yellow);
}
```

### Noise / Grain Texture

```css
.wall-texture {
  position: relative;
}
.wall-texture::before {
  content: '';
  position: absolute;
  inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.06'/%3E%3C/svg%3E");
  background-size: 256px;
  pointer-events: none;
  z-index: 0;
}
```

---

## Components

### Sticker Label

```css
.sticker {
  display: inline-block;
  padding: 0.4em 0.8em;
  font-family: var(--font-mono);
  font-size: 0.6875rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  background: var(--neon-yellow);
  color: var(--wall);
  transform: rotate(-1.5deg);
  box-shadow: 2px 2px 0 rgba(0,0,0,0.4);
}
.sticker.cyan { background: var(--neon-cyan); }
.sticker.magenta { background: var(--neon-magenta); color: var(--chrome); }
.sticker.dark { background: var(--wall); color: var(--chrome); border: 2px solid var(--chrome); }
```

### Tag Text (Handwritten Headers)

```css
.tag-text {
  font-family: var(--font-tag);
  font-size: clamp(3rem, 8vw, 7rem);
  color: var(--neon-cyan);
  text-shadow:
    0 0 20px rgba(0, 240, 255, 0.4),
    0 0 40px rgba(0, 240, 255, 0.15);
  transform: rotate(-2deg);
  position: relative;
  z-index: 1;
}
```

### Block Letters

```css
.block-text {
  font-family: var(--font-block);
  font-size: clamp(2rem, 5vw, 4rem);
  text-transform: uppercase;
  letter-spacing: -0.02em;
  line-height: 0.95;
  color: var(--chrome);
  -webkit-text-stroke: 1px var(--chrome);
}
/* Outline variant */
.block-text.outline {
  color: transparent;
  -webkit-text-stroke: 2px var(--neon-magenta);
}
/* Fill variant */
.block-text.fill-cyan { color: var(--neon-cyan); -webkit-text-stroke: 0; }
```

### Neon Button

```css
.btn-neon {
  display: inline-flex;
  align-items: center;
  gap: 0.5em;
  padding: 0.7em 1.4em;
  font-family: var(--font-mono);
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  text-decoration: none;
  border: 2px solid var(--neon-cyan);
  background: transparent;
  color: var(--neon-cyan);
  cursor: pointer;
  transition: all 0.15s;
  position: relative;
}
.btn-neon:hover {
  background: var(--neon-cyan);
  color: var(--wall);
  box-shadow: 0 0 15px rgba(0,240,255,0.3), 0 0 30px rgba(0,240,255,0.1);
}
.btn-neon.magenta { border-color: var(--neon-magenta); color: var(--neon-magenta); }
.btn-neon.magenta:hover { background: var(--neon-magenta); color: var(--wall); box-shadow: 0 0 15px rgba(255,46,170,0.3); }
.btn-neon.solid { background: var(--neon-yellow); color: var(--wall); border-color: var(--neon-yellow); }
```

### Layered Chaos Card

```css
.chaos-card {
  background: var(--wall-texture);
  padding: 2rem;
  position: relative;
  overflow: hidden;
  border: none;
  transform: rotate(-0.5deg);
}
.chaos-card + .chaos-card { transform: rotate(0.8deg); margin-top: -0.5rem; }
```

---

## Layout

### Collage Grid (Overlapping)

```css
.collage {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 0.5rem;
  padding: 1rem;
}
.collage > * {
  position: relative;
  z-index: 1;
}
/* Overlap by shifting elements */
.collage .overlap-up { margin-top: -2rem; z-index: 2; }
.collage .overlap-left { margin-left: -1rem; z-index: 2; }
.collage .tilt-left { transform: rotate(-2deg); }
.collage .tilt-right { transform: rotate(1.5deg); }
```

### Composition Rules
1. **Nothing is perfectly aligned.** Slight rotations (±1-3°) on everything.
2. **Layers overlap.** Text sits on images. Stickers overlap borders. Tags cross grid lines.
3. **Scale is dramatic.** Hero text takes 50%+ of viewport. Then tiny sticker labels.
4. **Color bleeds.** Spray gradients extend beyond their containers.
5. **Negative space is dark.** The wall shows through between elements.

---

## Anti-Patterns

| ❌ Don't | ✅ Do |
|----------|-------|
| Clean, aligned grids | Overlapping, tilted, chaotic layouts |
| Muted/pastel colors | Full-saturation neon on dark |
| Serif or elegant fonts | Marker fonts + block sans + mono |
| Consistent spacing | Varied, organic, intentionally uneven |
| Light backgrounds | Dark concrete/wall backgrounds only |
| Harmonious palettes | Clashing neon combinations |
| Subtle hover effects | Glowing neon hover states |
| Perfect circles/shapes | Rough, hand-drawn energy |

---

## Accessibility

- Neon cyan on dark: 8.5:1 ✅ (but only at full saturation; dimmed variants need checking)
- Neon magenta on dark: 4.2:1 — **AA large text only.** Don't use for body text.
- Neon yellow on dark: 14.5:1 ✅ (best contrast of the neons)
- Chrome white on dark: 15:1 ✅
- Use `prefers-reduced-motion` to kill rotations and reduce glow intensity
- Ensure all rotated text is still selectable and screen-reader accessible
- Neon glows can trigger photosensitivity — offer `prefers-contrast: more` override

```css
@media (prefers-reduced-motion: reduce) {
  .sticker, .chaos-card, .tag-text { transform: none !important; }
}
@media (prefers-contrast: more) {
  :root { --neon-cyan: #FFFFFF; --neon-magenta: #FFFFFF; --neon-yellow: #FFFFFF; }
}
```
