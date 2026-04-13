# XOC‑512 / 1× OPG‑512 — Dual‑Plane

Two backend planes; CX8 2×400G per GPU.

Why choose it
- Plane‑level isolation and bandwidth headroom for medium‑scale clusters.

Attributes
- Backend: dual plane (2p); CX8 2×400G per GPU
- Frontend: BF3 2×200G (L3MH)

Assets
- `connectivity-map.csv` — end-to-end cabling and port mapping
- `topology-map.yaml` — topology authoring plan (DS5000-based)
- `wiring/` — Hedgehog Wiring CRDs
  - `wiring-backend-plane-a.yaml` — backend Plane A fabric wiring
  - `wiring-backend-plane-b.yaml` — backend Plane B fabric wiring
- `diagrams/` — visual diagrams
  - `hhfab/backend-plane-a.drawio` — backend Plane A topology (draw.io)
  - `hhfab/backend-plane-b.drawio` — backend Plane B topology (draw.io)
- `netbox_inventory.json` — NetBox inventory export

Notes
