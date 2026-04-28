# XOC‑128 / 1x OPG‑128 — Bundle Overview

This bundle wraps one OPG‑128 training building block in the XOC catalog so a 128‑xPU deployment can be referenced consistently alongside the larger XOC tiers.

Variants
- [`clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/`](clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md)
  Rail-optimized backend
- [`clos-sh--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/`](clos-sh--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md)
  Single-homed backend

Notes
- These variants intentionally reuse the maintained `opg-128` topology assets and only reframe them under the XOC catalog.
- No additional spine tier is introduced here; this bundle exists to keep the XOC catalog complete at the 128‑xPU size point.

See also
- XOC‑128 tier overview: [`../README.md`](../README.md)
- OPG‑128 building-block specs: [`../../../opg/opg-128/README.md`](../../../opg/opg-128/README.md)
