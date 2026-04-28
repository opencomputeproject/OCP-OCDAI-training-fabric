# OPG‑128 — Rail‑Optimized Clos (CX7 1×400G, BF3 2×200G, Converged Storage)

This building block applies a rail‑optimized Clos to a 128‑xPU training pod (16 servers × 8 xPUs). Backend traffic uses CX7 1×400G per GPU; frontend/storage use BF3 2×200G per server with L3 multi‑homing.

## What this building block provides

Rail‑optimized wiring constrains each GPU's CX7 NIC to a dedicated backend rail leaf
(one NIC per rail, predictable switch locality per rail domain). DS5000 zoning rules are
consistent with larger OPG tiers, supporting clean scale.

## Uplink reservation

Each backend rail leaf reserves ports 33–64 (32×800G) for XOC spine connectivity.
Each frontend leaf reserves even ports 2–64 (32×800G) for XOC spine connectivity.
These ports must not be consumed by server connections.

## What a composer must supply

1. XOC backend spine switches (DS5000) cabled to the 32×800G reserved ports on each
   backend rail leaf
2. XOC frontend spine switches (DS5000) cabled to the 32×800G reserved ports on each
   frontend leaf
3. External/border connectivity (DS3000 or equivalent) if WAN/ISP uplinks are required

## Attributes

- Backend: single plane, rail‑optimized
- Scale‑out NICs: CX7 1×400G per GPU
- Frontend NICs: BF3 2×200G per server (L3MH)
- Storage: converged, 2×200G per server
- Optics: OS2 SMF DR‑class (default)
- DS5000 zoning: 4×200G on odd ports; 2×400G unrestricted; 32×800G uplinks reserved

## Assets

- `connectivity-map.csv` — end-to-end cabling and port mapping
- `topology-map.yaml` — topology authoring plan (DS5000-based)
- `wiring/` — Hedgehog Wiring CRDs
  - `wiring-backend.yaml` — backend fabric wiring
- `diagrams/` — visual diagrams
  - `hhfab/backend.drawio` — backend topology diagram (draw.io)
- `netbox_inventory.json` — NetBox inventory export

## See also

- Size overview: `../README.md`
- Compositions overview: `../../README.md`
