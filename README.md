[README.md](https://github.com/user-attachments/files/32425508/README.md)
# Composure — thoughts today, songs tomorrow

A journal that listens to a week of your feelings and composes music from them.
This folder is the finished, publishable site: one self-contained `index.html`
(the photographed desk scene, the affect contract and seeded week, writing /
speaking / highlighting on the page, the affect → music engine, and the
"Transpose this into a song" sequence). It needs no build step and no server
features — any static host works.

## Publish on GitHub Pages (about two minutes)

1. Create a new repository on GitHub (public, empty — no README).
2. Put the two files from this folder at the **root** of the repository:
   `index.html` and `.nojekyll`. Either drag them into the GitHub web UI
   ("Add file → Upload files"), or from a terminal:

   ```bash
   cd composure-site
   git init
   git add index.html .nojekyll README.md
   git commit -m "Composure"
   git branch -M main
   git remote add origin https://github.com/<you>/<repo>.git
   git push -u origin main
   ```

3. In the repository: **Settings → Pages → Build and deployment**.
   Source: *Deploy from a branch*. Branch: `main`, folder `/ (root)`. Save.
4. After a minute the site is live at `https://<you>.github.io/<repo>/`.

Netlify Drop (drag the folder onto app.netlify.com/drop), Vercel, Cloudflare
Pages and Surge all work the same way — it is a single static file.

## Run it locally

```bash
python3 -m http.server 8000 --directory composure-site
```

then open <http://localhost:8000>. (Opening `index.html` straight from disk also
works in most browsers; a local server avoids font/CDN quirks.)

## What's in it

- **The desk.** The reference photograph is the scene plate, scaled never
  reflowed (1536×1024 artboard). Only what changes per entry is DOM.
- **The week rail** (left of the notebook) opens any day. Days with an entry
  play the handwriting animation; empty days open a blank page to write on.
- **Write / speak / highlight.** Click the page (or the pen on the desk) to
  type; tap *speak* under the pen to dictate (Chrome/Edge/Safari — uses the
  browser's speech recognition, no server); click the lavender highlighter and
  drag across words to mark the ones that carry the feeling. The right page
  fills in from the text as you write (rule-based analysis, runs offline).
- **Transpose this into a song** (under the notebook): the words lift off the
  page onto a staff, the sheet rolls into a record, the record lands on the
  turntable, the needle drops and the motif plays while the LP spins.
  *or play the whole week* strings every day's motif into one piece and follows
  along on the rail. Playback controls appear under the record.
- Entries and highlights you add are saved in the browser (`localStorage`).
  To wipe them: open the browser console and run `Composure.reset()`.

External resources (loaded from CDNs): Google Fonts (Caveat, IBM Plex Mono) and
Tone.js 14.8 for audio. If Tone.js can't load, the animation still runs; the
status line under the notebook says so.

## Rebuilding from the sources

The parent folder holds the sources and the merge script:

- `composure.html` — the design agent's scene (plate, contract, seeded entries)
- `journal-input.js`, `tone-engine.js` — the build agent's modules
- `transpose.html`, `journal-input.html` — the build agent's standalone prototypes
- `build.py` — assembles `composure-site/index.html` from the above
- `make_disc.py` — cuts the clean spinning-record image (`assets/`) out of the plate

```bash
python3 make_disc.py   # only if the plate photograph changes (needs Pillow, numpy, scipy)
python3 build.py
```
