# OPG-64 -- Mesh-Converged Single-Homed

This variant normalizes the old converged OPG-64 training topology into the
canonical tokenized naming scheme and uses single-homed `same-switch`
distribution for `scale-out`.

Why choose it
- Compact OPG-64 building block with a simpler scale-out attachment model.
- Keeps `soc-storage`, `inb-mgmt`, and `oob-mgmt` identical to the
  rail-optimized sibling.

Attributes
- Scale-out: CX7 1x400G per GPU on a two-leaf mesh with `same-switch`
  distribution
- SoC/Storage: BF3 2x200G and storage-facing 2x200G on a two-leaf mesh
- In-band management: DS2000 fabric with a generic 2x25GbE NIC on every server
  class; one port connected
- OOB management: DS1000 fabric with management/BMC on every device

Assets
- `topology-map.yaml` -- topology authoring plan

Notes
- Intended grouped-per-leaf `same-switch` realization depends on
  `hh-netbox-plugin` issue `#322`.

See also
- Size overview: ../README.md
- Compositions overview: ../../README.md
