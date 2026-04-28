# OPG-256 — Rail-Optimized Clos Building Block (CX7 1×400G; BF3 2×200G)

This building block provides a 256-xPU rail-optimized Clos network for 32 eight-GPU servers. Each GPU connects to the backend via a single ConnectX-7 400G NIC (8 NICs per server, one per rail), while each server attaches to the frontend via a BlueField-3 DPU with two 200G ports (L3MH across two leaves). This OPG does not include an XOC spine; uplink ports are reserved on each leaf for XOC connectivity (see Composer requirements below).

The rail-optimized wiring maps each pair of rails to a dedicated backend leaf switch, which means every backend leaf serves all 32 servers but only two of their eight GPUs. This produces the same switch count and bandwidth as a standard Clos but constrains the cabling pattern to simplify deployment and troubleshooting. The frontend fabric converges storage (NVMe-oF/RoCEv2), scale-out control-plane, and in-band management onto a single leaf-spine tier with EVPX/VXLAN isolation via Hedgehog VPCs.

## Assets

- `connectivity-map.csv` — end-to-end cabling and port mapping
- `topology-map.yaml` — topology authoring plan (DS5000-based)
- `wiring/` — Hedgehog Wiring CRDs
  - `wiring-backend.yaml` — backend fabric wiring
- `diagrams/` — visual diagrams
  - `hhfab/backend.drawio` — backend topology diagram (draw.io)
- `netbox_inventory.json` — NetBox inventory export

## Attributes

- **Multihoming:** L3MH (ECMP) on both backend and frontend; no MLAG/ESLAG required
- **Backend fabric:** 4 DS5000 leaves + 4 DS5000 spines; ports 1-32 use 2x400G breakout for server downlinks, ports 33-64 at 800G for spine uplinks
- **Frontend fabric:** 2 DS5000 leaves + 2 DS5000 spines; odd ports 1-27 use 4x200G breakout for endpoint downlinks (56 logical ports per leaf), ports 33-64 at 800G for spine uplinks
- **Rail mapping:** be-leaf-01 = rails 1,2; be-leaf-02 = rails 3,4; be-leaf-03 = rails 5,6; be-leaf-04 = rails 7,8
- **Optics:** OS2 single-mode DR-class throughout (OSFP 2x400G DR4 for backend/uplinks, OSFP 4x200G DR4 for frontend downlinks, SFP+ 10G LR for OOB)
- **OOB:** 3x DS1000-48 switches carrying 120 endpoints (56 BMCs + 64 PDUs) with 10G SFP+ uplinks to frontend spines

## Considerations

- **Backend controller ports:** The Hedgehog backend controller (hh-ctrl-be) uses one 2x400G breakout port on be-leaf-01 and be-leaf-02 (port E1/33), reducing those leaves' spine uplinks from 32 to 31. This is a negligible bandwidth impact (<4%) and avoids displacing any server connections.
- **Frontend port capacity:** 14 odd ports with 4x200G breakout yield 56 logical ports per FE leaf. With 55 endpoints connected (32 compute + 10 storage + 10 metadata + 2 gateways + 1 controller), there is 1 spare 200G port per leaf for future expansion.
- **Vendor neutrality:** NVIDIA B200/CX7/BF3 are used as the reference GPU/NIC/DPU platform. The network design (switch counts, speeds, port zoning, rail mapping) works equally well with any vendor's hardware at equivalent speeds and link counts.

## Switch Summary

| Role | Model | Count | Network |
|------|-------|-------|---------|
| Backend leaf | Celestica DS5000 | 4 | backend |
| Backend spine | Celestica DS5000 | 4 | backend |
| Frontend leaf | Celestica DS5000 | 2 | frontend |
| Frontend spine | Celestica DS5000 | 2 | frontend |
| OOB | Celestica DS1000-48 | 3 | oob |
| **Total** | | **15** | |

## Composer requirements

To form a functional XOC cluster using this OPG, the composer must supply:

1. **XOC backend spine switches** (DS5000): cable to the 32×800G reserved uplinks on each
   backend rail leaf (ports 33–64 per switch)
2. **XOC frontend spine switches** (DS5000): cable to the 32×800G reserved uplinks on each
   frontend leaf (even ports 2–64 per switch)
3. **External/border connectivity** (DS3000 or equivalent) for WAN/ISP uplinks if required

Note: The Hedgehog backend controller (hh-ctrl-be) uses port E1/33 on be-leaf-01 and
be-leaf-02, reducing those leaves' effective XOC uplink capacity from 32 to 31.

## References

- OPG-M Modular Cluster Building Block (2026-01-14)
- XOC-N Cluster Architecture (2026-01-14)
