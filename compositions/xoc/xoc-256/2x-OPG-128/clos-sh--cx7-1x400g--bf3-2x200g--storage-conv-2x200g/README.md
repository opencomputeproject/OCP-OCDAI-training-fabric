# XOC‑256 / 2× OPG‑128 — Single‑Homed Clos (CX7 1×400G, BF3 2×200G)

## Composed topology

Two OPG‑128 building blocks (16 servers each) are connected under a shared DS5000 spine
layer to form a 256‑xPU training cluster. The XOC spine consists of DS5000 backend spine
switches (be‑spine) and DS5000 frontend spine switches (fe‑spine).

**Backend fabric:** Each server's backend NICs terminate on a single backend leaf (8 servers
per leaf, single‑homed). Backend leaf uplinks (32×800G per leaf, ports 33–64) connect to
the XOC backend spine.

**Frontend fabric:** Each server connects a BF3 2×200G DPU to 2 frontend leaf switches
(L3MH/ECMP). Frontend leaf uplinks (32×800G per leaf, even ports 2–64) connect to the
XOC frontend spine. Storage, in‑band management, and control‑plane traffic share the
frontend fabric.

**OoB:** DS1000 per OPG‑128 unit for BMC and PDU management.

## Composition attributes

| Attribute | Value |
|-----------|-------|
| Total xPUs | 256 (32 servers × 8 xPUs) |
| Backend topology | Single‑homed Clos (8 servers per backend leaf) |
| Backend spine | DS5000 (XOC‑level; connects backend leaf uplinks across both OPG units) |
| Frontend topology | 2‑stage Clos (FE leaves + FE spine) |
| Scale‑out NICs | CX7 1×400G per GPU |
| Frontend NICs | BF3 2×200G per server (L3MH) |

## Why this composition

Single‑homed backend provides clearer per‑leaf failure domains and simpler host cabling
than rail‑optimized (no rail‑domain constraint on NIC assignment). This aligns with the
OPG‑M exemplar backend topology. Choose this composition when straightforward operations
and predictable failure isolation are priorities.

## Tradeoffs

- **Single‑homed backend:** any job spanning servers on different backend leaves generates
  cross‑leaf traffic via the XOC spine; there is no rail‑domain locality benefit.
- **Simpler cabling:** NIC-to-leaf assignment is by server, not by GPU rail number.
- **DS1000 OoB is per OPG unit:** a unified OoB fabric across both units requires
  additional planning outside this composition.

## OPG‑128 building blocks

This composition uses two instances of OPG‑128 clos‑sh. For the building‑block spec
(port budget, uplink reservations, constraints), see:
`../../../opg-128/clos-sh--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md`

## How to use

1. Deploy and validate each OPG‑128 unit independently using the OPG‑128 assets.
2. Install XOC spine switches (be‑spine and fe‑spine, DS5000).
3. Cable the 32×800G uplinks from each backend leaf to the be‑spine, and the
   32×800G uplinks from each frontend leaf to the fe‑spine.
4. Import `netbox_inventory.json` into NetBox (optional) and apply Hedgehog Wiring CRDs
   from the `wiring/` folder.

## Assets

- `connectivity-map.csv` — end‑to‑end cabling and port mapping
- `topology-map.yaml` — topology authoring plan (DS5000‑based)
- `wiring/` — Hedgehog Wiring CRDs (per fabric)
- `diagrams/` — visuals (per fabric)
- `netbox_inventory.json` — NetBox inventory export

## See also

- Bundle overview: `../README.md`
- OPG‑128 building blocks: `../../../opg-128/README.md`
- Compositions overview: `../../../README.md`
