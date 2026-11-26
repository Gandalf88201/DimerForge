# Changelog

All notable changes to DimerForge. Versions follow [Semantic Versioning](https://semver.org/); each release is a git tag.

## [Unreleased]

### Added
- **Molecular builder** in the Design tab, which now has two modes. **2D sketch** draws a structural formula, like the builder on moleculesfriends.eu: atoms C, N, O, S, P, F, Cl, Br, I, H, B, Si (and any other symbol), single, double and triple bonds, 3- to 6-membered and benzene rings (free, grown or fused), formal charges, unpaired electrons and an eraser, with implicit hydrogens, undo and redo, keyboard shortcuts, SMILES in and out, examples, and **Sketch molecule A** to turn the current molecule into a drawing. **3D edit** is the previous designer.
- **Valence rules.** Edits that give an atom more bonds than it can form (allowed valences from the valence electrons and the formal charge, with expanded valences from period 3) are blocked, with the reason and the charge that would make them legal; changing a four-bonded carbon to N makes it N⁺. A **Chemistry check** flags expanded valences, carbocations and carbanions, large or like charges, unpaired electrons, strained sp atoms in small rings, separate fragments and elements without rules, and gives the formula, molar mass, net charge, spin multiplicity, SMILES and InChIKey. In 3D edit, adding to a full atom is blocked, and **Add missing H** fills open valences.
- **Build 3D as molecule A**: hydrogens, a start from the drawing (or a distance-geometry embedding for bridged and caged rings) and a **UFF** optimisation (Rappé et al. 1992; typing as in RDKit, energies agree with RDKit's UFF). Molecule A keeps the drawn bond orders and formal charges, which the MOL/SDF export writes, and the Export tab's charges and multiplicities follow the drawing. **Optimise (UFF)** relaxes molecule A in 3D edit.
- **RDKit.js** 2026.9.2 vendored in `vendor/rdkit/` (BSD licence) for SMILES, InChI, sanitisation and 2D layout. It runs offline, in a hidden frame with its own Content-Security-Policy.
- Manual sections on the valence rules, 3D structures and UFF, and RDKit.js.
- **Built-in manual** in English: **Help** in the top bar (or <kbd>F1</kbd>, or <kbd>?</kbd>) opens it over the workspace, with a contents list and a search box that highlights matches; a **?** button on each tool tab, and on the dock, opens its own section. It covers how to use every tool and the science and conventions behind them (molecular frame, placement of B, contacts and clashes, bonds and elements, crystal handling, Tidy, file formats, scan conventions, references), with diagrams that follow the light and dark themes.

### Changed
- The MOL/SDF export takes an atom with more bonds than the neutral element allows as the matching ion (ammonium N⁺, borate B⁻) when it guesses charges.
- The Content-Security-Policy allows frames from the same folder (`frame-src 'self'`), for RDKit.js.

## [2.4.0] - 2026-11-25

### Changed
- **New workspace layout**, like a desktop program: the 3D viewer fills the window; a resizable dock under it holds the Readouts, Export and Input tabs; the tools sit in a side panel that scrolls on its own. No more empty space under the viewer or long page scrolling. On narrow or short windows the panels stack.
- **Light and dark themes** with a switch in the new top bar (Auto follows the system). The 3D viewer and the scan heat map follow the theme; in light mode atoms and bonds are outlined so hydrogens stay visible.
- Compact, consistent styling: smaller type, joined button groups, section labels, aligned parameter rows. Readouts gain a row with the molecules' formulas, atom counts and plane fit.

### Added
- Top bar with **Open…**, **Paste** and **Examples** (alloxan, benzene crystal). The theme, the dock height and its open tab are remembered.

## [2.3.0] - 2026-11-25

### Added
- **Start over** button (top right): after you confirm, it clears the molecule, dimer, designer history, crystal, scan and every setting, empties the saved session and leaves an empty workspace. The empty workspace survives a reload.
- **Load example: alloxan** button in Input structure, and an empty-workspace message in the viewer, readouts and export panel.

## [2.2.0] - 2026-11-25

### Added
- **Scan tab** for structural correlation maps: a full grid over any of d, slip long, slip short, tilt long, tilt short and twist (minimum, maximum and step each; the rest fixed), up to 20,000 dimers.
- Live preview heat map of any two scanned parameters, coloured by tightest contact (% of vdW sum), closest contact, closest heavy-atom contact, centroid distance or angle between planes, with clashes marked. Hover, or use the arrow keys, to see each dimer in the viewer.
- One `.zip` per scan: a main folder with one subfolder per dimer (`.xyz` plus a Gaussian or ORCA input), `index.xlsx` (every grid point with its readouts and clash flag, plus the settings), a self-contained matplotlib script `plot_scan.py` (1D, 2D or 3D), `all_dimers.xyz` and a README. Clashing dimers can be left out; they stay listed in the index.

## [2.1.0] - 2026-11-25

### Added
- Keyboard control of the 3D viewer: arrow keys rotate (Shift for small steps), `+`/`−` zoom, `0` resets.
- Designer: **Select atoms by number** (`3, 7-9`), so every designer action works without a mouse.
- Content-Security-Policy that blocks scripts, styles, fonts and connections from other sites.

### Changed
- IBM Plex fonts are served from `fonts/` instead of Google Fonts, so visitors' IP addresses are no longer sent to Google. First paint is faster (about 50 ms instead of 90 ms in testing).

### Fixed
- Accessibility: the page has a `main` landmark, the export preview can be scrolled with the keyboard, button groups are named, and input and export messages are announced to screen readers. axe-core (WCAG 2.2 AA and best practice) reports no violations.

## [2.0.0] - 2026-11-24

### Added
- CIF input: symmetry expansion, whole molecules rebuilt across cell faces, molecule kinds, and neighbour pairs with distances, angles, contacts and the generating symmetry operation.
- **Load pair as dimer**: reproduces a real crystal pair exactly in the builder. New **Mirror B** parameter for inversion- and mirror-related pairs; partners that are different molecules (Z′ > 1, co-crystals) are kept as their own molecule B.
- Molecular designer: add, delete and re-element atoms, attach 15 groups, set distances, angles and dihedrals, tidy geometry, 18 templates, undo and redo.
- Export of the dimer, molecule A or molecule B as XYZ, PDB, MOL/SDF (guessed bond orders), Gaussian (counterpoise fragments) and ORCA input.
- Example crystal `examples/Benzene_Pbca_COD7238223.cif` (public domain).

### Fixed
- Element symbols are checked against the full periodic table. Before, two-letter symbols outside a small table were silently cut to one letter (`Na`→N, `Fe`→F, `Co`→C), so exports had wrong elements.
- The atom count on the first line of an `.xyz` file is enforced, with line-numbered errors.
- The option checkboxes are saved and restored with the session.

## [1.0.0] - 2026-11-22

- First release: interactive `.xyz` dimer builder with interplanar distance, slip, tilt and twist in the molecular frame, contact readouts with a clash warning, and `.xyz` export.

[Unreleased]: https://github.com/Gandalf88201/DimerForge/compare/v2.4.0...HEAD
[2.4.0]: https://github.com/Gandalf88201/DimerForge/compare/v2.3.0...v2.4.0
[2.3.0]: https://github.com/Gandalf88201/DimerForge/compare/v2.2.0...v2.3.0
[2.2.0]: https://github.com/Gandalf88201/DimerForge/compare/v2.1.0...v2.2.0
[2.1.0]: https://github.com/Gandalf88201/DimerForge/compare/v2.0.0...v2.1.0
[2.0.0]: https://github.com/Gandalf88201/DimerForge/compare/v1.0.0...v2.0.0
[1.0.0]: https://github.com/Gandalf88201/DimerForge/releases/tag/v1.0.0
