Repository Analysis: anatomy-viewer-clone
Purpose
A static web application that clones the Visible Human Browsers from Leiden University Medical Center (LUMC), Dept. of Anatomy & Embryology. It provides interactive anatomical cross-section viewers based on the Visible Human Project dataset.
Original source: caskanatomy.info/browser/ (mirrored via HTTrack on 11 Jan 2025)
License/Copyright: © 2013–2017 D. Jansma & M.C. de Ruiter, Dept. of Anatomy LUMC, Leiden. Free to use.
Structure
anatomy-viewer-clone/
├── index.html                  # Landing page listing all 3 viewers
├── av/                         # Shared assets (jQuery, logos, UI images)
├── html5/                      # Viewer 1: Complete trunk, cryosections
│   ├── fembrowser.html
│   └── ~744 JPG images (t*, s*, ff* prefixes)
├── ctviewer/                   # Viewer 2: Complete trunk, CT scans
│   ├── fembrowserct.html
│   ├── fembrowserctsmall.html
│   └── ~1,366 JPG images (f*, s*, t* prefixes)
└── headneckbrowser/            # Viewer 3: Head & Neck
    ├── fembrowser.html
    └── ~1,440 JPG images (t*, s*, ff* prefixes)
3 Viewers Available
Viewer
Subject
Image type
Size
Images
Complete trunk browser (html5/)
Female, full trunk
Gross cryosections
15 MB
744
Complete trunk CT browser (ctviewer/)
Female, full trunk
CT scans
15–25 MB
1,366
Head & Neck browser (headneckbrowser/)
Female, head & neck
Gross cryosections
27 MB
1,440
Technical Stack
Pure static HTML — no server-side code, no build system, no frameworks
jQuery (minified) + jQuery UI + jQuery Touch — for drag interaction
Image-preloading pattern: All slice images (t*.jpg, s*.jpg, ff*.jpg) are preloaded into JS arrays on page load
3-plane linked browsing: Transversal, Sagittal, and Frontal planes displayed simultaneously; dragging one plane updates the other two in real time via pixel-position-to-index mapping
Navigation: Keyboard arrows, draggable crosshairs (kruis.png), and scroll buttons
Compatibility: Targets IE, Firefox, Chrome, Safari (IE6 conditional comment present)
Viewport requirements: 836×1064px to 855×1274px depending on viewer
Branches
main — primary branch
claude/analyze-github-repo-WAB8J — current working branch
Commits
5 commits total; last update via README. No CI/CD configuration.
Key Observations
Massive image payload: The repo hosts thousands of raw JPEG slices directly in git — not suitable for large-scale or production use without Git LFS or a CDN.
No build/deployment config: Served directly as static files; GitHub Pages works out of the box since everything is plain HTML/JS/images.
Links in index.html still point to the original caskanatomy.info domain — not to the GitHub Pages URLs — so the landing page navigation doesn't actually route to the local viewer copies.
Legacy code: Uses HTML 4.01 Transitional DOCTYPE, IE6 conditional comments, and inline minified CSS/JS in the viewer pages.
Viewport is fixed-width (950–1274px) — not responsive/mobile-friendly.