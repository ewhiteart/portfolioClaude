# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A static HTML mirror of [ericcleaverwhite.com](https://www.ericcleaverwhite.com), exported from Webflow and localized for offline/rebuild use. There is no build system, no package manager, and no framework — everything is plain HTML, CSS, and vendored JS.

## Previewing the site

Open `index.html` directly in a browser, or serve it locally:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

Serving via a local server (rather than `file://`) avoids any browser restrictions on local file loading.

## Repository structure

```
index.html                  # Home page
about/index.html            # About page
work/
  silvercar/index.html
  national-geographic/index.html
  tiffs-treats/index.html
  atlas-wearables/index.html
  other-work/index.html
css/webflow.css             # All styles (single file, exported from Webflow)
js/                         # Vendored JS — do not edit
images/                     # All assets (159 files); Webflow CDN filenames preserved
```

## How assets are referenced

All paths are **relative**, not root-relative. Depth depends on location:

| File location | Image prefix | CSS/JS prefix |
|---|---|---|
| `index.html` | `images/` | `css/` / `js/` |
| `about/index.html` | `../images/` | `../css/` / `../js/` |
| `work/*/index.html` | `../../images/` | `../../css/` / `../../js/` |

When adding a new page, match the prefix depth to its folder level. If you move a page, update all relative paths in it.

## CSS architecture

`css/webflow.css` is a single minified-style file containing:
- A CSS reset (lines 1–~200)
- Webflow's utility classes (`w-nav`, `w-dropdown`, `w-layout-grid`, `w-slider`, etc.)
- Site-specific classes (`navigation`, `section`, `parallax`, `work-image`, `cc-work-1` through `cc-work-4`, etc.)

The parallax background images for each project section (`silvercar`, `natgeo2`, `tiffs`, `atlas`, `other`) are set as CSS `background-image` on `.section.parallax.<name>` — not via `<img>` tags. Those image filenames are referenced in `webflow.css` and must exist in `images/`.

The only inline style in the HTML is the `#shadowed` drop-shadow filter, defined in a `<style>` block in each page's `<head>`.

## JS files

| File | Purpose |
|---|---|
| `js/jquery.min.js` | jQuery 3.5.1 |
| `js/webfont.js` | Google WebFont Loader (loads Montserrat) |
| `js/webflow.chunk1.js` + `webflow.chunk2.js` + `webflow.main.js` | Webflow runtime (navbar, slider, animations) — home page uses all three |
| `js/webflow.228536fa.875acbab671be7ab.js` | Webflow runtime for inner pages (about + work pages) |

Fonts still load from Google Fonts CDN at runtime. All other external dependencies are vendored.

## Known issues in the mirror

- The contact form (`<form method="get">`) will not submit — it was handled by Webflow's backend.
- The works grid on the home page has some `href="#"` placeholders for Erisyon, NatGeo, Tiff's, Dribe, and Atlas cards (the thumbnail grid section), while the parallax feature sections below link correctly.
- Several project title links in the parallax sections incorrectly point to `work/silvercar/index.html` regardless of project — a copy/paste artifact from the original Webflow export.
- reCAPTCHA on the contact form is disabled (loads from Google CDN but the site key won't match).
