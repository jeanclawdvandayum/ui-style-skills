---
name: blueprint-ui
description: Technical schematic / engineering blueprint aesthetic for web interfaces — cyan lines on dark navy backgrounds, circuit diagram patterns, grid-paper overlays, precise geometric construction, annotation callouts, dimension lines, component symbols, and the precise elegance of engineering drawings. Also covers isometric/axonometric technical illustration variant with blue grid paper.
---

# Blueprint UI — Technical Schematic Aesthetic

Build web interfaces that feel like they were drawn on a drafting table — precise cyan lines on dark navy, circuit-board trace patterns, grid-paper backgrounds, annotation callouts with leader lines, dimension markers, and the clinical beauty of engineering documentation.

---

## Design Tokens

### Color Palette

```css
:root {
  /* Surfaces */
  --navy:           #0A0E1A;  /* Primary background — deep navy */
  --navy-mid:       #111828;  /* Raised surface */
  --navy-light:     #1A2340;  /* Elevated panels */

  /* Blueprint lines */
  --cyan:           #00C8FF;  /* Primary line color */
  --cyan-dim:       #0088AA;  /* Secondary/construction lines */
  --cyan-glow:      rgba(0, 200, 255, 0.1);  /* Subtle glow */
  --cyan-faint:     rgba(0, 200, 255, 0.06); /* Grid lines */

  /* Annotation */
  --white:          #E8EAF0;  /* Primary text */
  --white-dim:      rgba(232, 234, 240, 0.5);  /* Secondary text */
  --white-faint:    rgba(232, 234, 240, 0.15); /* Tertiary */

  /* Status colors */
  --signal-green:   #00FF88;  /* Active/connected */
  --signal-red:     #FF4466;  /* Error/disconnected */
  --signal-amber:   #FFAA00;  /* Warning/pending */
}
```

**Rules:**
- Background is ALWAYS dark navy. Never light.
- Cyan is the ONLY primary accent. Everything else is white at varying opacities.
- Status colors (green/red/amber) are for data states only, never decoration.
- Lines and borders use `--cyan-dim` or `--cyan`. Never white borders.

### Typography

```css
:root {
  --font-tech:  'Space Mono', 'IBM Plex Mono', monospace;  /* Primary — everything */
  --font-label: 'Inter', system-ui, sans-serif;              /* Annotations, body */
}
```

```html
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Inter:wght@400;500&display=swap" rel="stylesheet">
```

**Monospace dominates.** This aesthetic is 80% mono. Sans-serif only for longer annotations.

### Type Scale

| Role | Font | Size | Weight | Spacing | Transform |
|------|------|------|--------|---------|-----------|
| Title | tech | `1.5rem` | 700 | `0.15em` | `uppercase` |
| Section | tech | `1rem` | 700 | `0.12em` | `uppercase` |
| Label | tech | `0.625rem` | 400 | `0.1em` | `uppercase` |
| Value | tech | `1.125rem` | 400 | `0em` | `none` |
| Annotation | label | `0.75rem` | 400 | `0.02em` | `none` |
| Dimension | tech | `0.5625rem` | 400 | `0.05em` | `none` |

---

## Patterns

### Grid Paper Background

```css
.grid-paper {
  background-color: var(--navy);
  background-image:
    linear-gradient(var(--cyan-faint) 1px, transparent 1px),
    linear-gradient(90deg, var(--cyan-faint) 1px, transparent 1px),
    linear-gradient(rgba(0,200,255,0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0,200,255,0.03) 1px, transparent 1px);
  background-size: 80px 80px, 80px 80px, 16px 16px, 16px 16px;
}
```

### Circuit Trace Lines

```css
/* Horizontal/vertical circuit traces */
.trace-h { border-top: 1px solid var(--cyan-dim); }
.trace-v { border-left: 1px solid var(--cyan-dim); }

/* Trace node (junction point) */
.trace-node {
  width: 8px; height: 8px;
  border-radius: 50% !important;
  background: var(--cyan);
  box-shadow: 0 0 6px var(--cyan-glow);
}

/* PCB-style trace pattern (decorative) */
.pcb-traces {
  background-image:
    linear-gradient(0deg, transparent 49.5%, var(--cyan-faint) 49.5%, var(--cyan-faint) 50.5%, transparent 50.5%),
    linear-gradient(90deg, transparent 49.5%, var(--cyan-faint) 49.5%, var(--cyan-faint) 50.5%, transparent 50.5%);
  background-size: 40px 40px;
}
```

### Dot Grid (Isometric Variant)

```css
.dot-grid-iso {
  background-image: radial-gradient(circle, var(--cyan-dim) 1px, transparent 1px);
  background-size: 20px 20px;
}
```

---

## Components

### Annotation Callout

```css
.callout {
  position: relative;
  padding: 0.5rem 0.8rem;
  border: 1px solid var(--cyan-dim);
  background: rgba(0, 200, 255, 0.04);
  font-family: var(--font-label);
  font-size: 0.75rem;
  color: var(--white);
}
/* Leader line — CSS triangle pointing to target */
.callout::before {
  content: '';
  position: absolute;
  bottom: -8px; left: 1rem;
  width: 1px; height: 8px;
  background: var(--cyan-dim);
}
.callout::after {
  content: '';
  position: absolute;
  bottom: -12px; left: calc(1rem - 2px);
  width: 5px; height: 5px;
  border-radius: 50% !important;
  background: var(--cyan);
}
```

### Dimension Line

```css
.dimension {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-family: var(--font-tech);
  font-size: 0.5625rem;
  letter-spacing: 0.05em;
  color: var(--cyan-dim);
}
.dimension::before, .dimension::after {
  content: '';
  flex: 1;
  height: 1px;
  background: var(--cyan-dim);
}
.dimension-val { color: var(--cyan); white-space: nowrap; }
```

### Component Symbol Card

```css
.component-card {
  background: var(--navy-mid);
  border: 1px solid var(--cyan-dim);
  padding: 1.2rem;
  position: relative;
}
.component-card::before {
  content: attr(data-ref);
  position: absolute;
  top: -0.5em; right: 0.8rem;
  font-family: var(--font-tech);
  font-size: 0.5625rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--cyan);
  background: var(--navy);
  padding: 0 0.3em;
}
.component-card h3 {
  font-family: var(--font-tech);
  font-size: 0.875rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--cyan);
  margin-bottom: 0.5rem;
}
```

```html
<div class="component-card" data-ref="C-001">
  <h3>Module Name</h3>
  <p>Description text here</p>
</div>
```

### Status Indicators

```css
.status-dot {
  display: inline-flex;
  align-items: center;
  gap: 0.4em;
  font-family: var(--font-tech);
  font-size: 0.625rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}
.status-dot::before {
  content: '';
  width: 6px; height: 6px;
  border-radius: 50% !important;
  background: var(--signal-green);
  box-shadow: 0 0 6px rgba(0,255,136,0.3);
}
.status-dot.error::before { background: var(--signal-red); box-shadow: 0 0 6px rgba(255,68,102,0.3); }
.status-dot.pending::before { background: var(--signal-amber); box-shadow: 0 0 6px rgba(255,170,0,0.3); }
```

### Data Table

```css
.schema-table {
  width: 100%;
  border-collapse: collapse;
  font-family: var(--font-tech);
  font-size: 0.8125rem;
}
.schema-table th {
  font-size: 0.625rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  text-align: left;
  padding: 0.6rem 0.8rem;
  border-bottom: 1px solid var(--cyan);
  color: var(--cyan);
  font-weight: 400;
}
.schema-table td {
  padding: 0.5rem 0.8rem;
  border-bottom: 1px solid rgba(0,200,255,0.08);
  color: var(--white);
}
.schema-table tr:hover td { background: var(--cyan-glow); }
```

### Navigation

```css
.bp-nav {
  display: flex;
  align-items: center;
  padding: 0.6rem 1.2rem;
  background: var(--navy-mid);
  border-bottom: 1px solid var(--cyan-dim);
  gap: 2rem;
}
.bp-nav-link {
  font-family: var(--font-tech);
  font-size: 0.625rem;
  font-weight: 400;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--white-dim);
  text-decoration: none;
  transition: color 0.15s;
}
.bp-nav-link:hover, .bp-nav-link.active { color: var(--cyan); }
```

### Buttons

```css
.bp-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.4em;
  padding: 0.5em 1.2em;
  font-family: var(--font-tech);
  font-size: 0.6875rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  border: 1px solid var(--cyan-dim);
  background: transparent;
  color: var(--cyan);
  cursor: pointer;
  transition: all 0.15s;
  text-decoration: none;
}
.bp-btn:hover { background: var(--cyan-glow); border-color: var(--cyan); }
.bp-btn.filled { background: var(--cyan); color: var(--navy); border-color: var(--cyan); }
.bp-btn.filled:hover { background: var(--cyan-dim); }
```

---

## Layout

### Schematic Canvas

```css
.schematic {
  min-height: 100vh;
  padding: 2rem;
  position: relative;
}
/* Everything sits on the grid paper */
body.blueprint { background: var(--navy); }
body.blueprint .schematic { @extend .grid-paper; }
```

### Panel Layout

```css
.bp-panels {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1.5rem;
}
```

---

## Anti-Patterns

| ❌ Don't | ✅ Do |
|----------|-------|
| Light backgrounds | Dark navy always |
| Multiple hue accents | Cyan only (status colors excepted) |
| Serif or display fonts | Monospace for everything |
| Rounded corners on panels | Sharp corners (round dots only) |
| Thick borders (>2px) | Thin 1px lines |
| Bright glows (neon) | Subtle 10% opacity glows |
| Casual/friendly copy | Technical, precise, annotated |

---

## Accessibility

- Cyan `#00C8FF` on navy `#0A0E1A`: 7.8:1 ✅ (passes AAA)
- White `#E8EAF0` on navy: 14.5:1 ✅
- Grid lines must not interfere with readability — keep at 3-6% opacity
- Signal colors need labels, not just color (colorblind consideration)
