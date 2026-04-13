# OPG‑512 — Variants Overview

Welcome. OPG‑512 scales the pod‑sized building block to 512 xPUs. Operators typically choose between:
- Rail‑optimized (CX7 1×400G): simpler wiring and better spine relief with rail‑local placement.
- Dual‑plane (CX8 2×400G): higher isolation and bandwidth headroom.

The compositions in this repository are designed to work with air or liquid cooling at any
density. Cooling medium and rack density are physical deployment choices that do not affect
the network topology.

Variants
- [`clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/`](clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md)
  Rail‑optimized backend (CX7 1×400G)
- [`dual-plane--cx8-2x400g--bf3-2x200g--2p--storage-conv-2x200g/`](dual-plane--cx8-2x400g--bf3-2x200g--2p--storage-conv-2x200g/README.md)
  Dual‑plane backend (CX8 2×400G)

Common attributes
- Frontend: BF3 2×200G per server (L3MH); storage converged at this tier
- Switches: DS5000 for 800/400/200G; DS3000 optional for border; DS1000 (OoB); DS2000 (in‑band)
- Zoning: 4×200G on odd ports; 2×400G unrestricted; preserve 32×800G uplinks per leaf
- Optics: OS2 single‑mode DR‑class (default)

Notes
- Compositions overview: ../README.md
