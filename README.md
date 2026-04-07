# Visible Human Browsers — Anatomy Viewer Clone

Interactive anatomical cross-section viewer based on the [Visible Human Project](https://www.nlm.nih.gov/research/visible/visible_human.html) dataset, cloned and enhanced from the original work by D. Jansma & M.C. de Ruiter, Dept. of Anatomy & Embryology, LUMC, Leiden.

**Live site (GitHub Pages):** `https://achma-learning.github.io/anatomy-viewer-clone/`

---

## Description

Three simultaneous anatomical planes (transversal, sagittal, frontal) are displayed side by side. Interacting with one plane automatically updates the crosshair positions in the other two, so all three views stay spatially in sync at all times.

No plugin or installation required — pure HTML + jQuery, runs entirely in the browser.

---

## Available Viewers

| Viewer | Subject | Image type | Size |
|--------|---------|-----------|------|
| **Complete trunk — Cryosections** | Female, full trunk | Gross cryosections | ~15 MB |
| **Complete trunk — CT scan (full)** | Female, full trunk | CT sections | ~25 MB |
| **Complete trunk — CT scan (reduced)** | Female, full trunk | CT sections | ~15 MB |
| **Head & Neck** | Female, head & neck | Gross cryosections | ~27 MB |

---

## Features

### Navigation
- **Three linked planes** — transversal, sagittal, and frontal displayed simultaneously
- **Drag crosshair** — click and drag the crosshair on any panel to reposition; the other two panels update in real time
- **Arrow buttons** — on-screen ↑ ↓ ← → buttons for each panel

### Keyboard shortcuts

| Key | Action |
|-----|--------|
| `↑` / `↓` | Step through **transversal** slices |
| `←` / `→` | Step through **sagittal** slices |
| `PgUp` / `PgDn` | Step through **frontal** slices |
| `Shift` + any above | Jump **×10 slices** at once |
| `F` | Toggle **fullscreen** |
| `H` | Toggle **help / shortcuts panel** |
| `Esc` | Close help panel |

### Mouse & touch
| Action | Result |
|--------|--------|
| **Scroll wheel** over a panel | Navigate that panel's slices |
| **Two-finger pinch** | Zoom the viewer in / out (0.5× – 4×) |
| **Double-tap** | Reset zoom to 1× |

### UI
- **Scale-to-fit** — viewer automatically scales to fill the viewport with no scrollbars, on any screen size
- **Fullscreen button** — `⛶` in the top-left toolbar
- **Loading progress bar** — gold → pink gradient bar replaces the plain "LOADING…" text, with a `X% (N / total)` counter
- **Download for offline use** — ZIP download link on the landing page; all files are self-contained static assets
- **← Home** button on every viewer page

---

## Project Structure

```
anatomy-viewer-clone/
├── index.html                   # Landing page — viewer selection
├── av/                          # Shared assets (jQuery, logos, UI images)
├── html5/                       # Cryosection trunk viewer
│   ├── fembrowser.html
│   └── t*.jpg  s*.jpg  ff*.jpg  (~744 images)
├── ctviewer/                    # CT trunk viewer (full + reduced)
│   ├── fembrowserct.html
│   ├── fembrowserctsmall.html
│   └── t*.jpg  s*.jpg  f*.jpg   (~1 366 images)
└── headneckbrowser/             # Head & Neck viewer
    ├── fembrowser.html
    └── t*.jpg  s*.jpg  ff*.jpg  (~1 440 images)
```

---

## Offline Use

1. Click **Download ZIP** on the landing page, or clone the repository:
   ```bash
   git clone https://github.com/achma-learning/anatomy-viewer-clone.git
   ```
2. Open `index.html` in any modern browser — no server needed.

---

## Tech Stack

- HTML5, CSS3
- jQuery · jQuery UI (draggable) · jQuery Touch
- Vanilla JS (ES5) — `PerformanceObserver`, `requestFullscreen`, Touch Events API
- Hosted via **GitHub Pages** (static, no backend)

---

## Credits & License

Original browsers developed by **D. Jansma & M.C. de Ruiter**, Dept. of Anatomy & Embryology, Leiden University Medical Center, the Netherlands.
© 2013 – 2017. Free to use for educational purposes.

Dataset: [NLM Visible Human Project](https://www.nlm.nih.gov/research/visible/visible_human.html)

Alternative viewer: [IMAIOS e-Anatomy](https://www.imaios.com/en/e-anatomy/whole-body/visible-human-project)
