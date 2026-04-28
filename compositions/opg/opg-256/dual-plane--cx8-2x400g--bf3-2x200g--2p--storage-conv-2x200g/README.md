# OPG‑256 — Dual‑Plane Backend (CX8 2×400G, BF3 2×200G, Converged Storage)

Two independent backend fabrics (Plane A/B) with CX8 2×400G per GPU — one port to each plane. Improves bandwidth and allows plane‑level maintenance windows.

Why choose it
- Higher failure isolation: each plane can be maintained independently.
- More bandwidth headroom for large collectives and busy clusters.

Attributes
- Backend: dual plane (2p); CX8 2×400G per GPU (RoCE)
- Frontend: BF3 2×200G per server (L3MH)
- Storage: converged, 2×200G per server
- Optics/Zoning: per DS5000 guidance; maintain uplink budgets across planes

Assets
- `connectivity-map.csv` — end-to-end cabling and port mapping
- `topology-map.yaml` — HNP topology plan input (DS5000-based)
- `wiring/` — Hedgehog Wiring CRDs
  - `wiring-backend-plane-a.yaml` — backend Plane A fabric wiring
  - `wiring-backend-plane-b.yaml` — backend Plane B fabric wiring
- `diagrams/` — visual diagrams
  - `hhfab/backend-plane-a.drawio` — backend Plane A topology (draw.io)
  - `hhfab/backend-plane-b.drawio` — backend Plane B topology (draw.io)
- `netbox_inventory.json` — NetBox inventory export

Considerations
- Dual‑plane adds parts and operational complexity; use when steady‑state utilization is high or isolation is a priority.
