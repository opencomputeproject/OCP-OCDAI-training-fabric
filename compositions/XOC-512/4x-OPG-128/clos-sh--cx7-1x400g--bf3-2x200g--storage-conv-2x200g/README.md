# XOC‑512 / 4× OPG‑128 — Single‑Homed Clos

Overview
- Four OPG‑128 building blocks under an XOC spine layer. Single‑homed backend per OPG and converged frontend.

Why choose it
- Simpler host semantics at XOC scale; good for staged adoption or environments prioritizing straightforward operations.

Attributes
- Backend: single‑homed; CX7 1×400G per GPU
- Frontend: BF3 2×200G (L3MH)

Assets
- `connectivity-map.csv` — end‑to‑end cabling and port mapping
- `topology-map.yaml` — topology authoring plan (DS5000‑based)
- `wiring/` — Hedgehog Wiring CRDs (per fabric)
- `diagrams/` — visuals (per fabric)
- `netbox_inventory.json` — NetBox inventory export

Notes
