# RDKit.js in DimerForge

This folder holds [RDKit.js](https://github.com/rdkit/rdkit-js), the WebAssembly build of the RDKit cheminformatics toolkit (`@rdkit/rdkit` 2026.9.2 from npm, "MinimalLib"). The Design tab's 2D sketcher uses it to read and write SMILES, to sanitise structures (valence, aromaticity, charges), to compute InChI and InChIKey, and to lay out 2D drawings (CoordGen).

| File | What it is |
| --- | --- |
| `RDKit_minimal.js` | The Emscripten glue, unchanged from the npm package. |
| `RDKit_minimal.wasm.js` | `RDKit_minimal.wasm`, gzip-compressed and base64-encoded into one JavaScript string (3.2 MB instead of 7.4 MB). A plain `<script>` tag can load it, also when `index.html` is opened from disk (`file://`), where browsers refuse to fetch a `.wasm` file. The SHA-256 of the original `.wasm` is in the file's first comment. |
| `rdkit-frame.html` | A small page that loads RDKit and answers requests from DimerForge by `postMessage`. |
| `LICENSE.txt` | The RDKit licence (BSD 3-Clause). |

## Why a frame

The Embind glue in `RDKit_minimal.js` builds functions at run time (`new Function`), so it needs `'unsafe-eval'`, and WebAssembly needs `'wasm-unsafe-eval'`. Only the frame's Content-Security-Policy allows those. The main page keeps its strict policy and gains just `frame-src 'self'`. The frame loads nothing from other sites.

DimerForge loads the frame the first time the 2D sketcher needs it. If RDKit cannot load, the sketcher still works and still applies DimerForge's own valence rules; only SMILES, InChI and the 2D clean-up are unavailable.

## Updating

```bash
npm pack @rdkit/rdkit@<version>
tar xzf rdkit-rdkit-<version>.tgz
cp package/dist/RDKit_minimal.js vendor/rdkit/
python3 - package/dist/RDKit_minimal.wasm vendor/rdkit/RDKit_minimal.wasm.js <<'EOF'
import sys, gzip, base64, hashlib
d = open(sys.argv[1], 'rb').read()
b = base64.b64encode(gzip.compress(d, 9, mtime=0)).decode()
open(sys.argv[2], 'w').write('/* RDKit_minimal.wasm, gzip + base64. SHA-256 of the .wasm: ' + hashlib.sha256(d).hexdigest() + ' */\nself.RDKIT_WASM_GZ_B64="' + b + '";\n')
EOF
```

## Licences

RDKit is © Rational Discovery LLC, Greg Landrum, Julie Penzotti and others, under the BSD 3-Clause licence in `LICENSE.txt`. The WebAssembly build also includes third-party code that RDKit bundles, for example Boost, CoordGen, maeparser and the InChI library, each under its own open-source licence.
