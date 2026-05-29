# XOC-64 / 1x OPG-64 -- Mesh-Converged Rail-Optimized

This variant wraps the OPG-64 converged training building block in the XOC catalog using the canonical tokenized naming scheme and rail-optimized scale-out placement.

Why choose it
- Smallest composed training case with explicit XOC framing and minimal topology overhead.
- Preserves rail-aware scale-out semantics while separating `soc-storage`, `inb-mgmt`, and `oob-mgmt`.

Attributes
- Scale-out: CX7 1x400G per GPU on a two-leaf mesh with rail-optimized distribution
- SoC/Storage: BF3 2x200G and storage-facing 2x200G on a two-leaf mesh
- In-band management: DS2000 fabric with a generic 2x25GbE NIC on every server class; one port connected
- OOB management: DS1000 fabric

Assets
- `connectivity-map.csv` -- end-to-end cabling and port mapping (128 cables)
- `netbox_inventory.json` -- NetBox inventory export (21 devices, 259 modules, 481 interfaces)
- `bom.csv` -- bill of materials (servers, switches, NICs, transceivers)
- `topology-plan.yaml` -- topology authoring plan (DS5000-based)
- `wiring/` -- Hedgehog Wiring CRDs per fabric (`inb-mgmt`, `soc-storage-scale-out`)
- `diagrams/hhfab/` -- hhfab diagrams and validate logs per fabric
- `generated/` -- pipeline provenance (inputs, run logs)

Notes
- `scale-out` and `soc-storage` are combined on a single `soc-storage-scale-out` DS5000 mesh pair.
- Connection IDs align with fabric names where practical.

See also
- Bundle overview: ../../README.md
- Compositions overview: ../../../README.md
