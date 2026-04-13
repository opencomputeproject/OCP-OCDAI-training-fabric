# XOC‑1024 / 2× OPG‑512 — Dual‑Plane

Attributes
- Backend: dual plane (2p); CX8 2×400G per GPU
- Frontend: BF3 2×200G (L3MH); storage converged

Assets (partial — generation incomplete; needs fast-follow)
- `topology-map.yaml` — topology authoring plan (DS5000-based)
- Pending: `connectivity-map.csv`, `wiring/`, `diagrams/`, `netbox_inventory.json`

Notes
- Wiring and NetBox export blocked by port-exhaustion gap at XOC‑1024 scale; see generate.log.

What to expect next
- We will publish connectivity maps, wiring, diagrams, and NetBox inventory once port budgeting is finalized for this tier.
