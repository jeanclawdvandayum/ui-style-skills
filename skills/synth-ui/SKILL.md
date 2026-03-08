---
name: synth-ui
description: Dark modular synthesizer aesthetic for web interfaces — near-black backgrounds, teal/green accent lighting, knob and slider UI metaphors, LED indicator dots, patch cable connection lines, oscilloscope waveforms, amber warm-analog variant, VU meters, dense control panel layouts. Inspired by Eurorack modules and vintage audio equipment.
---

# Synth UI — Modular Dark Interface

Build web interfaces that feel like the faceplate of a Eurorack synthesizer module or a vintage oscilloscope. Near-black backgrounds lit by precise accent glows, dense control layouts, LED indicator dots, waveform visualizations, and the satisfying precision of knobs and faders.

Two variants: **Cold (teal/green digital)** and **Warm (amber/gold analog)**.

---

## Design Tokens

### Color Palette

```css
:root {
  /* Surfaces */
  --panel:          #0D0D0D;  /* Primary background — near-black */
  --panel-raised:   #1A1A1A;  /* Raised panel/module surface */
  --panel-inset:    #080808;  /* Inset/recessed areas */
  --bezel:          #2A2A2A;  /* Panel borders, bezels */

  /* Cold variant (digital/teal) */
  --cold-primary:   #00E5A0;  /* Teal-green — primary accent */
  --cold-dim:       #00B87D;  /* Dimmed state */
  --cold-glow:      rgba(0, 229, 160, 0.15);  /* Glow halo */
  --cold-text:      #7DFFC8;  /* Light teal for readable text */

  /* Warm variant (analog/amber) */
  --warm-primary:   #FFB020;  /* Amber-gold — primary accent */
  --warm-dim:       #CC8D1A;  /* Dimmed state */
  --warm-glow:      rgba(255, 176, 32, 0.12);  /* Glow halo */
  --warm-text:      #FFD980;  /* Light amber for readable text */

  /* Shared accents */
  --led-red:        #FF3333;  /* Alert/clip LED */
  --led-green:      #33FF66;  /* OK/active LED */
  --led-off:        #333333;  /* Inactive LED */
  --text-primary:   #E0E0E0;  /* Primary text */
  --text-muted:     #707070;  /* Secondary labels */
  --text-dim:       #404040;  /* Tertiary/background labels */
}
```

**Rules:**
- Background is ALWAYS dark (`--panel` or `--panel-raised`). Never light mode.
- Choose ONE accent variant per interface: cold OR warm. Don't mix.
- Accent color is for highlights, active states, and key data. Most UI stays grayscale.
- Glows are subtle — 10-15% opacity max. The effect is "backlit," not "neon."

### Typography

```css
:root {
  --font-display: 'Space Grotesk', system-ui, sans-serif;    /* Module names */
  --font-mono:    'IBM Plex Mono', 'Courier New', monospace;  /* Values, labels */
  --font-body:    'Inter', system-ui, sans-serif;              /* Body text */
}
```

```html
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=IBM+Plex+Mono:wght@400;500;600&family=Inter:wght@400;500&display=swap" rel="stylesheet">
```

### Type Scale

| Role | Font | Size | Weight | Spacing | Transform |
|------|------|------|--------|---------|-----------|
| Module name | display | `1.125rem` | 700 | `-0.02em` | `uppercase` |
| Parameter label | mono | `0.625rem` | 500 | `0.1em` | `uppercase` |
| Value readout | mono | `1.5rem` | 600 | `-0.02em` | `none` |
| Small value | mono | `0.875rem` | 500 | `0em` | `none` |
| Body | body | `0.875rem` | 400 | `normal` | `none` |
| Section label | mono | `0.5625rem` | 400 | `0.12em` | `uppercase` |

---

## Component Recipes

### Module Panel

The fundamental building block — a rectangular control panel:

```css
.module {
  background: var(--panel-raised);
  border: 1px solid var(--bezel);
  padding: 1rem;
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
}
.module-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 0.5rem;
  border-bottom: 1px solid var(--bezel);
}
.module-name {
  font-family: var(--font-display);
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text-primary);
}
```

### LED Indicators

```css
.led {
  display: inline-block;
  width: 8px; height: 8px;
  border-radius: 50% !important; /* Exception: LEDs are round */
  background: var(--led-off);
  transition: all 0.15s;
}
.led.on { background: var(--cold-primary); box-shadow: 0 0 6px var(--cold-glow), 0 0 12px var(--cold-glow); }
.led.warm.on { background: var(--warm-primary); box-shadow: 0 0 6px var(--warm-glow), 0 0 12px var(--warm-glow); }
.led.red.on { background: var(--led-red); box-shadow: 0 0 6px rgba(255,51,51,0.3); }

/* LED row (level meter) */
.led-row { display: flex; gap: 3px; }
.led-row .led { width: 6px; height: 12px; border-radius: 1px !important; }
```

### Knob Control

```css
.knob {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.4rem;
}
.knob-dial {
  width: 48px; height: 48px;
  border-radius: 50% !important;
  background: conic-gradient(from 220deg, var(--bezel) 0deg, #444 180deg, var(--bezel) 280deg);
  border: 2px solid #444;
  position: relative;
  cursor: pointer;
}
.knob-dial::after {
  content: '';
  position: absolute;
  top: 4px; left: 50%;
  width: 2px; height: 10px;
  background: var(--text-primary);
  transform: translateX(-50%);
  border-radius: 1px !important;
}
.knob-label {
  font-family: var(--font-mono);
  font-size: 0.5625rem;
  font-weight: 500;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text-muted);
}
```

### Fader / Slider

```css
.fader-track {
  width: 4px; height: 120px;
  background: var(--panel-inset);
  border: 1px solid var(--bezel);
  position: relative;
  margin: 0 auto;
}
.fader-fill {
  position: absolute;
  bottom: 0; left: -1px; right: -1px;
  background: var(--cold-primary);
  transition: height 0.1s;
  box-shadow: 0 0 8px var(--cold-glow);
}
.fader-thumb {
  width: 20px; height: 8px;
  background: #555;
  border: 1px solid #777;
  position: absolute;
  left: 50%; transform: translateX(-50%);
  cursor: grab;
}
```

### Value Readout (Digital Display)

```css
.readout {
  font-family: var(--font-mono);
  font-size: 1.5rem;
  font-weight: 600;
  letter-spacing: -0.02em;
  color: var(--cold-primary);
  background: var(--panel-inset);
  border: 1px solid var(--bezel);
  padding: 0.4rem 0.8rem;
  text-align: right;
  text-shadow: 0 0 8px var(--cold-glow);
}
.readout.warm { color: var(--warm-primary); text-shadow: 0 0 8px var(--warm-glow); }
```

### Oscilloscope / Waveform Display

```css
.scope {
  background: var(--panel-inset);
  border: 1px solid var(--bezel);
  padding: 0.5rem;
  position: relative;
  overflow: hidden;
}
/* Grid overlay */
.scope::before {
  content: '';
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(var(--bezel) 1px, transparent 1px),
    linear-gradient(90deg, var(--bezel) 1px, transparent 1px);
  background-size: 20% 20%;
  opacity: 0.3;
}
.scope canvas { display: block; width: 100%; height: 100%; position: relative; z-index: 1; }
```

### Patch Cable Connection Lines

```css
/* SVG line between two elements — use with JS positioning */
.patch-line {
  stroke: var(--cold-primary);
  stroke-width: 2;
  fill: none;
  filter: drop-shadow(0 0 3px var(--cold-glow));
}
/* Patch point (jack socket) */
.jack {
  width: 16px; height: 16px;
  border-radius: 50% !important;
  background: var(--panel-inset);
  border: 2px solid #555;
  cursor: pointer;
  transition: border-color 0.15s;
}
.jack:hover { border-color: var(--cold-primary); }
.jack.connected { border-color: var(--cold-primary); box-shadow: 0 0 4px var(--cold-glow); }
```

### VU Meter (Warm Variant)

```css
.vu-meter {
  display: flex;
  gap: 2px;
  align-items: flex-end;
  height: 60px;
  padding: 4px;
  background: var(--panel-inset);
  border: 1px solid var(--bezel);
}
.vu-bar {
  width: 6px;
  background: var(--warm-primary);
  transition: height 0.1s;
  box-shadow: 0 0 4px var(--warm-glow);
}
.vu-bar.clip { background: var(--led-red); }
```

---

## Layout Patterns

### Control Panel Grid

```css
.synth-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 2px;
  background: var(--panel);
  padding: 2px;
}
```

### Rack Layout (Fixed-Width Modules)

```css
.rack {
  display: flex;
  gap: 2px;
  overflow-x: auto;
  background: var(--panel);
  padding: 2px;
}
.rack .module {
  min-width: 160px;
  flex-shrink: 0;
}
```

---

## Anti-Patterns

| ❌ Don't | ✅ Do |
|----------|-------|
| Light/white backgrounds | Dark `#0D0D0D` always |
| Bright neon glows (>20% opacity) | Subtle backlit glows (10-15%) |
| Multiple accent colors | One variant: cold teal OR warm amber |
| Rounded-everything (pill shapes) | Sharp panels + round LEDs/knobs only |
| Large padding/whitespace | Dense, tight control layouts |
| Decorative fonts | Geometric sans + monospace only |
| Drop shadows | Inner glows and border highlights |

---

## Accessibility

- Text on dark: ensure `--text-primary` (#E0E0E0) on `--panel` (#0D0D0D) = 15:1 ✅
- Accent on dark: `--cold-primary` on `--panel` = 8.5:1 ✅ (passes AAA)
- Warm accent: `--warm-primary` on `--panel` = 8.2:1 ✅
- LED indicators must not be color-only; pair with labels or position changes
- Dense layouts need keyboard navigation with visible focus rings (use accent color glow)
