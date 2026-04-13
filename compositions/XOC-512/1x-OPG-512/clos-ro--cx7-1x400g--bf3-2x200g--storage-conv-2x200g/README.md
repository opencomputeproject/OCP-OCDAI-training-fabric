# XOC‑512 / 1× OPG‑512 — Rail‑Optimized Clos

Rail‑optimized backend with converged frontend. Domain‑local placement keeps most collectives leaf‑local, reducing spine traffic across the composition.

Why choose it
- Medium‑scale clusters that benefit from lower tail latency and spine relief without changing host counts.

Attributes
- Backend: rail‑optimized; CX7 1×400G per GPU
- Frontend: BF3 2×200G (L3MH); storage converged

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
