# Changelog

All notable changes to DimerForge. Versions follow [Semantic Versioning](https://semver.org/); each release is a git tag.

## [Unreleased]

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

[Unreleased]: https://github.com/Gandalf88201/DimerForge/compare/v2.2.0...HEAD
[2.2.0]: https://github.com/Gandalf88201/DimerForge/compare/v2.1.0...v2.2.0
[2.1.0]: https://github.com/Gandalf88201/DimerForge/compare/v2.0.0...v2.1.0
[2.0.0]: https://github.com/Gandalf88201/DimerForge/compare/v1.0.0...v2.0.0
[1.0.0]: https://github.com/Gandalf88201/DimerForge/releases/tag/v1.0.0
