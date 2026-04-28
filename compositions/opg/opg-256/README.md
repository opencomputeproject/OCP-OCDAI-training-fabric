# OPG-256 — Variants Overview

Welcome. OPG-256 is a practical 256‑xPU building block (32 servers × 8 xPUs). It keeps the OPG concept approachable while offering two backend patterns operators commonly choose: rail‑optimized (simplicity + spine relief) and dual‑plane (isolation + bandwidth headroom).

The compositions in this repository are designed to work with air or liquid cooling at any
density. Cooling medium and rack density are physical deployment choices that do not affect
the network topology.

## Variants

- [`clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/`](clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md)
  Rail-optimized backend — 4 DS5000 leaves (2 rails per leaf); each leaf reserves 32×800G uplinks for XOC spine connectivity. Intra-rail collective traffic stays within a single leaf.

- [`dual-plane--cx8-2x400g--bf3-2x200g--2p--storage-conv-2x200g/`](dual-plane--cx8-2x400g--bf3-2x200g--2p--storage-conv-2x200g/README.md)
  Dual-plane backend — two independent backend fabrics (Plane A and Plane B). CX8 NICs provide 2×400G per GPU: port-1 to Plane A, port-2 to Plane B. Higher bandwidth and plane-level serviceability.

## Common Attributes

- **Frontend:** Converged (SO-C/Storage/In-band) with BF3 2×200G per endpoint, L3MH across two DS5000 FE leaves.
- **Switches:** DS5000 for 800/400/200G networks; DS1000-48 for OoB; DS2000 for in-band (optional).
- **Port zoning:** 4×200G on odd ports; 2×400G unrestricted; 32×800G uplinks per leaf.
- **Optics:** OS2 single-mode DR-class (default). Distance-friendly baseline; substitute optics per site survey.

## Notes

- Each variant folder contains: `connectivity-map.csv`, `topology-map.yaml`, `wiring/`, `diagrams/`, `netbox_inventory.json`.
- NVIDIA B200/B300 and BF3/CX7/CX8 are used as concrete examples; the network design is vendor-neutral for equivalent speeds and link counts.

See also
- Compositions overview: ../README.md
