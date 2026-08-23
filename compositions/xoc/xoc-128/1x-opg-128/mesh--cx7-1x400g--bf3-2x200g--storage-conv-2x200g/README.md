# XOC-128 / 1x OPG-128 — Dual Mesh

This XOC-128 reference architecture contains 16 compute servers (128 GPUs).
Its frontend and backend fabrics are independent two-switch Celestica DS5000
meshes; no spine tier is present.

## Per-server connectivity

- Backend: eight CX7 1x400G NICs. The rail-optimized allocation sends rails
  1–4 to `be-rail-leaf-01` and rails 5–8 to `be-rail-leaf-02`.
- Frontend: one BF3 2x200G NIC. Its two connections alternate across
  `fe-leaf-01` and `fe-leaf-02`.

## Generated assets

- `topology-map.yaml` — DIET topology-plan input.
- `connectivity-map.csv` — generated interface and cable mapping (164 cables).
- `netbox_inventory.json` — generated plan-scoped NetBox inventory (20 devices).
- `wiring/` — Hedgehog Wiring CRD exports for backend and frontend.
- `diagrams/hhfab/` — Draw.io diagrams and `hhfab validate` logs for each fabric.

Both wiring exports were generated from local NetBox plan 57 and passed
`hhfab validate`.
