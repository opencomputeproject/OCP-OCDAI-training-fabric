# OPG‑512 — Rail‑Optimized Clos (CX7 1×400G, BF3 2×200G, Converged Storage)

Rail‑optimized Clos backend at OPG‑512 scale. With tenancy aligned to first‑hop rail domains, most collectives remain leaf‑local, dramatically reducing spine traffic and uplink congestion; consistent DS5000 zoning supports clean scale.

Why choose it
- Reduce spine congestion at higher OPG scale by keeping collectives leaf‑local when placement aligns with rail domains.

Attributes
- Backend: single plane, rail‑optimized; CX7 1×400G per GPU
- Frontend: BF3 2×200G per server (L3MH)
- Storage: converged, 2×200G per server

Assets
- `connectivity-map.csv` — end-to-end cabling and port mapping
- `topology-map.yaml` — topology authoring plan (DS5000-based)
- `wiring/` — Hedgehog Wiring CRDs
  - `wiring-backend.yaml` — backend fabric wiring
- `diagrams/` — visual diagrams
  - `hhfab/backend.drawio` — backend topology diagram (draw.io)
- `netbox_inventory.json` — NetBox inventory export

Notes
