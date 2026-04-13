# XOC‑256 / 2× OPG‑128 — Rail‑Optimized Clos (CX7 1×400G, BF3 2×200G)

## Composed topology

Two OPG‑128 building blocks (16 servers each) are connected under a shared DS5000 spine
layer to form a 256‑xPU training cluster. The XOC spine consists of DS5000 backend spine
switches (be‑spine) and DS5000 frontend spine switches (fe‑spine).

**Backend fabric:** Each compute server connects 8 CX7 1×400G NICs to 8 dedicated backend
rail‑leaf switches (one NIC per rail, rail‑optimized distribution). Each rail‑leaf's
32×800G uplinks (ports 33–64) connect to the XOC backend spine, which provides full‑mesh
ECMP across all rail leaves from both OPG‑128 units.

**Frontend fabric:** Each server connects a BF3 2×200G DPU to 2 frontend leaf switches
(L3MH/ECMP). Frontend leaf uplinks (32×800G per leaf, even ports 2–64) connect to the
XOC frontend spine. Storage, in‑band management, and control‑plane traffic share the
frontend fabric.

**OoB:** DS1000 per OPG‑128 unit for BMC and PDU management.

## Composition attributes

| Attribute | Value |
|-----------|-------|
| Total xPUs | 256 (32 servers × 8 xPUs) |
| Backend topology | Rail‑optimized Clos (8 rail leaves, 1 NIC/GPU/rail) |
| Backend spine | DS5000 (XOC‑level; connects rail‑leaf uplinks across both OPG units) |
| Frontend topology | 2‑stage Clos (FE leaves + FE spine) |
| Scale‑out NICs | CX7 1×400G per GPU |
| Frontend NICs | BF3 2×200G per server (L3MH) |

## Why this composition

OPG‑128 alone provides 128 xPUs in a single isolated cluster. Composing two OPG‑128
units under an XOC spine doubles compute capacity to 256 xPUs while reusing the same
wiring rules and DS5000 switch family. The rail‑optimized pattern keeps intra‑rail
collectives leaf‑local; the XOC spine carries cross‑rail traffic that cannot be avoided
when jobs span both OPG units.

Choose this composition over 1× OPG‑256 when you want to grow incrementally (start with
one OPG‑128 and add the second later) or when separate OPG procurement and staging is
operationally preferred.

## Tradeoffs

- **Rail‑optimized backend:** jobs placed within a single OPG‑128 rail domain keep most
  scale‑out traffic leaf‑local. Jobs spanning both OPG units generate cross‑spine traffic.
- **DS1000 OoB is per OPG unit:** a unified OoB fabric across both units requires
  additional planning outside this composition.

## OPG‑128 building blocks

This composition uses two instances of OPG‑128 clos‑ro. For the building‑block spec
(port budget, uplink reservations, constraints), see:
`../../../OPG-128/clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/README.md`

## How to use

1. Deploy and validate each OPG‑128 unit independently using the OPG‑128 assets.
2. Install XOC spine switches (be‑spine and fe‑spine, DS5000).
3. Cable the 32×800G uplinks from each backend rail‑leaf to the be‑spine, and the
   32×800G uplinks from each frontend leaf to the fe‑spine.
4. Import `netbox_inventory.json` into NetBox (optional) and apply Hedgehog Wiring CRDs
   from the `wiring/` folder.
5. For best results, schedule multi‑node jobs within a single OPG‑128 rail domain to keep
   most scale‑out traffic leaf‑local. Jobs that span OPG units will cross the spine.

## Assets

- `connectivity-map.csv` — end‑to‑end cabling and port mapping
- `topology-map.yaml` — topology authoring plan (DS5000‑based)
- `wiring/` — Hedgehog Wiring CRDs (per fabric)
- `diagrams/` — visuals (per fabric)
- `netbox_inventory.json` — NetBox inventory export

## See also

- Bundle overview: `../README.md`
- OPG‑128 building blocks: `../../../OPG-128/README.md`
- Compositions overview: `../../../README.md`
