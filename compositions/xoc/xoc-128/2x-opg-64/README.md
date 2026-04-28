# XOC‑128 / 2x OPG‑64 — Bundle Overview

This bundle composes two OPG‑64 training building blocks into a 128‑xPU XOC. At this size the design stays mesh-based rather than adding a dedicated spine tier, so the two 64‑xPU first-hop domains remain visible in the topology.

Variants
- [`mesh-conv-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g--inb-2x25g/`](mesh-conv-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g--inb-2x25g/README.md)
  Mesh-converged, rail-optimized scale-out
- [`mesh-conv-sh--cx7-1x400g--bf3-2x200g--storage-conv-2x200g--inb-2x25g/`](mesh-conv-sh--cx7-1x400g--bf3-2x200g--storage-conv-2x200g--inb-2x25g/README.md)
  Mesh-converged, single-homed scale-out

Notes
- These variants are derived from the existing `xoc-64/1x-opg-64` mesh compositions and expanded to the 128‑xPU bundle context.
- The scale-out fabric remains a two-leaf mesh, consistent with the current RA description for XOC‑128.

See also
- XOC‑128 tier overview: [`../README.md`](../README.md)
- XOC‑64 bundle pattern: [`../../xoc-64/1x-opg-64/README.md`](../../xoc-64/1x-opg-64/README.md)
