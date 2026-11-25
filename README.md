# DimerForge

DimerForge builds stacked molecular dimers from a single structure, with control over interplanar distance, slippage and tilt. It reads `.xyz` and `.cif` files. It can take a molecule, or a real neighbouring pair, from a crystal, and it includes a small 3D molecular designer.

It fits the molecular plane to define a frame with long, short and normal axes. You place the copy by setting the interplanar distance, the slip along each in-plane axis, the tilt about each in-plane axis and the twist about the normal. A live 3D view shows the dimer, and the tool reports the centroid distance, the angle between planes and the closest contacts, with a warning when atoms clash. It exports the dimer, or either molecule, as `.xyz`, PDB, MOL/SDF, a Gaussian input or an ORCA input, ready for calculations on π-stacking and other dimer interactions.

## Usage

DimerForge is a single HTML file, plus the `fonts/` folder it uses, with no install and no build step. It makes no requests to other sites: the IBM Plex fonts are served from this repository, and a Content-Security-Policy blocks scripts, styles, fonts and connections from anywhere else. Structures you load stay in your browser.

- Open `index.html` in any modern browser, or
- serve it with GitHub Pages (Settings → Pages → deploy from the `main` branch, root folder).

### The workspace

DimerForge fills the browser window like a desktop program:

- **Top bar:** **Open…** a file, **Paste** text, load one of the **Examples** (alloxan, or the benzene crystal), switch the theme (**Auto** follows the system, or force **Light** or **Dark**), and **Start over**.
- **Viewer:** the 3D view takes the main area. In light mode it is light grey, with outlined atoms; in dark mode, near-black.
- **Bottom dock:** three tabs under the viewer: **Readouts** (distances, angles, contacts, clash warning and a summary of the molecules), **Export** (format, options and a live preview, with Copy and Download next to the tabs) and **Input** (paste .xyz or .cif text). Drag the divider above it, or focus it and use the arrow keys, to make it taller or shorter.
- **Side panel:** the Dimer, Design, Crystal and Scan tools. It scrolls on its own, so the viewer and the readouts stay in sight.

The theme, the dock's height and its open tab are remembered in the browser. On windows narrower than 980 px or shorter than 620 px, the panels stack and the page scrolls instead.

### Steps

1. Load a structure: **Open…** an `.xyz` or `.cif` file, drop one onto the viewer, or **Paste** the text and press **Use pasted text**. `examples/Alloxan.xyz` is loaded by default. `examples/Benzene_Pbca_COD7238223.cif` is a crystal to try; **Examples** in the top bar, and the Crystal tab, load it in one click.
2. **Dimer tab.** Place copy B with the sliders, the number fields or a preset (eclipsed, parallel-displaced, antiparallel, flipped face, T-shaped).
3. **Design tab.** Edit molecule A (see below). The dimer follows the edits.
4. **Crystal tab.** Pick a molecule from a CIF, or a real neighbouring pair, and load it into the dimer builder (see below).
5. **Scan tab.** Generate a whole grid of dimers over ranges of the parameters, for structural correlation maps (see below).
6. Check the **Readouts** tab and the viewer. Drag to rotate, scroll to zoom, and use the buttons to look along n, the long axis or the short axis (or along a, b or c in the crystal view).
7. In the **Export** tab, choose what to export (dimer, molecule A or molecule B) and the format, then copy or download it.

All settings, the current molecule and the input text are kept in the browser's local storage, so a reload restores the session.

**Start over** (top bar) clears everything after asking you to confirm: the molecule, the dimer, the designer history, the crystal, the scan, every setting and the saved session. It leaves an empty workspace, which stays empty after a reload, ready for your own file or a template. **Examples** in the top bar, or **Load example: alloxan** in the Input tab, bring back the default molecule. The theme and the dock layout are viewing preferences and are kept.

## Keyboard and screen readers

- Tab reaches every control. Both tab rows (Dimer, Design, Crystal, Scan, and Readouts, Export, Input) switch with the left and right arrow keys. The divider above the dock is a keyboard-resizable separator.
- The 3D viewer takes focus too. Arrow keys rotate the view (hold Shift for small steps), `+` and `−` zoom, and `0` resets it.
- In the Design tab, **Select atoms by number** accepts numbers and ranges, such as `3, 7-9`, in the order you want them measured. It stays in sync with clicks in the viewer. Delete, Ctrl+Z, Ctrl+Y and Esc work while the viewer or the page has focus.
- In the Crystal tab, the molecule and pair lists do everything that clicking in the viewer does.
- In the Scan tab, the preview map takes focus: arrow keys move between cells and show each dimer in the viewer (its values are announced), Enter keeps it, and Escape returns to the previous dimer.
- The Readouts tab describes the structure in text, and status messages are announced. The layout passes an axe-core audit (WCAG 2.2 AA) in both themes.

## Reading `.xyz` files

- Elements are read against the full periodic table (H–Og), so `Na`, `Fe`, `Zn`, `Co`, `Pt` and so on are kept as written. Upper-case symbols (`CL`, `NA`), labelled atoms (`C12`, `H2A`) and atomic numbers (`6`, `26`) are also accepted. `D` and `T` are read as hydrogen. An unknown symbol is an error that names the line. It is never silently shortened to a different element.
- When the first line is an atom count, exactly that many atom lines are read after the comment line. Too few atom lines, or a line that is not `Element x y z`, is an error with the line number. Extra lines, or later frames of a multi-frame file, are reported and ignored. A missing comment line is tolerated when the count then matches exactly.

## Reading `.cif` files

The CIF needs the cell (`_cell_length_*`, `_cell_angle_*`), the symmetry operations (`_space_group_symop_operation_xyz` or `_symmetry_equiv_pos_as_xyz`) and fractional coordinates (`_atom_site_fract_*`). DimerForge then works as follows.

- It applies every symmetry operation, merges atoms that land on the same site, and rebuilds whole molecules across the cell faces from covalent radii. A bond is accepted when two atoms are closer than the sum of their covalent radii plus 15% or 0.3 Å, whichever is larger, so poorly placed X-ray hydrogens stay bonded.
- It groups symmetry-equivalent molecules and lists each kind with its count in the cell. Click a molecule in the viewer to select it.
- For the selected molecule it lists every neighbour with any contact within the van der Waals sum + 0.5 Å. Pairs related by symmetry are merged. Each pair shows the centroid distance, the height above the plane, the lateral offset, the angle between the planes, the closest contact and the symmetry operation that generates the partner. It is labelled stacked, offset stack, edge-to-face, side by side, inclined, or contact (for small or non-planar molecules).
- **Load pair as dimer** puts the real crystal pair into the builder. When the partner is a symmetry copy of A, the operation is converted into the builder's parameters, so the dimer reproduces the crystal geometry exactly and you can then vary it from there. Inversion and mirror operations use the **Mirror B** option. When the partner is a different molecule (Z′ > 1, co-crystals, salts), it is kept as its own molecule B. The parameters then move it in its own frame, and **Make B a copy of A** returns to a normal homodimer.
- Only the first data block is used. Disordered structures keep only one disorder group. `dum` sites are skipped. Extended networks (polymeric or ionic solids) are flagged, because they cannot be cut into separate molecules.

## Molecular designer

The Design tab edits molecule A in the 3D viewer.

- **Select.** Click an atom. Shift-click adds an atom to the selection, or removes it. Esc clears.
- **Elements.** Choose from the palette, or type any symbol under *Other*. **Add atom** bonds a new atom to the selected atom in a free direction. **Set element** changes the selected atoms. **Delete** removes them.
- **Groups.** Choose CH₃, CF₃, OH, OCH₃, SH, NH₂, NO₂, CN, CHO, COOH, C₆H₅, F, Cl, Br or I. A selected H or halogen is replaced by the group along the same bond. On any other atom the group goes in a free position. Planar groups are laid in the plane of the parent's neighbours.
- **Measure and set.** With 2, 3 or 4 atoms selected, the panel shows the distance, angle or dihedral, and you can type a new value. The side of the last selected atom moves. A dihedral cannot be set about a bond that is in a ring.
- **Tidy geometry.** This relaxes the atoms you added or re-elemented, their neighbours, and any atoms you select. It moves them towards typical bond lengths (single-bond covalent radii, shortened for multiple bonds), typical angles (from the coordination number) and away from clashes. All other atoms keep their geometry. Bonds inside a group you attached keep the group's lengths.
- **Templates.** Benzene, naphthalene, anthracene, tetracene, phenanthrene, pyrene, triphenylene, coronene, pyridine, pyrimidine, pyrazine, 1,3,5-triazine, thiophene, furan, pyrrole, methane, water or a single carbon atom.
- Undo and Redo (Ctrl+Z, Ctrl+Y) cover every edit.

## Parameter scans

The Scan tab builds the first step of a structural correlation map: a set of dimers on a regular grid of the placement parameters, ready for single-point or interaction-energy calculations.

1. **Define the grid.** For each parameter (d, slip along long, slip along short, tilt about long, tilt about short, twist), either tick it and give a minimum, maximum and step, or leave it at a fixed value. **Fixed values from the current dimer** copies the Dimer tab's values. Every combination is generated, with the last parameter varying fastest. The panel shows the count live (for example 21 × 13 = 273), allows up to 20,000 dimers, and asks before building more than 5,000.
2. **Preview.** The heat map shows two scanned parameters against each other, coloured by a measure: tightest contact as % of the vdW sum, closest contact, closest heavy-atom contact, centroid distance or angle between planes. Any other scanned parameter is shown at a value you pick. Clashing points (a contact under 75% of the vdW sum) are marked ×. Hovering a cell, or moving to it with the arrow keys, shows that dimer in the viewer; a click or Enter keeps it.
3. **Generate.** Choose Gaussian or ORCA, a folder name, and whether to leave out clashing dimers. The method line, charges and multiplicities, counterpoise or fragment labels and the coordinate frame come from the Export tab.

The result is one `.zip` that unpacks into a main folder:

```
Alloxan_scan/
├── README.txt        what was scanned, the conventions and the contents
├── index.xlsx        sheet "Scan": one row per grid point (parameters, readouts, clash flag, folder, status)
│                     sheet "Settings": molecule, grid, fixed values, frame, QM settings, date
├── plot_scan.py      self-contained matplotlib plot (the data are embedded; needs numpy and matplotlib)
├── all_dimers.xyz    every written dimer as one multi-frame .xyz
├── 001_d3.2_sl0.0/
│   ├── 001_d3.2_sl0.0.xyz
│   └── 001_d3.2_sl0.0.gjf   (or .inp for ORCA)
└── …
```

Folder names are the run number and the scanned values (`d`, `sl` slip long, `ss` slip short, `tl` tilt long, `ts` tilt short, `tw` twist; `_m` when B is mirrored). Dimers left out because they clash keep their row in `index.xlsx`, marked *left out (clash)*.

`plot_scan.py` draws a 1D line, a 2D map (with the 75% clash contour when colouring by % of the vdW sum), a 3D surface of the measure over two parameters, or a 3D scatter over three. It starts with the axes, measure and view chosen in the app, and everything can be changed from the command line:

```bash
python plot_scan.py --mode 3d --x sl --y d --z tw --metric closest --fix tl=10 --save map.pdf
```

## Geometry conventions

- **Frame.** The centroid and the plane are fitted by PCA on the heavy atoms (or on all atoms, if you choose that option). The long axis is the direction of largest variance, the short axis comes next, and the normal is n = long × short. Eigenvector signs are fixed so that each axis's largest component is positive. An option swaps the long and short axes.
- **Placing copy B.** B starts as a copy of A (or as the crystal partner, in its own frame). It is rotated about A's centroid in the molecular frame, in this order: an optional mirror through its own plane, then twist about n, then tilt about the long axis, then tilt about the short axis. That is, M = R<sub>short</sub> · R<sub>long</sub> · R<sub>n</sub> · S, where S = diag(1, 1, −1) when the mirror is on. B is then translated by (slip<sub>long</sub>, slip<sub>short</sub>, d).
- **Interplanar distance d.** d is the height of B's centroid above A's plane. It may be negative, for example for a crystal partner below A. When B is tilted, the readouts also give the range of heights of B's atoms above plane A.
- **Clash check.** Contacts are compared with the sum of the van der Waals radii (Bondi, with Mantina et al. for main-group elements that Bondi lacks, and 2.00 Å otherwise). A contact under 75% of that sum is flagged.

## Output

Choose **Dimer A+B**, **Molecule A** or **Molecule B**, and a format.

- **XYZ.** The dimer has 2N atoms: molecule A (atoms 1…N), then molecule B. The comment line records all the parameters and the atom ranges.
- **PDB.** HETATM records with A as chain A (residue 1) and B as chain B (residue 2), a TER between them, and CONECT records for every bond.
- **MOL / SDF.** A V2000 record with bonds. Bond orders are guessed from the geometry: each atom's missing valence is paired with a bonded neighbour's, and nitro-type groups get formal charges. Check the result for unusual bonding.
- **Gaussian (.gjf).** An editable route line, and the charge and multiplicity of each molecule. For a dimer, counterpoise correction is on by default: atoms are tagged `(Fragment=1)` and `(Fragment=2)`, `counterpoise=2` is added, and the charge line lists the total and each fragment.
- **ORCA (.inp).** An editable method line, and an `* xyz` block with the charge and multiplicity. For a dimer, atoms can be labelled `C(1)` and `C(2)` for fragment-based methods.

For a dimer, the total charge is the sum of the two fragment charges. The total multiplicity is the high-spin combination, m<sub>A</sub> + m<sub>B</sub> − 1.

By default the coordinates are written in the molecular frame, with A in the xy-plane and centred at the origin. Untick that option to keep the input (or crystal Cartesian) frame.

The dimer file name encodes the parameters, for example `Alloxan_dimer_d3.40_sl1.50_ss0.00_tl0_ts0_tw0.xyz` (with `_m` when B is mirrored).

## Example data and fonts

`examples/Benzene_Pbca_COD7238223.cif` is entry 7238223 of the [Crystallography Open Database](https://www.crystallography.net/), whose data are in the public domain.

The IBM Plex Sans and IBM Plex Mono fonts in `fonts/` are © IBM Corp. and licensed under the SIL Open Font License 1.1 (see the licence files in that folder).

## Changes

See [CHANGELOG.md](CHANGELOG.md).
