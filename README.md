# Interactive Instax Camera

[![GitHub](https://img.shields.io/badge/GitHub-Interactive--Instax--Camera-blue?logo=github)](https://github.com/abdellahsaidi375/Interactive-Instax-Camera.github.io)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Three.js](https://img.shields.io/badge/Three.js-0.162.0-lightgrey?logo=three.js)](https://unpkg.com/three@0.162.0/build/three.module.js)

An interactive, story-driven web experience that walks through 5 emotional stages — from a falling sakura blossom, through a simulated Instax camera prank, to voice interaction, engraved signatures, and a glass-morphism rating panel. Built with vanilla JavaScript and Three.js — no bundlers, no frameworks.

---

## Table of Contents

- [Phases](#phases)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Rendering Architecture](#rendering-architecture)
- [Code Conventions](#code-conventions)
- [Key Files by Phase](#key-files-by-phase)
- [SVG Anatomy](#svg-anatomy)
- [Known Issues](#known-issues)
- [Contributing](#contributing)
- [License](#license)

---

## Phases

| Phase | Description |
|-------|-------------|
| **1** | Pink background, sakura blossom falls with physics → glows → clickable |
| **2** | Click → screen dims, music plays, typewriter text → Instax camera slides up → 3s countdown → 3 black photos (prank) |
| **3** | Voice 1: "forgot to show teeth" → 3s countdown → sun photo prints → Voice 2 → 3 real photos from local folder |
| **4** | Camera exits → 3 photos on table → marquee ribbons in DaveMoris font → engraved "Batoul" signature → glassmorphism rating slider |
| **5** | Rating GIF reactions (Sonic / suspicious / angry) → satisfaction meme → language buttons (AR, EN, DZ, FR) → "I miss you" audio |

---

## Prerequisites

- A static file server (any of these):
  ```sh
  npx http-server -p 8001
  npx serve
  ```
  > `file://` will **not** work — the Three.js importmap and `fetch` calls require HTTP.

- No build tools, package managers, or install steps required.

---

## Getting Started

```sh
# Clone
git clone https://github.com/abdellahsaidi375/Interactive-Instax-Camera.github.io.git
cd Interactive-Instax-Camera.github.io

# Serve from project root
npx http-server -p 8001

# Open http://localhost:8001
```

Open `http://localhost:8001` in your browser. The experience starts immediately with the sakura fall animation.

---

## Project Structure

```
├── index.html              # Main experience (Phase 1 entry point)
├── repo.html               # Standalone retro-camera + polaroid desk (ported)
├── compare.html            # SVG vs Three.js visual parity comparison
├── context.md              # Full user specification (final authority)
├── instax.md               # Structured phase-by-phase breakdown
├── AGENTS.md               # Agent conventions and gotchas
├── opencode.json           # OpenCode project configuration
│
├── .PHASE1/                # SVG → Three.js visual parity test suite
│   ├── Phase.html          #   Multi-SVG combiner
│   ├── compare.html        #   Pixel-diff compare tool
│   ├── Petals.html         #   Petal group test
│   ├── Stigma.html         #   Stigma group test
│   ├── Stamens.html        #   Stamens group test
│   ├── anthers.html        #   Anthers group test
│   ├── Petals/             #   Individual petal SVGs (5 petals)
│   ├── Stigma/             #   Individual stigma SVGs (5 segments)
│   ├── Anthers.svg/xml     #   Source SVGs with gradients
│   ├── Petals.xml/svg
│   ├── Stigma.xml/svg
│   └── Stamens.xml/svg
│
├── assets/
│   ├── fonts/              # ArefRuqaa-Bold.ttf, DaveMoris.otf
│   ├── ico/fav.ico
│   ├── img/
│   │   ├── sakura.svg      # Flower SVG (4 groups: Petals, Stigma, Stamens, Anthers)
│   │   ├── sakura.xml
│   │   ├── instax.png
│   │   ├── retro-camera.png/.webp
│   │   └── Falawla/        # Photo assets for Phase 3-4
│   └── vid/                # Video assets
│
├── glow/                   # Experimental canvas glow effects (not integrated)
│   ├── black-hole/
│   ├── glow/
│   ├── glowing-icons/
│   ├── glowing-stuff/
│   └── vortex/
│
├── graphify-out/           # Knowledge graph (90 nodes, 91 edges)
└── bck/                    # Backup files
```

---

## Rendering Architecture

Two parallel rendering approaches coexist for visual parity investigation:

### Approach A: `buildHierarchy()` (index.html)
- Parses SVG XML manually via `SVGLoader`
- Resolves gradients through `resolveGradients()` → `makeGradientTexture()` pipeline
- Builds Three.js groups by `<g>` id (`Petals`, `Stigma`, `Stamens`, `Anthers`)
- Applies element transforms as mesh transforms

### Approach B: `SVGLoader.toShapes()` (compare.html)
- Pure `SVGLoader.toShapes()` conversion
- Custom `makeStrokeGeometry()` + per-pixel gradient shader
- Different transform handling — produces slightly different output

Both are compared side-by-side in `compare.html` using a pixel-diff overlay.

---

## Code Conventions

- **`//#explaining_the_function#`** — must appear above every function
- **Full block replacement** — never patch; rewrite the entire function
- **ID-based isolation** — each stage/effect bound to a specific HTML `id`
- **No external frameworks** beyond the Three.js importmap
- **Vanilla JS only** — no TypeScript, React, or bundlers

### Three.js importmap (fixed version)
```html
<script type="importmap">
  {
    "imports": {
      "three": "https://unpkg.com/three@0.162.0/build/three.module.js",
      "three/addons/": "https://unpkg.com/three@0.162.0/examples/jsm/"
    }
  }
</script>
```
> Do not bump the version without testing visual parity.

---

## Key Files by Phase

| Phase | File(s) |
|-------|---------|
| 1 (Sakura fall) | `index.html` — physics at ~L73, animation loop at ~L476 |
| 2 (Camera prank) | `repo.html` — retro camera + polaroid system |
| 3 (Voice/sun) | `context.md` — audio `onended` chaining spec |
| 4 (Signature + glass) | `context.md` — marquee ribbons, engraved text, glass slider |
| 5 (Final/buttons) | `context.md` — rating GIFs, language button spec |

---

## SVG Anatomy

The sakura SVG (`assets/img/sakura.svg`, Inkscape-origin) contains four named `<g>` groups:

- **Petals** — 5 petal shapes with `userSpaceOnUse` linear gradients
- **Stigma** — 5 stigma segments
- **Stamens** — stamen filaments
- **Anthers** — anther tips

Gradients use `gradientTransform` matrix attributes and `xlink:href` inheritance. The `resolveGradients()` function handles both `linearGradient`/`radialGradient` elements with `userSpaceOnUse` and `objectBoundingBox` units.

---

## Known Issues

- **SVG path `src/img/sakura.svg`** — index.html loads this relative path; must serve from project root, not a subdirectory
- **Visual parity incomplete** — the two rendering approaches produce pixel-level differences (active investigation in `compare.html` and `triangulation_test.html`)
- **Phase build-order** — the full 5-phase flow is specified in `context.md` but not yet wired into a single page; index.html implements Phase 1 only

---

## Contributing

1. Read `context.md` (full spec) and `AGENTS.md` (conventions) before making changes
2. Place `//#explaining_the_function#` above every function
3. Replace entire function blocks — no patching
4. Test visual parity via `compare.html` when touching SVG/Three.js rendering
5. Use `echo 'Generating smart compound log...'` when archiving chat logs

---

## License

This project is open source. See the [LICENSE](LICENSE) file for details.
