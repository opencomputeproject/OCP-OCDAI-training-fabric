# XOC‑512 — 4× OPG‑128

Four OPG‑128 units (16 servers each) are connected behind a shared DS5000 spine layer to
form a 512‑xPU cluster. The XOC spine (DS5000) terminates the 32×800G uplinks from each
OPG's backend rail‑leaf and frontend leaf switches, providing cross‑OPG ECMP and external
egress.

The compositions in this repository are designed to work with air or liquid cooling at any
density. Cooling medium and rack density are physical deployment choices that do not affect
the network topology.

## Variants

- [`clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/`](clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md)
  Rail‑optimized backend (CX7 1×400G)
- [`clos-sh--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/`](clos-sh--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md)
  Single‑homed backend (CX7 1×400G)

For the sub‑bundle structure see [`2x-OPG-128/`](2x-OPG-128/README.md).

## See also

- XOC‑512 tier overview: [`../../README.md`](../../README.md)
- OPG‑128 building‑block specs: [`../../../OPG-128/`](../../../OPG-128/README.md)
