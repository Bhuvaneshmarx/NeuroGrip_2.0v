# NEUROGRIP H1 — AI-controlled bionic hand

Interactive project page for an EMG-controlled prosthetic hand running on an ESP32.
Built as a final year project demonstration.

**Live site:** https://USERNAME.github.io/REPO/

## What the page does

The 3D arm is not a widget in a corner — it fills the viewport and the scroll position
drives the camera, so each section is a shot in a short film.

| Section | What the model does |
|---|---|
| Overview | Assembled arm, slow orbit, assembles itself on load |
| Live control | Camera closes on the hand; pick any of 7 grips and it moves |
| Muscle layer | Socket shell turns transparent; the flexor and extensor contract in real time |
| Signal chain | Camera swings round the arm while a signal pulse travels electrode → fingertip |
| Inside the arm | The whole assembly pulls apart; clickable markers on every part |
| Reference sections | Stage dims to a backdrop for the tables, code and roadmap |

Interactive throughout:

- **Seven grips.** Pick one and the EMG sliders travel to the muscle pattern that
  produces it, the classifier confidence bars re-rank, and the fingers move.
- **Drive the muscles yourself.** Drag the flexor/extensor channels and watch the
  classifier change its mind. Mirrored controls in the live and muscle sections.
- **Simulated sEMG scope** with the 200 ms analysis window marked. MAV, RMS and
  zero-crossing readouts are computed from that buffer at 10 Hz — the same decision
  rate as the firmware.
- **Exploded view** with per-part specs and prices, selectable from the list or the model.
- **Drag anywhere on the background** to orbit the arm at any point in the page.

Signals on the page are simulated. The feature maths, window length, activation floor
and class set match the firmware.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole site — markup, CSS and JavaScript in one file. No build step. |
| `vendor/three.module.min.js` | Optional local copy of three.js. If present it is used; if absent the page falls back to the jsDelivr CDN. |

## Publishing to GitHub Pages

1. Create a repository and add `index.html` at its root.
2. Repository **Settings → Pages**.
3. Under **Build and deployment**, set *Source* to **Deploy from a branch**, branch `main`,
   folder `/ (root)`, then **Save**.
4. Wait about a minute, then open `https://USERNAME.github.io/REPO/`.

The repository must be **public** for Pages to serve it on a free account, and the file
must be named `index.html` in lowercase.

### Making it work without internet

The 3D view needs three.js. To bundle it so the page works on a machine with no network:

```bash
sh download-threejs.sh
```

or by hand:

```bash
mkdir -p vendor
curl -L -o vendor/three.module.min.js \
  https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.min.js
```

Commit the `vendor` folder. The page prefers the local copy automatically.

If WebGL is unavailable the page drops the 3D stage and notes it in the corner; every
control, reading and table still works.

## Performance notes

Shadows and the higher pixel ratio are disabled below 900 px wide, and the stage stops
rendering when the tab is hidden. On phones the canvas ignores pointer events so the page
scrolls normally, and the arm orbits on its own.

## Tech

No framework, no build tooling, no dependencies beyond three.js r160 — all geometry is
generated in code, there are no model files. Single HTML file, about 108 KB. Google Fonts
(Archivo, IBM Plex Sans, IBM Plex Mono) load from the CDN.

## Status

Demonstration unit. Not a medical device.
