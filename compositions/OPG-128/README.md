# OPG‑128 — Variants Overview

OPG‑128 is a 128‑xPU training building block: 16 compute servers (8 xPUs each) plus
dedicated backend and frontend leaf‑spine fabrics. It does not include an XOC spine or
external connectivity; those are supplied when this OPG is composed into an XOC cluster.

The compositions in this repository are designed to work with air or liquid cooling at any
density. Cooling medium and rack density are physical deployment choices that do not affect
the network topology.

## Variants

- [`clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/`](clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md)
  Rail‑optimized backend — each GPU's CX7 NIC connects to a dedicated rail leaf. NIC‑to‑leaf assignment is constrained by rail number; each leaf reserves 32×800G uplinks for XOC spine connectivity.

- [`clos-sh--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/`](clos-sh--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md)
  Single‑homed backend — each server's backend NICs terminate on a single leaf (8 servers per leaf). Backend leaves reserve the same 32×800G uplink budget as rail‑optimized.

### Comparing building‑block patterns

### Comparing building‑block patterns

| | Rail‑optimized | Single‑homed |
|--|---------------|--------------|
| NIC‑to‑leaf assignment | By rail number (predictable per GPU) | By server (each server on one leaf) |
| Intra‑rail locality | All GPUs on rail N share a leaf | Varies by server assignment |
| Backend leaf count | One per rail (8 leaves for 8 rails) | One per server group (2 leaves for 16 servers) |
| Uplink budget | 32×800G per rail leaf | 32×800G per backend leaf |

## Common attributes

- Backend switches: DS5000 leaf + DS5000 spine (per fabric)
- Frontend: DS5000 leaf pair (L3MH/ECMP) + DS5000 spine; converged storage/in‑band
- OoB: DS1000
- Port zoning: 4×200G on odd ports; 2×400G unrestricted; 32×800G uplinks per leaf reserved
- Optics: OS2 SMF DR‑class (default)

## Legacy folders

`clos-rail-optimized/` and `clos-single-homed/` are kept for discoverability.
