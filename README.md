# From Windows Servers to OpenShift Presentation

This repository contains a Reveal.js HTML slideshow and supporting speaker notes for a broad-audience presentation about moving from n-tier Windows server hosting to OpenShift-based container hosting.

## Files

- `index.html` — the Reveal.js presentation.
- `slide-notes.md` — expanded presenter notes and talk track for each slide.

## Run locally

Because the presentation uses Reveal.js from a CDN, open `index.html` directly in a browser or serve the folder with a simple local web server:

```bash
python3 -m http.server 8000
```

Then browse to <http://localhost:8000/>.

## Presenter notes

Reveal.js speaker notes are embedded in the slides. Press `S` while viewing the slideshow to open the speaker view. The expanded notes in `slide-notes.md` can be used as a rehearsal script or distributed separately.

## GitHub Pages deployment

The `.github/workflows/deploy-pages.yml` workflow publishes this static Reveal.js deck to GitHub Pages when changes are pushed to the `main` branch. It can also be started manually from the GitHub Actions `workflow_dispatch` button.

