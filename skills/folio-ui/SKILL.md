# folio-ui — Warm Editorial Technical Folio

A design system for single-file HTML pages inspired by premium technical portfolios and design studio documents. Warm parchment paper, high-contrast serif headlines, diagonal stripe section dividers, grid-line card layouts, and black isometric illustration style. The aesthetic of a beautifully typeset engineering manual printed on warm cotton paper.

## When to Use

- Landing pages, product pages, portfolio showcases
- Technical documentation with editorial quality
- Developer tool marketing pages
- Feature showcases with illustration-heavy layouts
- Any page that needs to feel premium, warm, and technically precise

## Palette

```css
:root {
  /* ── Surfaces ── */
  --bg:               #e4dcd0;   /* warm parchment — the foundation */
  --surface:          #f0eae2;   /* card interiors, elevated content */
  --surface-code:     #faf6f0;   /* code block backgrounds — near-white warm */

  /* ── Text ── */
  --text:             #1a1816;   /* warm near-black — primary text + accent */
  --text-dim:         #7a7470;   /* warm mid-gray — secondary text, captions */
  --text-inv:         #f0eae2;   /* inverted text on dark fills */

  /* ── Borders & Structure ── */
  --rule:             #2a2620;   /* dark warm — grid dividers, stripe pattern */
  --rule-light:       #c8c0b4;   /* lighter — subtle separators */

  /* ── Highlight ── */
  --highlight:        #f2c4d0;   /* pink/salmon — code value highlights, badges */
  --highlight-text:   #1a1816;   /* text on highlights */

  /* ── Semantic ── */
  --success:          #3a6a40;
  --warning:          #8a6a20;
  --error:            #8a2a20;
}

/* ── Dark Mode ── */
@media (prefers-color-scheme: dark) {
  :root {
    --bg:             #1a1816;
    --surface:        #262220;
    --surface-code:   #1e1c1a;
    --text:           #e4dcd0;
    --text-dim:       #8a8480;
    --text-inv:       #1a1816;
    --rule:           #c8c0b4;
    --rule-light:     #3a3630;
    --highlight:      #5a2838;
    --highlight-text: #f2c4d0;
    --success:        #6aaa70;
    --warning:        #caa440;
    --error:          #da6a60;
  }
}
```

### Palette Rules

1. **Black IS the accent.** There is no bright accent color. Emphasis comes from weight, size, and the contrast of `--text` against `--bg`. The only color beyond the monochrome warmth is `--highlight` (pink/salmon), used sparingly for code highlights and badges.
2. **Everything is warm.** No cool grays, no blue-blacks. Every shade has a warm undertone (yellow/brown bias). Pure `#000` and `#fff` are forbidden.
3. **Dark mode inverts warmth.** The parchment becomes the text color; the near-black becomes the background. The warmth transfers.
4. **Illustrations are monochrome.** Black, white, and grays only. No colored illustrations. If you need illustrated elements, use CSS/SVG in isometric projection with only `--text`, `--bg`, `--surface`, and `--rule` colors.

## Typography

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,500;0,600;0,700;1,400;1,500;1,600&family=DM+Sans:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
```

```css
:root {
  --font-display: 'Playfair Display', Georgia, 'Times New Roman', serif;
  --font-body:    'DM Sans', system-ui, -apple-system, sans-serif;
  --font-mono:    'JetBrains Mono', 'SF Mono', Consolas, monospace;
}
```

### Type Scale

| Element | Font | Size | Weight | Notes |
|---------|------|------|--------|-------|
| Hero display | `--font-display` | `clamp(40px, 7vw, 72px)` | 500 | Title-case, tight tracking `-1px` |
| Hero display italic | `--font-display` | same | 500 italic | Second line emphasis, slant creates visual tension |
| Section heading | `--font-display` | `clamp(24px, 4vw, 36px)` | 600 | Sentence case |
| Card heading | `--font-display` | `clamp(18px, 2.5vw, 24px)` | 600 | Left-aligned, max 2 lines |
| Body | `--font-body` | `clamp(14px, 1.2vw, 16px)` | 400 | `line-height: 1.65`, `max-width: 60ch` |
| Caption / label | `--font-body` | `12px` | 500 | Uppercase optional, `letter-spacing: 0.5px` |
| Code | `--font-mono` | `clamp(13px, 1.1vw, 15px)` | 400 | `line-height: 1.7`, `tab-size: 2` |
| Button | `--font-body` | `14px` | 600 | Sentence case, NO uppercase |
| Nav links | `--font-body` | `14px` | 500 | Sentence case |

### Typography Rules

1. **Playfair Display for ALL headings.** Every `<h1>` through `<h4>` uses the serif display font. Never use sans-serif for headings.
2. **Italic serif for emphasis lines.** When a headline has a second line for dramatic emphasis (e.g., "For Developers Who Swear / *It Wasn't Their Fault*"), the emphasis line uses Playfair Display Italic.
3. **DM Sans for everything else.** Body, nav, buttons, labels, captions — all sans-serif.
4. **JetBrains Mono for code only.** Never use monospace for non-code elements (labels, badges, etc. use DM Sans).
5. **No all-caps headings.** Headings use sentence case or title case. Uppercase is reserved for small labels/captions only.
6. **`text-wrap: balance` on headings.** Prevents orphaned words.

## Layout Foundation

```css
* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  font-family: var(--font-body);
  font-size: clamp(14px, 1.2vw, 16px);
  line-height: 1.65;
  color: var(--text);
  background: var(--bg);
  -webkit-font-smoothing: antialiased;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 clamp(20px, 4vw, 60px);
}
```

### Layout Rules

1. **ZERO border-radius.** Everywhere. Buttons, cards, inputs, code blocks, images — all sharp rectangles. The only exception is the macOS window chrome dots in code editor mockups (those are inherently circular).
2. **No box-shadows.** Depth comes from borders, background tiers, and spatial hierarchy — never from drop shadows.
3. **Grid dividers, not card borders.** Cards in a grid are separated by thin dark lines (`1px solid var(--rule)`), not individually bordered. The grid itself has the border; cells share edges.
4. **Generous vertical whitespace.** Sections have `padding: clamp(60px, 10vh, 120px) 0`. Content breathes.
5. **Left-aligned by default.** Center-alignment is reserved for hero sections and logo bars only.

## Signature Element: Diagonal Stripe Divider

The defining visual motif. A repeating 45° stripe pattern used as section separators. Think "technical drawing hatch pattern" or "security envelope interior."

```css
.stripe-divider {
  width: 100%;
  height: 14px;
  background-image: repeating-linear-gradient(
    -45deg,
    var(--rule),
    var(--rule) 1.5px,
    transparent 1.5px,
    transparent 7px
  );
  /* Fade edges for elegance */
  mask-image: linear-gradient(
    to right,
    transparent 0%,
    black 3%,
    black 97%,
    transparent 100%
  );
  -webkit-mask-image: linear-gradient(
    to right,
    transparent 0%,
    black 3%,
    black 97%,
    transparent 100%
  );
}

/* Variant: full-bleed (no edge fade) */
.stripe-divider--bleed {
  mask-image: none;
  -webkit-mask-image: none;
}
```

### Stripe Usage Rules

1. **Use at section boundaries.** Top and/or bottom of major sections (hero, features, footer). NOT between every element.
2. **Maximum 4 stripes per page.** More than that and the pattern loses its impact.
3. **Never stack stripes.** If two sections both have stripes at their boundary, only render one.
4. **Stripe height is fixed at 12-16px.** Don't scale it — it should feel like a consistent physical element (like a decorative border on printed stationery).

## Components

### Navigation Bar

```css
.nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px clamp(20px, 4vw, 60px);
  border-bottom: 1px solid var(--rule-light);
}

.nav__logo {
  display: flex;
  align-items: center;
  gap: 8px;
}

.nav__logo svg {
  width: 32px;
  height: 32px;
  color: var(--text);
}

.nav__links {
  display: flex;
  align-items: center;
  gap: clamp(16px, 3vw, 32px);
  list-style: none;
}

.nav__links a {
  font-size: 14px;
  font-weight: 500;
  color: var(--text);
  text-decoration: none;
  transition: color 0.15s ease;
}

.nav__links a:hover {
  color: var(--text-dim);
}

.nav__actions {
  display: flex;
  align-items: center;
  gap: 8px;
}
```

### Buttons

Two variants: solid (primary) and outlined (secondary). Both are sharp rectangles.

```css
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 600;
  line-height: 1;
  padding: 12px 24px;
  border: 1.5px solid var(--text);
  cursor: pointer;
  text-decoration: none;
  transition: background 0.15s ease, color 0.15s ease;
  border-radius: 0; /* ENFORCED — never round buttons */
}

.btn--solid {
  background: var(--text);
  color: var(--text-inv);
}

.btn--solid:hover {
  background: var(--text-dim);
  border-color: var(--text-dim);
}

.btn--outline {
  background: transparent;
  color: var(--text);
}

.btn--outline:hover {
  background: var(--text);
  color: var(--text-inv);
}

/* Small variant for cards */
.btn--sm {
  font-size: 13px;
  padding: 10px 18px;
}
```

### Hero Section

```css
.hero {
  text-align: center;
  padding: clamp(60px, 12vh, 140px) clamp(20px, 4vw, 60px);
  max-width: 800px;
  margin: 0 auto;
}

.hero__title {
  font-family: var(--font-display);
  font-size: clamp(40px, 7vw, 72px);
  font-weight: 500;
  line-height: 1.1;
  letter-spacing: -1px;
  text-wrap: balance;
  color: var(--text);
}

.hero__title em {
  font-style: italic;
  /* Italic line creates visual tension — the key Playfair moment */
}

.hero__subtitle {
  font-family: var(--font-body);
  font-size: clamp(14px, 1.5vw, 18px);
  font-weight: 400;
  color: var(--text-dim);
  margin-top: 20px;
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
  line-height: 1.6;
}

.hero__actions {
  display: flex;
  justify-content: center;
  gap: 12px;
  margin-top: 32px;
  flex-wrap: wrap;
}
```

```html
<section class="hero">
  <h1 class="hero__title">
    For Developers Who Swear<br>
    <em>It Wasn't Their Fault</em>
  </h1>
  <p class="hero__subtitle">
    Your AI-powered code space that catches the obvious,
    the subtle, and the "how did that even happen?"
  </p>
  <div class="hero__actions">
    <a href="#" class="btn btn--solid">Get Started</a>
    <a href="#" class="btn btn--outline">Book a Call</a>
  </div>
</section>
```

### Code Editor Mockup

The macOS-style code window is a hero element. Two-pane layout: file tree + code.

```css
.editor {
  border: 1px solid var(--rule);
  background: var(--surface-code);
  overflow: hidden;
  max-width: 960px;
  margin: 0 auto;
}

.editor__chrome {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 12px 16px;
  background: var(--surface);
  border-bottom: 1px solid var(--rule-light);
}

.editor__dot {
  width: 12px;
  height: 12px;
  border-radius: 50%; /* ONLY exception to zero border-radius rule */
}

.editor__dot--red    { background: #e85c4f; }
.editor__dot--yellow { background: #e8a84f; }
.editor__dot--green  { background: #5cc25f; }

.editor__body {
  display: grid;
  grid-template-columns: 200px 1fr;
  min-height: 320px;
}

.editor__sidebar {
  padding: 16px;
  border-right: 1px solid var(--rule-light);
  font-family: var(--font-body);
  font-size: 13px;
}

.editor__sidebar-title {
  font-size: 11px;
  font-weight: 500;
  color: var(--text-dim);
  margin-bottom: 4px;
}

.editor__sidebar-project {
  font-weight: 700;
  font-size: 13px;
  margin-bottom: 16px;
}

.editor__file-tree {
  list-style: none;
  font-size: 13px;
  line-height: 2;
}

.editor__file-tree li {
  padding-left: 0;
  color: var(--text);
}

.editor__file-tree li[data-indent="1"] { padding-left: 16px; }
.editor__file-tree li[data-indent="2"] { padding-left: 32px; }

.editor__file-tree li::before {
  content: '› ';
  color: var(--text-dim);
}

.editor__code {
  padding: 16px 24px;
  overflow-x: auto;
}

.editor__code-header {
  font-family: var(--font-mono);
  font-size: 12px;
  color: var(--text-dim);
  padding: 8px 24px;
  border-bottom: 1px solid var(--rule-light);
}

.editor__code pre {
  font-family: var(--font-mono);
  font-size: clamp(13px, 1.1vw, 15px);
  line-height: 1.7;
  color: var(--text);
  white-space: pre;
  tab-size: 2;
}

/* Syntax highlighting — warm monochrome + pink highlights */
.editor__code .kw  { color: var(--text); font-weight: 600; }  /* keywords */
.editor__code .str { color: var(--text-dim); }                 /* strings */
.editor__code .cm  { color: var(--rule-light); font-style: italic; } /* comments */
.editor__code .hl  {                                            /* highlighted values */
  background: var(--highlight);
  color: var(--highlight-text);
  padding: 1px 4px;
  margin: 0 -2px;
}

/* Line numbers */
.editor__lines {
  display: inline-block;
  text-align: right;
  padding-right: 16px;
  margin-right: 16px;
  border-right: 1px solid var(--rule-light);
  color: var(--rule-light);
  user-select: none;
  font-variant-numeric: tabular-nums;
}
```

### Logo Bar

Companies/partners displayed in equal columns separated by vertical lines.

```css
.logo-bar {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  border-top: 1px solid var(--rule);
  border-bottom: 1px solid var(--rule);
}

.logo-bar__item {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  padding: 20px 16px;
  border-right: 1px solid var(--rule);
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: 600;
  color: var(--text);
}

.logo-bar__item:last-child {
  border-right: none;
}

.logo-bar__item svg,
.logo-bar__item img {
  width: 24px;
  height: 24px;
  color: var(--text);
  filter: grayscale(100%);
}

@media (max-width: 768px) {
  .logo-bar {
    grid-template-columns: repeat(2, 1fr);
  }
  .logo-bar__item:nth-child(even) {
    border-right: none;
  }
}
```

### Feature Grid (Grid-Divided Cards)

Cards separated by grid lines — no individual borders, no shadows, no rounded corners.

```css
.feature-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  border: 1px solid var(--rule);
}

.feature-card {
  padding: clamp(24px, 4vw, 48px);
  border-bottom: 1px solid var(--rule);
  display: flex;
  flex-direction: column;
}

/* Vertical dividers: odd cards get right border */
.feature-card:nth-child(odd) {
  border-right: 1px solid var(--rule);
}

/* Remove bottom border from last row */
.feature-card:nth-last-child(-n+2) {
  border-bottom: none;
}

/* If odd total, last card has no right border */
.feature-card:last-child {
  border-right: none;
}

.feature-card__illustration {
  aspect-ratio: 4/3;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 24px;
}

.feature-card__illustration svg,
.feature-card__illustration img {
  max-width: 260px;
  max-height: 200px;
  color: var(--text);
  /* Illustrations MUST be monochrome: black/white/gray */
}

.feature-card__title {
  font-family: var(--font-display);
  font-size: clamp(18px, 2.5vw, 24px);
  font-weight: 600;
  line-height: 1.25;
  margin-bottom: 12px;
}

.feature-card__desc {
  font-size: clamp(13px, 1.2vw, 15px);
  color: var(--text-dim);
  line-height: 1.6;
  margin-bottom: 24px;
  max-width: 380px;
}

.feature-card__action {
  margin-top: auto;
}

@media (max-width: 768px) {
  .feature-grid {
    grid-template-columns: 1fr;
  }
  .feature-card:nth-child(odd) {
    border-right: none;
  }
  .feature-card:nth-last-child(-n+2) {
    border-bottom: 1px solid var(--rule);
  }
  .feature-card:last-child {
    border-bottom: none;
  }
}
```

### Isometric Illustration Style

When creating SVG illustrations for feature cards, follow this style:

1. **Isometric projection only.** 30° angles, consistent vanishing point. No perspective distortion.
2. **Monochrome palette:** `--text` (black), `--bg` (light fill), `--surface` (mid fill), `--rule` (dark lines). No colors.
3. **2px stroke weight** for outlines, `1px` for internal details.
4. **Cast shadows** as simple ellipses or offset shapes in `--rule-light`.
5. **Geometric primitives:** cylinders, cubes, discs, rectangles. No organic shapes.
6. **Technical subject matter:** gears, screens, lenses, keys, circuit-like connectors, gauges, dashboards.

```html
<!-- Example: simple isometric disc/platform -->
<svg viewBox="0 0 200 160" fill="none" xmlns="http://www.w3.org/2000/svg">
  <!-- Shadow ellipse -->
  <ellipse cx="100" cy="140" rx="70" ry="18" fill="var(--rule-light)" opacity="0.3"/>
  <!-- Base disc -->
  <ellipse cx="100" cy="110" rx="70" ry="18" fill="var(--surface)" stroke="var(--text)" stroke-width="2"/>
  <rect x="30" y="95" width="140" height="15" fill="var(--surface)" stroke="var(--text)" stroke-width="2"/>
  <ellipse cx="100" cy="95" rx="70" ry="18" fill="var(--bg)" stroke="var(--text)" stroke-width="2"/>
  <!-- Center element -->
  <polygon points="100,30 115,60 100,55 85,60" fill="var(--text)" stroke="var(--text)" stroke-width="1.5"/>
</svg>
```

### Content Section

```css
.section {
  padding: clamp(60px, 10vh, 120px) 0;
}

.section__label {
  font-family: var(--font-body);
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 0.5px;
  color: var(--text-dim);
  text-transform: uppercase;
  margin-bottom: 12px;
}

.section__title {
  font-family: var(--font-display);
  font-size: clamp(28px, 5vw, 44px);
  font-weight: 600;
  line-height: 1.15;
  letter-spacing: -0.5px;
  text-wrap: balance;
  margin-bottom: 20px;
}

.section__desc {
  font-size: clamp(14px, 1.3vw, 17px);
  color: var(--text-dim);
  line-height: 1.65;
  max-width: 560px;
}
```

### Two-Column Layout

```css
.split {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: clamp(32px, 5vw, 80px);
  align-items: center;
}

/* Reverse variant: content right, visual left */
.split--reverse {
  direction: rtl;
}

.split--reverse > * {
  direction: ltr;
}

@media (max-width: 768px) {
  .split {
    grid-template-columns: 1fr;
  }
  .split--reverse {
    direction: ltr;
  }
}
```

### Stats / Metrics Row

```css
.stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
  border-top: 1px solid var(--rule);
  border-bottom: 1px solid var(--rule);
}

.stat {
  padding: 32px 24px;
  border-right: 1px solid var(--rule);
}

.stat:last-child {
  border-right: none;
}

.stat__value {
  font-family: var(--font-display);
  font-size: clamp(32px, 5vw, 48px);
  font-weight: 500;
  line-height: 1;
  font-variant-numeric: tabular-nums;
}

.stat__label {
  font-size: 13px;
  color: var(--text-dim);
  margin-top: 8px;
}
```

### Footer

```css
.footer {
  padding: clamp(40px, 6vh, 80px) clamp(20px, 4vw, 60px);
  border-top: 1px solid var(--rule);
  display: grid;
  grid-template-columns: 2fr repeat(3, 1fr);
  gap: clamp(24px, 4vw, 60px);
}

.footer__brand {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: 600;
}

.footer__tagline {
  font-size: 13px;
  color: var(--text-dim);
  margin-top: 8px;
  max-width: 280px;
  line-height: 1.6;
}

.footer__col-title {
  font-size: 13px;
  font-weight: 600;
  margin-bottom: 16px;
}

.footer__links {
  list-style: none;
  font-size: 13px;
  line-height: 2.2;
}

.footer__links a {
  color: var(--text-dim);
  text-decoration: none;
}

.footer__links a:hover {
  color: var(--text);
}

@media (max-width: 768px) {
  .footer {
    grid-template-columns: 1fr 1fr;
  }
  .footer__brand-col {
    grid-column: 1 / -1;
  }
}
```

### Form Inputs

```css
.input {
  font-family: var(--font-body);
  font-size: 14px;
  padding: 12px 16px;
  background: var(--surface-code);
  border: 1px solid var(--rule-light);
  color: var(--text);
  width: 100%;
  border-radius: 0; /* ENFORCED */
  transition: border-color 0.15s ease;
}

.input:focus {
  outline: none;
  border-color: var(--text);
}

.input::placeholder {
  color: var(--text-dim);
}
```

### Inline Code & Code Highlights

```css
/* Inline code in body text */
code {
  font-family: var(--font-mono);
  font-size: 0.9em;
  background: var(--surface);
  padding: 2px 6px;
  border: 1px solid var(--rule-light);
}

/* Pink highlight for special values (hex colors, important strings) */
.code-hl {
  background: var(--highlight);
  color: var(--highlight-text);
  padding: 2px 6px;
  font-family: var(--font-mono);
  font-size: 0.9em;
}
```

### Badges / Tags

```css
.badge {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  font-family: var(--font-body);
  font-size: 12px;
  font-weight: 500;
  padding: 4px 10px;
  background: var(--surface);
  border: 1px solid var(--rule-light);
}

.badge--highlight {
  background: var(--highlight);
  border-color: var(--highlight);
  color: var(--highlight-text);
}
```

## Page Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Title</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,500;0,600;0,700;1,400;1,500;1,600&family=DM+Sans:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
  <style>
    /* Paste palette, typography, layout foundation, and component CSS here */
  </style>
</head>
<body>

  <!-- Navigation -->
  <nav class="nav"> ... </nav>

  <!-- Stripe divider -->
  <div class="stripe-divider"></div>

  <!-- Hero -->
  <section class="hero"> ... </section>

  <!-- Code editor mockup -->
  <div class="container">
    <div class="editor"> ... </div>
  </div>

  <!-- Stripe divider -->
  <div class="stripe-divider" style="margin-top: 60px;"></div>

  <!-- Logo bar -->
  <div class="logo-bar"> ... </div>

  <!-- Stripe divider -->
  <div class="stripe-divider"></div>

  <!-- Feature grid -->
  <section class="section">
    <div class="container">
      <div class="feature-grid"> ... </div>
    </div>
  </section>

  <!-- Footer -->
  <footer class="footer container"> ... </footer>

</body>
</html>
```

## Anti-Patterns (things that BREAK the folio aesthetic)

1. **Border-radius on ANYTHING.** Not even `2px`. Zero. Sharp. Always. (Exception: macOS dots.)
2. **Box-shadows.** No `box-shadow` anywhere. Depth comes from borders and background tiers.
3. **Bright accent colors.** No blue, no green, no orange buttons or links. Black is the accent.
4. **Cool grays.** Every gray must have warm undertone. No `#f5f5f5` or `#6b7280`.
5. **Colored illustrations.** Illustrations are monochrome (black + white + gray). No colored SVGs or images.
6. **Rounded buttons.** `border-radius: 9999px` or any pill shapes are forbidden.
7. **Monospace for non-code.** Labels and badges use DM Sans, not JetBrains Mono.
8. **Gradient backgrounds.** The background is flat `--bg`. No subtle gradients, no radial glows.
9. **Card shadows or individual card borders.** Feature cards are grid-divided, not individually boxed.
10. **More than 3 font weights.** Stick to 400, 500/600, and 700 max. Don't load every weight.
11. **All-caps headings.** Headings are sentence/title case. Caps are for tiny labels only.
12. **Dense stripe usage.** Max 4 diagonal stripe dividers per page. They're punctuation, not wallpaper.
13. **Emoji in headings or buttons.** Keep it typographic. Icons are monochrome SVG only.

## Responsive Behavior

```css
@media (max-width: 768px) {
  .hero__title { letter-spacing: -0.5px; }
  .editor__body { grid-template-columns: 1fr; }
  .editor__sidebar { display: none; }
  .feature-grid { grid-template-columns: 1fr; }
  .split { grid-template-columns: 1fr; }
  .stats { grid-template-columns: repeat(2, 1fr); }
  .footer { grid-template-columns: 1fr 1fr; }
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

## Distinguishing Folio from Other Warm UI Systems

| Property | folio-ui | western-ui | retro-ui |
|----------|----------|------------|----------|
| Background | Warm parchment `#e4dcd0` | Linen/canvas texture | Solid flat color |
| Headings | Playfair Display serif | Fraunces (WONK axis) | Bebas Neue (condensed) |
| Accent color | Black (monochrome) | Warm brown/red | Strict 3-color |
| Dividers | Diagonal stripe pattern | Horizontal rules | Bold geometric lines |
| Cards | Grid-divided (shared edges) | Individual bordered | Individual bordered |
| Illustrations | Monochrome isometric | Canvas/linen filtered | Flat color blocks |
| Shadows | None | Subtle | None |
| Typography mood | Editorial/premium | Frontier/artisanal | Industrial/mechanical |
