# Visible Human Browsers — AI Context File
_Last synced: 2026-06-15 @ 8224d89_

## 1. What This Is (Plain English)
- **In one sentence:** A website that lets you scrub through a real human body in cross-section — three views at once (top-down, side, front) that stay in sync as you drag.
- **Why it exists:** It's a clone (and tune-up) of the LUMC "Visible Human" anatomy browser from caskanatomy.info, rehosted so it can live on GitHub Pages and be tinkered with. Learning/educational anatomy tool.
- **Who uses it:** Public-ish, but low stakes — a personal rehost of an educational tool. No logins, no user data, nothing to break for other people except a static page.
- **Vibe:** Mirrored legacy site (HTTrack rip of an old jQuery app) with a modern enhancement layer bolted on top. Half "ancient artifact you shouldn't touch", half "my own JS I added at the bottom".

## 2. How To Run It
- **Setup once:** None. There is no build, no install, no dependencies to fetch — every library is already committed under `av/`.
- **Run dev:** Open `index.html` in a browser. Drag/zoom works from `file://` (verified). If anything acts up, serve it instead:
  - `python3 -m http.server` → open `http://localhost:8000/`
- **Build / deploy:** No build. Deploy = push to GitHub; served as static files. README says it's live at `https://achma-learning.github.io/anatomy-viewer-clone/` (GitHub Pages). There is **no** Pages workflow file, so Pages is presumably set to "deploy from branch" in repo settings — _not verified in-repo_.
- **Required env vars:** None (no `.env.example`, no secrets, no backend).

## 3. Tech Stack
- **Language + runtime:** Plain HTML5 + CSS3 + browser JavaScript (ES5 in the viewers). No Node runtime, no `package.json`, no lockfile.
- **Framework / key libraries (all vendored in `av/`):**
  - jQuery **3.1.1** (custom slim build, see header in `av/jquerymin.js`)
  - jQuery UI **1.12.1** — only `widget, data, scroll-parent, draggable, mouse` (`av/jqueryui.js:1`)
  - jQuery UI Touch Punch **0.2.3** — maps touch → mouse so draggable works on phones (`av/jquerytouch.js`)
- **What kind of project:** Static multi-page website (one landing page + 4 standalone viewer pages). No bundler, no framework.
- **External services:** None at runtime. All images/libs are local. Landing page has outbound *links* to nlm.nih.gov, caskanatomy.info, imaios.com, and a GitHub ZIP-download link — links only, no API calls.

## 4. Code Map (The Important Files Only)
- `index.html` — Landing page. Self-contained `<style>`, three viewer cards, download + related-links sections. Links point to the **local** viewer files (`html5/fembrowser.html`, etc.) — note this contradicts the stale claim in `analysis-v0.md:57`.
- `html5/fembrowser.html` — **Open this first to understand the viewer.** The cryosection trunk viewer; the cleanest example of the shared 3-plane pattern. Native canvas 1064×836.
- `ctviewer/fembrowserct.html` / `ctviewer/fembrowserctsmall.html` — CT trunk viewers (full ~25 MB / reduced ~15 MB). Native 1274×855. Frontal images are `f*.jpg` here (not `ff*`).
- `headneckbrowser/fembrowser.html` — Head & Neck viewer. Native 1355×680. Its drag callbacks build image src by string (`"s"+x+".jpg"`) instead of preloaded array `.src` — a small per-file divergence.
- `av/` — Shared assets: the three jQuery libs above, plus logos and `lightbluegrad*.png` / `bluegradient.png` background strips.
- **Per-viewer UI sprites** (in each viewer dir): `kruis.png` (crosshair block), `crosshair.gif` (drag handle img), `up/down/left/right.gif` (arrows), `boxje.gif` (slice-number box), `loading.gif`. Each viewer dir is intentionally self-contained.

### How one viewer works (the mental model)
Each page preloads every slice into JS maps `tar` (transversal), `sar` (sagittal), `far` (frontal) on load. Three `<img>` panels show the current slice of each plane. A 1px jQuery-UI `draggable` (`#tdraggable/#sdraggable/#fdraggable`) sits in each panel; its pixel position is mapped to an array index to pick the slice, and dragging one panel writes the linked coordinate into the other two so all three stay in sync. Arrow buttons + a `setInterval` do the same by ±1.

## 5. Rules For Editing This Code
- **Zero-dependency on purpose.** No `npm`, no `package.json`, no build step. Libraries live in `av/`. Don't "modernize" by adding a bundler.
- **ES5 in the viewer pages.** They target old browsers (IE conditional comments present). Keep `var`, no arrow functions / template literals inside the legacy inline scripts. (The enhancement IIFE at the bottom is also ES5 — keep it that way.)
- **Use `.lpos()`, never `.position()`, for crosshair / slice reads.** `.position()` returns transform-scaled coordinates and reintroduces the alignment bug. `$.fn.lpos()` returns true local px. (See §6.)
- **Keep viewers self-contained & relative-pathed.** The "download ZIP, open offline" promise (`index.html` + README) depends on no absolute URLs and each viewer carrying its own sprites.
- **Don't change container pixel sizes or rename slice images casually** — the slice index math is hard-wired to them (see §6).
- **Apply viewer changes to all 4 files.** `html5`, `ctviewer/*` (×2), `headneckbrowser` share the same pattern; a fix in one usually belongs in all four. (The nested `headneckbrowser/headneckbrowser/` copy is dead — see §6.)

## 6. Fragile Bits & Landmines
- **The crosshair scale-fix (most important — just added).** Each viewer has an injected patch right after the jQuery includes: `$.fn.lpos()` + overrides of `_mouseStart`/`_generatePosition` on `$.ui.draggable.prototype`. It reads the live zoom from `#background`'s computed `transform` matrix and divides pointer travel by it. **Symptom if removed/broken:** crosshair drifts away from the cursor (moves by `delta × scale`) whenever the viewer is scaled to fit. Don't delete it; don't switch its reads back to `.position()`.
- **Slice index math is coupled to CSS container sizes.** Panel widths/heights (e.g. html5 `#tcontainer` 372×308; ct 450×362; headneck 460×420/560) must match the preloaded array ranges. Resizing a container in CSS silently maps to the wrong slices. The new scale-fix also reads these via `clientWidth/clientHeight` for drag clamping.
- **Cryptic image-preload constants.** Loops like `t = 405, tt = 825 … tt+=5` (`html5/fembrowser.html`) or `step=1.4892086…` (`ctviewer`) are tuned per viewer to the exact image counts/spacing. Treat as magic numbers; don't refactor without the dataset in front of you.
- **The giant `#thandle` (1000×1800, offset top:-900 left:-500) is intentional.** It's a big invisible grab area around the 1px crosshair so it's easy to click. Looks like a mistake; isn't.
- **`background:00ff00` on the draggables** is invalid CSS (missing `#`) but harmless — the elements are 1px and covered. Legacy from the original mirror; not worth touching.
- **`headneckbrowser/headneckbrowser/` is an older duplicate viewer.** It is **not** linked from `index.html` and did **not** receive the scale-fix. Looks deletable (and probably is), but confirm it's truly unreferenced before removing. `headneckbrowser/av/` is also a partial duplicate of `/av`.
- **Huge binary payload in git.** ~3,500 JPEG slices (~80 MB) committed directly — no Git LFS. Don't bulk-move/optimize images without a migration plan, or history/Pages will suffer.
- **`<!-- Mirrored from … HTTrack -->` comments** mark the origin rip. Harmless; leave them.

## 7. Current State
- **Last shipped:** Crosshair drift fix (commit `8224d89`, PR #7) — made jQuery UI draggable scale-aware so the crosshair tracks the pointer at any zoom; verified in Chromium across all 4 viewers and all 3 planes at scale <1 and >1.
- **Working on now:** PR #7 open against `main` on branch `claude/pensive-thompson-tu4g62` (may be awaiting review/merge).
- **Next up:**
  1. Decide whether to delete the dead `headneckbrowser/headneckbrowser/` (and dedupe `headneckbrowser/av/`).
  2. _Nothing else firmly queued._ (Candidate, not committed: Git LFS for the image payload.)

## 8. Update Protocol (Verbatim)
> **For the AI Assistant:** When asked to "Update CONTEXT.md":
> 1. Re-run Phase 0 — check for new `GEMINI.md` / `CLAUDE.md` / `.github/` files.
> 2. Re-scan the tree, manifests, and `.github/workflows/` for drift.
> 3. Read our recent conversation for new decisions, fragile bits discovered, or shifted goals.
> 4. Refresh the `_Last synced_` line with today's date and current commit SHA.
> 5. Rewrite — do not append. One clean source of truth. Preserve still-true content, revise the rest.
> 6. Keep §1 and §2 in plain English. Keep the file under ~350 lines.
