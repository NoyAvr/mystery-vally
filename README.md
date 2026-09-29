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
