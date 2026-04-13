# OPG‑64 — Rail‑Optimized Clos (CX7 1×400G, BF3 2×200G, Converged Storage)

This variant applies a rail‑optimized Clos to a 64‑xPU training cluster. Scale‑out traffic uses CX7 1×400G per GPU (RoCE); frontend and storage use BF3 2×200G per server with L3 multihoming across two leaves. Storage is converged into the frontend network for simplicity at this tier.

Why choose it
- Dramatically reduces spine traffic when tenants are scheduled within a common first‑hop rail domain (rail‑n NICs on the same leaf), cutting the uplink congestion that drives most AI network performance issues.
- Aligns with DS5000 port zoning and scales cleanly to larger OPG tiers without changing wiring rules.
- Converged frontend reduces switch count while remaining production‑practical.

Attributes
- Backend plane: single plane (Clos, rail‑optimized)
- Scale‑out NICs: CX7 1×400G per GPU
- Frontend NICs: BF3 2×200G per server (L3MH)
- Storage: converged, 2×200G per server
- Optics: OS2 single‑mode DR‑class (default)
- DS5000 zoning: 4×200G breakouts on odd ports; 2×400G unrestricted; reserve 32×800G uplinks per leaf

Assets
- `connectivity-map.csv` — end‑to‑end cabling and port mapping
- `topology-map.yaml` — topology authoring plan (DS5000‑based)
- `wiring/` — Hedgehog Wiring CRDs
  - `wiring-backend.yaml` — backend fabric wiring
- `diagrams/` — visual diagrams
  - `hhfab/backend.drawio` — backend topology diagram (draw.io)
- `netbox_inventory.json` — NetBox inventory export

Scheduling note
- Prefer placing multi‑node jobs within the same first‑hop rail domain. Spreading nodes across domains forces scale‑out via the spine; packed placement keeps most collectives leaf‑local.

Related plans and notes

References
- OPG‑M System Architecture (2026‑01‑14): https://www.opencompute.org/documents/opg-m-system-architecture-final-14-january-2026-pdf
- XOC‑N System Architecture (2026‑01‑14): https://www.opencompute.org/documents/xoc-n-system-architecture-final-14-january-2026-pdf

See also
- Size overview: ../README.md
- Compositions overview: ../../README.md
