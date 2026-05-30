# XOC‑512 / 1× OPG‑512 — Dual‑Plane

Two backend planes; CX8 2×400G per GPU.

Why choose it
- Plane‑level isolation and bandwidth headroom for medium‑scale clusters.

Attributes
- Backend: dual plane (2p); CX8 2×400G per GPU
- Frontend: BF3 2×200G (L3MH)

Assets
- `connectivity-map.csv` — end-to-end cabling and port mapping (1728 cables)
- `netbox_inventory.json` — NetBox inventory export (91 devices, 3424 modules, 4086 interfaces)
- `bom.csv` — bill of materials (servers, switches, NICs, transceivers)
- `topology-plan.yaml` — topology authoring plan (DS5000-based)
- `wiring/` — Hedgehog Wiring CRDs per fabric
  - `wiring-backend-plane-a.yaml` — hhfab validate OK
  - `wiring-backend-plane-b.yaml` — hhfab validate OK
  - `wiring-frontend.yaml` — hhfab validate OK
- `diagrams/hhfab/` — hhfab diagrams and validate logs per fabric
- `generated/` — pipeline provenance (inputs, run logs)

Notes
