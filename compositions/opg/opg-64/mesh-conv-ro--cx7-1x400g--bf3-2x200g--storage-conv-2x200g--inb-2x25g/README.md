# OPG-64 -- Mesh-Converged Rail-Optimized

This variant normalizes the old converged OPG-64 training topology into the
canonical tokenized naming scheme and uses explicit fabrics for `scale-out`,
`soc-storage`, `inb-mgmt`, and `oob-mgmt`.

Why choose it
- Minimal switch count for a compact OPG-64 building block while preserving
  rail-aware scale-out placement.
- Separates management fabrics cleanly without changing the base server mix.

Attributes
- Scale-out: CX7 1x400G per GPU on a two-leaf mesh with `rail-optimized`
  distribution
- SoC/Storage: BF3 2x200G and storage-facing 2x200G on a two-leaf mesh
- In-band management: DS2000 fabric with a generic 2x25GbE NIC on every server
  class; one port connected
- OOB management: DS1000 fabric with management/BMC on every device

Assets
- `topology-map.yaml` -- topology authoring plan

Notes
- Connection IDs align with fabric names where practical.

See also
- Size overview: ../README.md
- Compositions overview: ../../README.md
