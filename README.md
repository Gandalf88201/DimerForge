# DimerForge

DimerForge builds stacked molecular dimers from a single `.xyz` structure, with control over interplanar distance, slippage and tilt.

It fits the molecular plane to define a frame with long, short and normal axes. You place the copy by setting the interplanar distance, the slip along each in-plane axis, the tilt about each in-plane axis and the twist about the normal. A live 3D view shows the dimer, and the tool reports the centroid distance, the angle between planes and the closest contacts, with a warning when atoms clash. It exports a two-molecule `.xyz` ready for calculations on π-stacking and other dimer interactions.

## Usage

DimerForge is a single self-contained HTML file with no install and no build step.

- Open `index.html` in any modern browser, or
- serve it with GitHub Pages (Settings → Pages → deploy from the `main` branch, root folder).

Then:

1. Load a structure: open an `.xyz` file, drop one onto the viewer, or paste the text and press **Use pasted text**. `examples/Alloxan.xyz` is loaded by default.
2. Place copy B with the sliders, the number fields or a preset (eclipsed, parallel-displaced, antiparallel, flipped face, T-shaped).
3. Check the readouts and the viewer. Drag to rotate, scroll to zoom, and use the buttons to look along n, the long axis or the short axis.
4. Copy or download the dimer `.xyz`.

## Geometry conventions

- **Frame.** The centroid and the plane are fitted by PCA on the heavy atoms (or on all atoms, if you choose that option). The long axis is the direction of largest variance, the short axis comes next, and the normal is n = long × short. Eigenvector signs are fixed so that each axis's largest component is positive. An option swaps the long and short axes.
- **Placing copy B.** B starts as a copy of A. It is rotated about A's centroid in the molecular frame, in this order: twist about n, then tilt about the long axis, then tilt about the short axis. That is, M = R<sub>short</sub> · R<sub>long</sub> · R<sub>n</sub>. B is then translated by (slip<sub>long</sub>, slip<sub>short</sub>, d).
- **Interplanar distance d.** d is the height of B's centroid above A's plane. When B is tilted, the readouts also give the range of heights of B's atoms above plane A.
- **Clash check.** Contacts are compared with the sum of the Bondi van der Waals radii. A contact under 75% of that sum is flagged.

## Output

The output is a standard `.xyz` file with 2N atoms: molecule A (atoms 1…N), then molecule B (atoms N+1…2N). The comment line records all the parameters and the atom ranges. By default the coordinates are written in the molecular frame, with A in the xy-plane and centred at the origin. Untick that option to keep the input frame.

The file name encodes the parameters, for example `Alloxan_dimer_d3.40_sl1.50_ss0.00_tl0_ts0_tw0.xyz`.
