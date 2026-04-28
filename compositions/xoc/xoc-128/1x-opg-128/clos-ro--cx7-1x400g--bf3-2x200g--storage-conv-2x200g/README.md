# XOC-128 / 1x OPG-128 -- Clos RO

This variant wraps the rail-optimized OPG-128 training building block in the XOC catalog. At 128 xPUs, it preserves the established OPG-128 rail-aware first-hop model without introducing a separate XOC spine tier in this folder.

Attributes
- Backend: rail-optimized Clos
- Scale-out NICs: CX7 1x400G per GPU
- Frontend NICs: BF3 2x200G per server (L3MH)
- Storage: converged, 2x200G per server

Assets present here
- `connectivity-map.csv` -- end-to-end device and port mapping
- `topology-map.yaml` -- compute-only topology authoring plan reframed for `xoc-128`
- `netbox_inventory.json` -- inventory export copied from the maintained `opg-128` source

Notes
- This directory intentionally reuses the maintained `opg-128` asset set and only reframes it under the `xoc-128` catalog.
- Reserved-uplink and leaf-role assumptions remain those documented by the source OPG-128 variant.

See also
- Bundle overview: [`../README.md`](../README.md)
- OPG-128 source variant: [`../../../opg/opg-128/clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md`](../../../opg/opg-128/clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md)
