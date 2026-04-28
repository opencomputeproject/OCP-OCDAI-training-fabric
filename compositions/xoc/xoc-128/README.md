# XOC‑128 — Overview

XOC‑128 reaches 128 xPUs in the training catalog without introducing a dedicated spine tier. It provides two composition paths:
- `2x-opg-64/` — two OPG‑64 first-hop domains composed into a 128‑xPU mesh
- `1x-opg-128/` — one OPG‑128 building block wrapped in the XOC catalog

Operator benefits
- Offers a clean 128‑xPU transition point between `xoc-64` and `xoc-256`.
- Preserves the two design paths already implied by the reference architecture: scaling out from two OPG‑64 units or standardizing on one OPG‑128 unit.
- Avoids inventing a separate spine layer where the current RA describes a two-leaf mesh approach at this size.

Bundles
- `2x-opg-64/`
- `1x-opg-128/`

Asset notes
- `1x-opg-128` includes the same topology-first assets already maintained under `opg-128`.
- `2x-opg-64` is currently topology-map-first and README-first, derived from the existing `xoc-64` mesh patterns.

See also
- Compositions overview: [`../README.md`](../README.md)
- OPG‑64 building-block specs: [`../../opg/opg-64/README.md`](../../opg/opg-64/README.md)
- OPG‑128 building-block specs: [`../../opg/opg-128/README.md`](../../opg/opg-128/README.md)
