# XOC‑512 / 1× OPG‑512 — Bundle Overview

This bundle places a single OPG‑512 unit under an XOC spine layer to form a 512‑xPU
cluster. The XOC spine (DS5000) connects the uplinks from the OPG‑512 backend rail‑leaf
and frontend leaf switches, providing cross‑fabric ECMP and external egress that the
OPG‑512 building block does not supply on its own.

The compositions in this repository are designed to work with air or liquid cooling at any
density. Cooling medium and rack density are physical deployment choices that do not affect
the network topology.

## Variants

- [`clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/`](clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md)
  Rail‑optimized backend (CX7 1×400G)
- [`dual-plane--cx8-2x400g--bf3-2x200g--2p--storage-conv-2x200g/`](dual-plane--cx8-2x400g--bf3-2x200g--2p--storage-conv-2x200g/README.md)
  Dual‑plane backend (CX8 2×400G)

See each variant folder for the full composed‑solution guide.

## See also

- XOC‑512 tier overview: [`../../README.md`](../../README.md)
- OPG‑512 building‑block specs: [`../../../opg-512/`](../../../opg-512/README.md)
