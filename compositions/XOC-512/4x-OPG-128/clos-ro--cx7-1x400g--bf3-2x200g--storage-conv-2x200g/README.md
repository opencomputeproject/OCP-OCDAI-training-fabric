# XOC‑512 / 4× OPG‑128 — Rail‑Optimized Clos

Overview
- Four OPG‑128 building blocks under an XOC spine layer. Rail‑optimized backend across OPGs.

Why choose it
- Reduce spine utilization at larger scale by keeping collectives leaf‑local when jobs are placed within first‑hop rail domains.

Attributes
- Backend: rail‑optimized; CX7 1×400G per GPU (per OPG)
- Frontend: BF3 2×200G (L3MH); storage converged per OPG

Assets
- `connectivity-map.csv` — end‑to‑end cabling and port mapping
- `topology-map.yaml` — topology authoring plan (DS5000‑based)
- `wiring/` — Hedgehog Wiring CRDs (per fabric)
- `diagrams/` — visuals (per fabric)
- `netbox_inventory.json` — NetBox inventory export

Notes
 
See also
- Tier overview: ../../README.md
- Compositions overview: ../../../README.md
