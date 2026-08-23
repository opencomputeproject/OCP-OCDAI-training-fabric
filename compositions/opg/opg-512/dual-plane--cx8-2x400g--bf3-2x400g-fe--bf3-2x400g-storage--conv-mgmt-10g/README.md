# OPG-512 — Dual-Plane Backend with Dedicated Frontend, Storage, and Converged Management

This OPG-512 reference architecture contains 64 compute servers (512 GPUs) and
two independent CX8 400G backend planes. Each server also has dedicated
frontend and storage connectivity plus a managed converged-management fabric.

## Per-server connectivity

- Backend: eight CX8 dual-port 400G NICs; port 0 terminates on backend-plane-a
  and port 1 on backend-plane-b.
- Frontend: two BF3 400G NICs (2x400G total) to the DS5000 frontend fabric.
- Storage: two BF3 400G NICs (2x400G total) to the dedicated DS5000 storage
  fabric.
- Converged management: one BMC 10GBASE-T link and two Intel X550 10GBASE-T
  links. DS2000 leaf ports use SFP+ 10GBASE-T copper adapters; every DS2000
  leaf has four 100G uplinks to the DS3000 spine tier.

## Generated assets

- `topology-map.yaml` — DIET topology-plan input.
- `connectivity-map.csv` — generated interface and cable mapping.
- `netbox_inventory.json` — generated plan-scoped NetBox inventory.
- `wiring/` — one validated Hedgehog Wiring CRD export per managed fabric.
- `diagrams/hhfab/` — `hhfab` validation logs and Draw.io diagrams per fabric.

All wiring artifacts were generated from the local NetBox plan and passed
`hhfab validate`.
