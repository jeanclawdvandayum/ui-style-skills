# Voxel Type — GPU-Rendered Animated Voxelized Typography

Create kinetic mosaic typography where text is rendered as a dense grid of GPU-shaded nested-square cells with directional lighting, edge dithering, bloom post-processing, and glitch corruption effects.

**Stack:** Three.js (InstancedBufferGeometry + custom shaders) + EffectComposer (bloom) + bitmap font engine  
**Inspired by:** @yiannifive's "voxelized type in motion"

## When to Use

- Hero text / title cards with dramatic presence
- Loading / transition animations
- Kinetic typography presentations
- Experimental / generative type art
- Slide decks with motion design quality

## Architecture Overview

```
Text Input
    ↓
Bitmap Font (6×8 per char)
    ↓
Adaptive Subdivision (2×–5× per font pixel)
    ↓
Dilation (4-connected, +1 cell stroke width)
    ↓
Edge Detection (neighbor counting → size graduation)
    ↓
Color Assignment (directional lighting + noise)
    ↓
InstancedBufferGeometry (per-cell attributes)
    ↓
Custom Vertex Shader (assembly animation, drift, wave, pulse)
    ↓
Custom Fragment Shader (nested square, dot, slash, glitch)
    ↓
EffectComposer (bloom + scanline/vignette overlay)
    ↓
CSS 3D Transform (mouse parallax)
```

## Dependencies

```html
<script type="importmap">
{
  "imports": {
    "three": "https://cdn.jsdelivr.net/npm/three@0.168.0/build/three.module.js",
    "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.168.0/examples/jsm/"
  }
}
</script>
```

```javascript
import * as THREE from 'three';
import { EffectComposer } from 'three/addons/postprocessing/EffectComposer.js';
import { RenderPass } from 'three/addons/postprocessing/RenderPass.js';
import { UnrealBloomPass } from 'three/addons/postprocessing/UnrealBloomPass.js';
import { OutputPass } from 'three/addons/postprocessing/OutputPass.js';
```

## 1. Bitmap Font Engine

Characters defined as 6-wide × 8-tall grids. `#` = filled, `.` = empty:

```javascript
const G = s => s.split('').map(c => c === '#' ? 1 : 0);
const FONT = {};
const def = (ch, ...rows) => { FONT[ch] = rows.map(G); };

def('A', '.####.','#....#','#....#','######','#....#','#....#','#....#','......');
def('B', '#####.','#....#','#....#','#####.','#....#','#....#','#####.','......');
// ... define all needed characters
```

**Why bitmap over canvas text rendering:** Resolution-independent. No font rasterization artifacts. Every character uses a fixed grid, so density scales perfectly across all viewport sizes. The GPU shader handles all visual detail per-pixel.

### Full Character Set

Include at minimum: A-Z, 0-9, space, ↑, ↓. Each character is exactly CW=6 wide × CH=8 tall (including 1 empty bottom row for spacing).

## 2. Adaptive Subdivision

The critical system for mobile legibility. Each font pixel expands into a sub×sub block of voxel cells:

```javascript
function calcSub(text, W, H) {
  const lines = text.split('\n');
  const longest = lines.reduce((a,b) => a.length > b.length ? a : b);
  const lc = lines.length;
  const minCell = 3.0; // minimum CSS px per cell

  for (let s = 5; s >= 2; s--) {
    const gap = Math.max(1, Math.round(s * 0.7));
    const cols = longest.length * CW * s + Math.max(0, longest.length - 1) * gap;
    const rows = lc * CH * s + Math.max(0, lc - 1) * gap;
    const cellW = (W * 0.86) / cols;
    const cellH = (H * 0.52) / rows;
    const cell = Math.min(cellW, cellH);
    if (cell >= minCell) return s;
  }
  return 2; // minimum subdivision
}
```

**How it adapts:**

| Text | Viewport | Sub | Grid | Cell Size | Stroke |
|------|----------|-----|------|-----------|--------|
| "V3" | 393px (phone) | 5× | 60×40 | ~5.5px | 5 cells = 28px |
| "DEPOSIT" | 393px (phone) | 2× | 96×16 | ~3.5px | 2+2 dilated = 14px |
| "V3" | 1920px (desktop) | 5× | 60×40 | ~28px | 5 cells = 140px |
| "DEPOSIT" | 1920px (desktop) | 4× | 192×32 | ~8px | 4+2 dilated = 48px |

Short text gets maximum density (5×). Long text gets minimum viable density (2×). Cell size always stays ≥ 3 CSS pixels (≥ 9 physical on retina).

## 3. Grid Generation with Dilation

```javascript
function textToGrid(text, sub) {
  const lines = text.split('\n');
  const charGap = Math.max(1, Math.round(sub * 0.7));
  const lineGap = Math.max(1, Math.round(sub * 1.0));

  // Build subdivided grid per line
  const lineGrids = lines.map(line => {
    const chars = line.split('');
    const grid = [];
    for (let row = 0; row < CH * sub; row++) {
      const fontRow = Math.floor(row / sub);
      let r = [];
      for (let ci = 0; ci < chars.length; ci++) {
        const bm = FONT[chars[ci]] || FONT[' '];
        for (let col = 0; col < CW * sub; col++) {
          r.push(bm[fontRow][Math.floor(col / sub)]);
        }
        if (ci < chars.length - 1)
          for (let g = 0; g < charGap; g++) r.push(0);
      }
      grid.push(r);
    }
    return grid;
  });

  // Center lines, combine with line gaps
  const maxW = Math.max(...lineGrids.map(g => g[0].length));
  const grid = [];
  for (let li = 0; li < lineGrids.length; li++) {
    const lg = lineGrids[li];
    const pad = Math.floor((maxW - lg[0].length) / 2);
    for (const row of lg) {
      const fr = new Array(maxW).fill(0);
      for (let c = 0; c < row.length; c++) fr[pad + c] = row[c];
      grid.push(fr);
    }
    if (li < lineGrids.length - 1)
      for (let g = 0; g < lineGap; g++) grid.push(new Array(maxW).fill(0));
  }

  // 4-connected dilation: thicken strokes by 1 cell per side
  const dilated = grid.map(r => [...r]);
  for (let r = 0; r < grid.length; r++) {
    for (let c = 0; c < maxW; c++) {
      if (grid[r][c]) continue;
      if ((r > 0 && grid[r-1][c]) || (r < grid.length-1 && grid[r+1][c]) ||
          (c > 0 && grid[r][c-1]) || (c < maxW-1 && grid[r][c+1])) {
        dilated[r][c] = 1;
      }
    }
  }
  return { grid: dilated, cols: maxW, rows: dilated.length };
}
```

**Dilation** adds 1 cell to each edge of every stroke (4-connected neighbors only). This makes text bolder and more legible, especially at small cell sizes on mobile.

## 4. Cell Generation

For each filled grid cell, compute:

### Edge Detection (Size Graduation)
Count empty 8-neighbors. More empty neighbors = smaller cell = dissolving edge:

```javascript
const edgeSizes = [1, 0.80, 0.58, 0.38, 0.20];
// edgeSizes[emptyNeighborCount] = size multiplier
```

### Directional Lighting
Position-based color index with noise for mosaic texture:

```javascript
const nx = (col - minCol) / textWidth;  // 0–1 horizontal
const ny = (row - minRow) / textHeight; // 0–1 vertical
const light = 1 - (nx * 0.35 + ny * 0.55); // top-left = brightest
const noise = (Math.random() - 0.5) * 0.22;
const ci = Math.max(0, Math.min(3, Math.floor((light + noise) * 3.99)));
```

### Assembly Animation Setup
Each cell gets a random start position (for fly-in) and stagger delay (distance from center):

```javascript
const angle = Math.random() * Math.PI * 2;
const dist = 400 + Math.random() * 350;
const fromX = targetX + Math.cos(angle) * dist;
const fromY = targetY + Math.sin(angle) * dist;
const stagger = distFromCenter / maxDist; // 0–1
```

## 5. Instanced Geometry

All cells rendered in a **single draw call** via `InstancedBufferGeometry`:

```javascript
const base = new THREE.PlaneGeometry(1, 1);
const geo = new THREE.InstancedBufferGeometry();
geo.index = base.index;
geo.attributes.position = base.attributes.position;
geo.attributes.uv = base.attributes.uv;
geo.instanceCount = cellCount;

// Per-instance attributes (Float32Array)
geo.setAttribute('aTarget',   new THREE.InstancedBufferAttribute(targetData, 2));
geo.setAttribute('aFrom',     new THREE.InstancedBufferAttribute(fromData, 2));
geo.setAttribute('aSize',     new THREE.InstancedBufferAttribute(sizeData, 1));
geo.setAttribute('aColorIdx', new THREE.InstancedBufferAttribute(colorData, 1));
geo.setAttribute('aPhase',    new THREE.InstancedBufferAttribute(phaseData, 1));
geo.setAttribute('aStagger',  new THREE.InstancedBufferAttribute(staggerData, 1));
geo.setAttribute('aFlags',    new THREE.InstancedBufferAttribute(flagsData, 1));
```

**Per-instance attributes:**
| Attribute | Type | Purpose |
|-----------|------|---------|
| `aTarget` | vec2 | Final screen position (px) |
| `aFrom` | vec2 | Random start position for assembly |
| `aSize` | float | Cell size in px (after edge detection) |
| `aColorIdx` | float | Palette index 0–3 |
| `aPhase` | float | Random phase offset for ambient animation |
| `aStagger` | float | Assembly delay factor 0–1 |
| `aFlags` | float | Bit-packed: bit0 = hasSlash, bit1 = isGlitch |

## 6. Vertex Shader

Handles all animation on the GPU:

```glsl
attribute vec2 aTarget, aFrom;
attribute float aSize, aColorIdx, aPhase, aStagger, aFlags;
uniform float uTime;
uniform vec2 uRes;
varying vec2 vUv;
varying float vCI, vFlags, vOpacity;

void main() {
  vUv = uv;
  vCI = aColorIdx;
  vFlags = aFlags;

  // Assembly (easeOutBack)
  float start = aStagger * 0.5;
  float raw = clamp((uTime - start) / 1.2, 0.0, 1.0);
  float s = 1.70158;
  float eased = 1.0 + (s + 1.0) * pow(raw - 1.0, 3.0) + s * pow(raw - 1.0, 2.0);
  vec2 cPos = mix(aFrom, aTarget, eased);

  // Ambient (post-assembly)
  float amb = smoothstep(0.85, 1.0, raw);
  cPos.x += amb * sin(uTime * 0.8 + aPhase) * 1.8;
  cPos.y += amb * (cos(uTime * 0.6 + aPhase * 1.3) * 1.8
           + sin(uTime * 2.2 + cPos.x * 0.006) * 2.5);

  // Pulse + gap
  float pulse = 1.0 + amb * sin(uTime * 1.6 + aPhase) * 0.04;
  float sz = aSize * pulse * 0.92; // 0.92 = inter-cell gap

  vOpacity = min(1.0, raw * 3.0);

  // Position (flip Y for screen coords)
  vec2 wPos = vec2(cPos.x, uRes.y - cPos.y) + position.xy * sz;
  gl_Position = projectionMatrix * modelViewMatrix * vec4(wPos, 0.0, 1.0);
}
```

## 7. Fragment Shader

Renders the nested square pattern per-pixel. Resolution-independent — looks sharp at any cell size:

```glsl
precision highp float;
varying vec2 vUv;
varying float vCI, vFlags, vOpacity;
uniform vec3 uPal[4];
uniform vec3 uAccent;
uniform float uTime;

void main() {
  if (vOpacity < 0.01) discard;
  vec2 p = vUv * 2.0 - 1.0; // -1 to 1

  // Palette lookup (WebGL 1 compatible)
  int ci = int(vCI + 0.5);
  vec3 outerCol, innerCol;
  if (ci == 0)      { outerCol = uPal[0]; innerCol = uPal[1]; }
  else if (ci == 1) { outerCol = uPal[1]; innerCol = uPal[2]; }
  else if (ci == 2) { outerCol = uPal[2]; innerCol = uPal[3]; }
  else              { outerCol = uPal[3]; innerCol = uPal[3]; }
  vec3 dotCol = uPal[3];

  // Discard outside cell bounds
  float outerDist = max(abs(p.x), abs(p.y));
  if (outerDist > 0.5) discard;

  vec3 col = outerCol;

  // Edge shadow (depth effect)
  col *= 1.0 - smoothstep(0.3, 0.5, outerDist) * 0.2;

  // Inner square (anti-aliased via smoothstep)
  float innerMask = 1.0 - smoothstep(0.515, 0.565, outerDist);
  col = mix(col, innerCol, innerMask);

  // Center dot with glow
  float dotDist = length(p);
  float dotMask = 1.0 - smoothstep(0.085, 0.135, dotDist);
  col = mix(col, dotCol, dotMask);
  col += dotCol * exp(-dotDist * 4.5) * 0.06; // subtle glow

  // Diagonal slash (bit 0 of flags)
  if (mod(vFlags, 2.0) > 0.5) {
    float sDist = abs(p.x + p.y) / 1.414;
    float sMask = (1.0 - smoothstep(0.02, 0.044, sDist))
                * (1.0 - smoothstep(0.34, 0.38, max(abs(p.x), abs(p.y))));
    col = mix(col, dotCol, sMask * 0.55);
  }

  // Glitch bands (bit 1 of flags)
  if (mod(floor(vFlags / 2.0), 2.0) > 0.5) {
    float gt = sin(uTime * 4.0 + p.y * 15.0);
    if (gt > 0.65) {
      float band = step(0.5, fract(vUv.y * 6.0 + uTime * 3.0));
      col = mix(col, uAccent, band * 0.55);
    }
  }

  // Inner highlight (subtle top-left light)
  col += innerMask * vec3(0.03) * (1.0 - (p.x + p.y) * 0.2);

  gl_FragColor = vec4(col * vOpacity, 1.0);
}
```

## 8. Scene Setup + Post-Processing

```javascript
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(W, H);
renderer.setPixelRatio(Math.min(devicePixelRatio, 2));
renderer.setClearColor(0x000000);

const camera = new THREE.OrthographicCamera(0, W, H, 0, -100, 100);

// Bloom
const composer = new EffectComposer(renderer);
composer.addPass(new RenderPass(scene, camera));
composer.addPass(new UnrealBloomPass(
  new THREE.Vector2(W, H),
  0.35,  // strength — subtle glow
  0.5,   // radius
  0.72   // threshold — only bright cells bloom
));
composer.addPass(new OutputPass());
```

### Scanline + Vignette Overlay

Rendered as a separate pass AFTER bloom (so scanlines aren't bloomed):

```javascript
const overlayScene = new THREE.Scene();
const overlayMat = new THREE.ShaderMaterial({
  transparent: true, depthTest: false,
  fragmentShader: `
    precision highp float;
    uniform vec2 uRes;
    void main() {
      float scan = mod(gl_FragCoord.y, 3.0) < 1.0 ? 0.03 : 0.0;
      float vig = 1.0 - smoothstep(0.3, 0.85,
        length((gl_FragCoord.xy / uRes - 0.5) * vec2(1.6, 1.0)));
      gl_FragColor = vec4(0., 0., 0., scan + (1.0 - vig) * 0.4);
    }
  `
});
// Render after composer:
renderer.autoClear = false;
renderer.render(overlayScene, overlayCamera);
renderer.autoClear = true;
```

## 9. Color Palettes

Each palette: 4 tones (dark → light) + accent for glitch + 3 glitch band colors:

```javascript
const PALETTES = {
  copper:   { c: ['#0c0604','#4a2814','#a06038','#e8b878'], a: '#40e0d0' },
  midnight: { c: ['#0c1830','#2a4878','#6898c8','#c8dff8'], a: '#ff44aa' },
  forest:   { c: ['#0c2010','#1a5832','#48a868','#b0f0c8'], a: '#ffee00' },
  ember:    { c: ['#1e0c06','#6a3018','#c87038','#ffd8a8'], a: '#00ffee' },
  violet:   { c: ['#140c20','#3c2060','#7848b8','#c8a0f8'], a: '#00ff88' },
  ocean:    { c: ['#060c1e','#1a3860','#4080c0','#a0d4ff'], a: '#ff6600' },
};
```

**Color assignment per cell:**
- `uPal[ci]` → outer square color (dark tones in shadows)
- `uPal[min(ci+1, 3)]` → inner square color (one step lighter)
- `uPal[3]` → center dot (always lightest)
- `uAccent` → glitch band color (high contrast)

## 10. Mouse Parallax

Simple CSS 3D transform — no Three.js camera manipulation needed:

```javascript
document.addEventListener('mousemove', e => {
  const mx = e.clientX / W, my = e.clientY / H;
  const rx = (my - 0.5) * -4;  // ±2°
  const ry = (mx - 0.5) * 6;   // ±3°
  renderer.domElement.style.transform =
    `perspective(1200px) rotateX(${rx}deg) rotateY(${ry}deg)`;
});
```

## Configuration Reference

| Parameter | Default | Purpose |
|-----------|---------|---------|
| `minCell` | 3.0 | Minimum cell size in CSS px (calcSub threshold) |
| `edgeSizes` | [1, 0.80, 0.58, 0.38, 0.20] | Size multipliers by empty neighbor count |
| Gap factor | 0.92 | Inter-cell gap (vertex shader `sz * 0.92`) |
| Assembly dur | 1.2s | Time for cells to fly in |
| Max stagger | 0.5 | Delay spread for assembly (center → edge) |
| Bloom strength | 0.35 | Glow intensity |
| Bloom threshold | 0.72 | Luminance threshold for bloom |
| Dilation | 4-connected | Stroke thickening method |
| Slash chance | 0.25 | Probability of diagonal slash per cell |
| Glitch zone | > 0.8 normX | Right 20% of text bounds |
| Glitch chance | 0.10 | Per-cell probability within zone |

## Performance

- **Rendering:** Single draw call via InstancedBufferGeometry (5000+ cells at 60fps)
- **Animation:** 100% GPU (vertex shader) — zero JS per-frame per-cell work
- **Post-processing:** 2 render passes (bloom + overlay)
- **Memory:** ~7 floats × 4 bytes × cellCount = ~140KB for 5000 cells
- **Mobile:** Caps DPR at 2, minimum cell size 3px prevents excessive instance counts

## Anti-Patterns

| Don't | Why | Do Instead |
|-------|-----|------------|
| Use canvas text rendering | Rasterization quality varies by device/OS | Bitmap font with subdivision |
| Fixed cell size across viewports | Too sparse on mobile, too dense on desktop | Adaptive subdivision (calcSub) |
| Skip dilation | 1-cell strokes are too thin on mobile | 4-connected dilation pass |
| Bloom without threshold | Everything glows, looks washed out | threshold 0.7+ so only bright cells glow |
| Animate in JS per-frame | CPU bottleneck at high cell counts | All animation in vertex shader |
| 8-connected dilation | Over-thickens corners, fills small gaps | 4-connected preserves character shape |

## Demo

**Working demo:** `~/Desktop/projects/voxel-type/v3-explainer.html` (22KB, single file)

9-slide Alchemix V3 explainer with:
- Adaptive subdivision per slide
- 6 distinct color palettes
- Click/arrow/scroll/swipe navigation
- Mouse parallax
- Assembly animation on slide change
- Ambient drift + wave + pulse
- Edge glitch corruption
- Bloom + scanlines + vignette

## Credits

Technique inspired by [@yiannifive](https://x.com/yiannifive) — "voxelized type in motion" created in Cavalry with Pangram Pangram's Editorial New typeface.

Skill by Jean 🦾 for the Alchemix ecosystem.
