# Blackwood Manor — Chapter I: The Vanishing

A single-file interactive mystery puzzle (Three.js + GSAP) inspired by *Monument Valley* and *Knives Out*.

Open `index.html` in a browser. It loads Three.js r160 (with OrbitControls) and GSAP 3.12.5 from the first
source that responds: unpkg → jsDelivr (cdnjs is also tried for GSAP) → the bundled copy in `vendor/`.

If your network blocks those CDNs, serve the folder over http so the `vendor/` copy can be used
(browsers won't load it from a `file://` page):

```sh
npx serve .        # or: python3 -m http.server
```

If the page still won't open, the loading screen says why (library blocked, WebGL disabled, or a script error).

## How to play
- Drag to turn the diorama, scroll to zoom, or use the on-screen pad / arrow keys (`R` resets).
- The rust-brown marks scattered over crates, columns and book stacks are fragments of one trail.
  Find the single viewing angle where they line up. The journal's **Perspective** meter shows how close you are.
- Once the trail glows, click its last print by the east wall (or **Follow the trail →**) to send the detective along it.

## How the illusion works
Every footprint has a "true" spot `P` on the floor path. It is moved along the secret viewing direction `D`
by a random depth `t`, so `Q = P + t·D`, and a pedestal is built under it. An orthographic camera looking
along `−D` projects the whole line `P + t·D` onto one pixel, so from that angle every fragment lands back on
the path. The alignment check compares `normalize(camera.position − controls.target)` against `D`
(`angleTo < EPSILON`), and releasing the camera within `SNAP` eases it onto the exact angle.

---

# Jewelry Workshop — Step 1: The Bench

`workshop.html` is a separate single-file Three.js (r160) scene: a studio-lit, high-density gold ring floating
over a polished marble workbench. It uses the same library loading as the mystery (unpkg → jsDelivr → `vendor/`).

- **Lighting:** warm key (the only shadow caster, PCF soft shadows), cool fill, rim, ambient, plus a
  procedural HDR "softbox" environment (PMREM) so the metals have something to reflect.
- **Ring:** `TorusGeometry(1, 0.18, 64, 128)` with dynamic position/normal buffers. Pristine data is kept on
  `ring.userData.initialPositions` / `initialNormals`, with a per-vertex `heat` buffer ready for melting.
- **Controls:** damped OrbitControls that can't go under the bench. Idle spin/float pauses while you drag and
  resumes 2.5 s later.
- **UI:** Yellow, White and Rose Gold presets (cross-faded), idle toggle, reset shape and reset view.
- **Extending:** everything is exposed on `window.workshop` (`scene`, `camera`, `ring`, `interaction.pick()`,
  `idle.hold(reason)`, `resetRing()`, and a `systems` array of `{ update(dt, elapsed) }` hooks for the frame loop).
