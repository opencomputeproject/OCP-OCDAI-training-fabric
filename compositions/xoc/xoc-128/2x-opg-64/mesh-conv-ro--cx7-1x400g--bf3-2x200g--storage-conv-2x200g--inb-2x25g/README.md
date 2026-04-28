# XOC-128 / 2x OPG-64 -- Mesh-Converged Rail-Optimized

This variant composes two OPG-64 mesh-converged training building blocks into a 128-xPU XOC using the rail-optimized scale-out distribution model.

Attributes
- Scale-out: rail-optimized mesh using the existing OPG-64 token set
- SoC/Storage: mesh-converged DS5000 pair
- In-band management: DS2000 pair with generic 2x25GbE server management NICs
- OOB management: DS1000 pair

Assets present here
- `topology-map.yaml` -- topology-first authoring plan adapted from the maintained `xoc-64` mesh variant

Notes
- This is a synthesized `xoc-128` topology derived from the existing `xoc-64/1x-opg-64` rail-optimized mesh pattern.
- Quantities were expanded to represent two OPG-64 building blocks under one XOC-128 bundle.

See also
- Bundle overview: [`../README.md`](../README.md)
- Source mesh pattern: [`../../../xoc-64/1x-opg-64/mesh-conv-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g--inb-2x25g/README.md`](../../../xoc-64/1x-opg-64/mesh-conv-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g--inb-2x25g/README.md)
