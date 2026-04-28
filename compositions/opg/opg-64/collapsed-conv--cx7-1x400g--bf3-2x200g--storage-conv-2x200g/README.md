# OPG‑64 — Collapsed Converged (CX7 1×400G, BF3 2×200G, Converged Storage)

## What this building block provides

A single converged leaf pair (2× DS5000) carries both the scale‑out (RDMA) backend and
the converged frontend (storage/in‑band/management) for up to 8 compute servers (64 xPUs).
Out‑of‑band management uses a DS1000 pair. No spine switch is active in this building
block; the topology includes a placeholder spine pair whose uplink ports are reserved for
future XOC connectivity or standalone growth.

This is the simplest OPG topology: one managed fabric instead of two, no dedicated
spine, and all endpoint types sharing the same leaf pair.

## Fabrics

- **Converged fabric** (Hedgehog‑managed): single DS5000 leaf pair carrying backend
  (RDMA/RoCEv2), frontend (storage NVMe‑oF, in‑band, control‑plane), and storage traffic
- **OoB fabric** (unmanaged): DS1000 pair for BMC and PDU management

## Endpoint summary

| Role | Count | Notes |
|------|-------|-------|
| Compute servers (8 xPU each) | 8 | 64 xPUs total |
| Storage servers | 3 | Hyperconverged |
| Metadata servers | 3 | |
| HH gateways | 2 | |
| HH controller | 1 | |

## Port budget

| Zone | Ports (per leaf) | Breakout | Logical speed | Purpose |
|------|-----------------|---------|--------------|---------|
| Backend server | 1–16 | 2×400G | 400G | xPU compute backend connections |
| Converged FE | 27,29,31,33,35,37 (6 ports) | 4×200G | 200G | FE/storage/in‑band |
| **XOC uplinks** | **26,28,30,32,34,36,38–63 (32 ports)** | **1×800G** | **800G** | **Reserved for XOC spine or expansion** |

## What a composer must supply

This OPG provides the leaf pair and all endpoint connections. To form a functional XOC
cluster, the composer must supply:

1. **XOC spine switches** — DS5000 (or equivalent) spine pair connected to the 32×800G
   reserved uplink ports per leaf
2. **Uplink cabling** — 32× OSFP-800G-DR8 cables per leaf from the reserved uplink zone
   to the XOC spine
3. **External/border connectivity** — DS3000 or equivalent for WAN/ISP uplinks if required
   (out of scope for this OPG)

The collapsed leaf pair can operate stand‑alone without a spine only if no external
connectivity or cross‑OPG traffic is required. For any multi‑OPG composition or external
egress, the XOC spine is required.

## Assumptions and constraints

- Port zoning: 4×200G breakouts are only valid on odd ports (27,29,31,…); 2×400G is
  unrestricted. Violating the odd‑port rule disrupts the uplink budget.
- The spine placeholder pair in the topology plan is not wired; post‑generation manual
  adjustment is required to activate it.
- 32×800G uplinks per leaf are reserved and must not be consumed by server connections.

## Attributes

- Backend: converged on DS5000 leaf pair (single plane)
- Multihoming: L3MH/ECMP (HNP validation shim is eslag; RA intent is L3MH)
- Scale‑out NICs: CX7 1×400G per GPU
- Frontend/storage NICs: BF3 2×200G per server
- Optics: OS2 SMF DR‑class (OSFP-800G-2×400G-DR4 for backend, OSFP-800G-4×200G-DR4 for FE)

## Assets

- `connectivity-map.csv` — end‑to‑end cabling and port mapping
- `topology-map.yaml` — topology authoring plan
- `wiring/wiring-backend.yaml` — backend fabric wiring (Hedgehog CRDs)
- `diagrams/hhfab/backend.drawio` — backend topology diagram
- `netbox_inventory.json` — NetBox inventory export

## See also

- OPG‑64 overview: `../README.md`
- Example XOC using this OPG: `../../../xoc-256/` (illustrative; not prescriptive)
- Compositions overview: `../../../README.md`
