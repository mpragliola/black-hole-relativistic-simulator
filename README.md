# Black Hole Simulator

A real-time, in-browser simulation of a non-rotating (Schwarzschild) black hole with a lensed accretion disk, written as a single WebGL2 fragment shader. No build step, no dependencies.

**[▶ Live demo](https://mpragliola.github.io/black-hole-relativistic-simulator/)**

## What you see

- **Gravitational lensing.** Every pixel traces a light ray through curved spacetime, so the starfield behind the hole is bent around it and the far side of the disk is lifted over the top of the shadow.
- **Photon ring.** Computed, not painted: rays that skim the photon sphere (1.5 r<sub>s</sub>) loop around the hole and pick up extra, exponentially thinner images of the disk and sky.
- **Relativistic disk effects.** Doppler shift, gravitational redshift and beaming, each adjustable.
- **Teaching aids.** Letter grids for the disk and the sky make lensing obvious (letters repeat, flip and wrap), and a false-colour redshift map shows the frequency shift directly. Every control has an info tooltip.

## Controls

| Input | Action |
|---|---|
| Drag | Orbit the camera |
| Mouse wheel / pinch | Change distance |
| `H` or the **Controls** button | Collapse / expand the panel |
| `i` icons in the panel | Explain each control |

## How it works

- Units: Schwarzschild radius r<sub>s</sub> = 1. Disk from the innermost stable orbit (3 r<sub>s</sub>) to 14 r<sub>s</sub>.
- A photon stays in one plane through the hole, so each ray is integrated as a planar orbit with the exact equation `d²u/dφ² + u = 1.5·u²` (`u = r_s / r`), using RK4 in the orbit angle φ.
- Each time a ray crosses the disk plane it samples the disk. The disk emits blackbody colour from a Novikov–Thorne-like temperature profile, scaled by the Doppler × gravitational frequency ratio *g* (brightness ∝ *g*<sup>n</sup>).
- The sky is an equirectangular image sampled along the final, lensed ray direction.

Simplifications: no black-hole spin (Kerr), an infinitely thin disk, procedural turbulence, and a simple tone map. It is a visualisation, not a scientific renderer.

## Run locally

The sky texture is loaded as an image, so open the page over HTTP rather than from `file://`:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Requires a browser with WebGL2. If it runs slowly, lower **Render scale** or **Ray steps** in the panel.

## Deployment

Every push to `main` runs [`.github/workflows/deploy-pages.yml`](.github/workflows/deploy-pages.yml), which publishes the site to the `gh-pages` branch with `keep_files: true`. Existing files on that branch are kept, and because GitHub Pages serves each project repository under its own path (`/<repo>/`), this site does not touch the other project sites under the same account.

One-time setup: in the repository settings, set **Pages → Source** to the `gh-pages` branch, root folder.

## Project layout

```
index.html                      # page, UI and the GLSL fragment shader
assets/stars.jpg                # equirectangular sky background
.github/workflows/deploy-pages.yml
```

## License

Code: MIT, see [LICENSE](LICENSE). The sky image in `assets/` is not covered by this license.
