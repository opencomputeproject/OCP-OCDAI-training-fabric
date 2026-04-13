# OPG‑128 — Single‑Homed Clos (CX7 1×400G, BF3 2×200G, Converged Storage)

This building block applies a single‑homed Clos backend to a 128‑xPU training pod. Each
server's backend NICs terminate on one leaf (8 servers per leaf); frontend remains L3
multihomed.

## What this building block provides

Single‑homed wiring assigns each server's backend NICs to a single leaf, giving clear
per‑leaf failure domains. This matches the OPG‑M exemplar topology for the backend.
DS5000 zoning is consistent with rail‑optimized variants at this tier.

## Uplink reservation

Each backend leaf reserves ports 33–64 (32×800G) for XOC spine connectivity.
Each frontend leaf reserves even ports 2–64 (32×800G) for XOC spine connectivity.
These ports must not be consumed by server connections.

## What a composer must supply

1. XOC backend spine switches (DS5000) cabled to the 32×800G reserved ports on each
   backend leaf
2. XOC frontend spine switches (DS5000) cabled to the 32×800G reserved ports on each
   frontend leaf
3. External/border connectivity (DS3000 or equivalent) if WAN/ISP uplinks are required

## Attributes

- Backend: single‑homed Clos (per‑server backend on one leaf)
- Scale‑out NICs: CX7 1×400G per GPU
- Frontend NICs: BF3 2×200G per server (L3MH)
- Storage: converged, 2×200G per server
- Optics and zoning as per DS5000 guidance (4×200G on odd ports; 2×400G unrestricted;
  reserve 32×800G uplinks/leaf)

## Assets

- `connectivity-map.csv` — end‑to‑end cabling and port mapping
- `topology-map.yaml` — topology authoring plan (DS5000‑based)
- `wiring/` — Hedgehog Wiring CRDs
  - `wiring-backend.yaml` — backend fabric wiring
- `diagrams/` — visual diagrams
  - `hhfab/backend.drawio` — backend topology diagram (draw.io)
- `netbox_inventory.json` — NetBox inventory export

## See also

- Size overview: `../README.md`
- Compositions overview: `../../README.md`
