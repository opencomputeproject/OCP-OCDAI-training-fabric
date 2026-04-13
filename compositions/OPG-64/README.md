# OPG-64 — Variants Overview

OPG‑64 is a 64‑xPU training building block: 8 compute servers (8 xPUs each) plus the leaf
switches and supporting infrastructure they attach to. It does not include a spine or
external connectivity; those are provided by the XOC when this OPG is composed into a
larger cluster.

The compositions in this repository are designed to work with air or liquid cooling at any
density. Cooling medium and rack density are physical deployment choices that do not affect
the network topology.

## Variants

- [`collapsed-conv--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/`](collapsed-conv--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md)
  Collapsed converged — single DS5000 leaf pair carries backend and frontend traffic. No active spine; 32×800G uplinks reserved for XOC connectivity. OoB via DS1000.

- [`clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/`](clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md)
  Rail‑optimized Clos — separate backend fabric (DS5000 leaf per rail) and converged frontend fabric. Each CX7 NIC is constrained to one rail leaf, producing predictable switch locality per rail domain.

- [`mesh-conv-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g--inb-2x25g/`](mesh-conv-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g--inb-2x25g/README.md)
  Mesh‑converged, rail‑optimized — DS5000 leaf pair with explicit fabrics for scale‑out, soc‑storage, in‑band management (DS2000, 2×25G per server), and OoB management (DS1000).

- [`mesh-conv-sh--cx7-1x400g--bf3-2x200g--storage-conv-2x200g--inb-2x25g/`](mesh-conv-sh--cx7-1x400g--bf3-2x200g--storage-conv-2x200g--inb-2x25g/README.md)
  Mesh‑converged, single‑homed — same topology as mesh-conv-ro but with same‑switch scale‑out distribution. Simpler NIC‑to‑leaf assignment; remaining fabrics are identical to the rail‑optimized sibling.

## Common attributes

- Switch family: DS5000 (800/400/200G) + DS1000 (OoB)
- Port zoning: 4×200G breakouts on odd ports only; 2×400G unrestricted
- Uplink budget: 32×800G per leaf, reserved for XOC spine connectivity
- Frontend multihoming: L3MH/ECMP across two leaves (Clos variants)
- Optics: OS2 SMF DR‑class (default)

## Notes

- Full connection maps and BOMs live inside each variant folder when available.
