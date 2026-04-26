# OPG-256 Virtual — Variants Overview

This folder contains a teaching-oriented virtual composition that mirrors the structure of a real OPG while making the Hedgehog workflow easier to learn. It is not a production deployment target and it is not intended to simulate xPU workload behavior, RoCE performance, or host tuning.

The logical model uses OCP Open Cluster Designs terminology:
- `SO-C/storage` is the converged host-facing fabric used for service, storage, and operator-facing connectivity.
- `scale-out` is the high-bandwidth training fabric logically modeled for the xPU nodes.

What makes this composition different
- The logical model describes 32 xPU-node servers with `2x25GbE` SO-C/storage links and `8x400GbE` scale-out links.
- The VLAB-deployable subset instantiates only the SO-C/storage fabric.
- The host-facing multihoming model in the deployable subset uses `ESLAG` for a simpler learning experience in Hedgehog VLAB.

Variant
- [`virtual-soc-storage-eslag--fe-2x25g/`](virtual-soc-storage-eslag--fe-2x25g/README.md)
  Virtual teaching composition for learning the workflow from connectivity map to NetBox model to Hedgehog wiring deployment.

Common attributes
- Logical xPU-node count: 32 servers
- Logical xPUs: 256 total
- Logical SO-C/storage links per server: 2x25GbE
- Logical scale-out links per server: 8x400GbE
- VLAB subset: SO-C/storage fabric only
- VLAB host multihoming: ESLAG
- Deployable lab goal: verify intent translation and basic end-to-end connectivity

Notes
- The files in this folder are scaffolding for learning and documentation.
- `topology-map.yaml` is a starter draft and is expected to need follow-up updates to match the latest authoring schema.
- `wiring.yaml` and `vpc.yaml` are written as teachable starting points for the VLAB subset, not as final production artifacts.

See also
- Compositions overview: [`../README.md`](../README.md)
