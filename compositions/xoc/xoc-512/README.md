# XOC‑512 — Overview

Welcome. XOC‑512 composes OPG units behind a DS5000 spine to reach ~512 xPUs. Choose the bundle that fits your operating model:
- 1x‑OPG‑512: one large OPG under a spine (mirrors OPG‑512: rail‑optimized and dual‑plane)
- 4x‑OPG‑128: four OPG‑128 units under a spine (mirrors OPG‑128: rail‑optimized and single‑homed)

Operator benefits
- Reuse familiar OPG wiring and scheduling patterns at higher scale.
- Rail‑optimized variants reduce spine utilization when jobs are rail‑local; dual‑plane variants add isolation and bandwidth headroom.

Bundles
- 1x-opg-512/
- 4x-opg-128/

What you’ll find in each variant
- `connectivity-map.csv`, `topology-map.yaml`, `wiring/`, `diagrams/`, `netbox_inventory.json`

See also
- Compositions overview: ../README.md
