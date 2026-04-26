# OPG-256 Virtual — SO-C/Storage ESLAG VLAB Teaching Composition

This composition is a learning asset. It shows how a user can start from reference-architecture artifacts, understand the intended topology, and then use a Hedgehog wiring diagram to instantiate a working fabric in VLAB.

It is intentionally not a real AI training cluster. The objective is to teach the operator workflow:
1. Review the connectivity map.
2. Understand the logical inventory and cabling plan.
3. Inspect the NetBox-oriented inventory scaffold.
4. Apply the Hedgehog wiring diagram for the deployable subset.
5. Validate that the fabric comes up and basic host-to-host connectivity works.

## Important caveats

- This is a pedagogical composition, not a production RA recommendation.
- The logical model includes both `SO-C/storage` and `scale-out` fabrics so the artifact set still resembles a real cluster composition.
- The deployable VLAB subset includes only the `SO-C/storage` fabric.
- `ESLAG` is used in the VLAB subset because it produces a better out-of-box learning experience than `HostBGP` for vanilla lab hosts.
- The bandwidth choices in the lab subset are illustrative. `2x25GbE` is used to make the topology easier to inspect; the point is artifact workflow, not performance.

## Composition attributes

- Scope: OPG teaching composition
- Logical xPU-node servers: 32
- Logical xPUs represented: 256
- Logical `SO-C/storage` fabric: 2 leaves, 2 spines, `2x25GbE` per server
- Logical `scale-out` fabric: 4 leaves, 2 spines, `8x400GbE` per server
- VLAB-deployable subset: `SO-C/storage` only
- Host-facing multihoming in VLAB subset: ESLAG
- Leaf-to-spine uplinks in VLAB subset: 2 uplinks per leaf total, one to each spine

## What each asset is for

- `connectivity-map.csv`
  Logical composition map. It shows the full teaching topology and includes representative scale-out rows so users can understand the relationship between server NICs and scale-out leaves.
- `connectivity-map-vlab-soc-storage.csv`
  Reduced map for the deployable VLAB subset. This is the easiest file to study before launching the lab.
- `netbox_inventory.json`
  Inventory scaffold for the logical teaching composition. It is meant to show how a NetBox-oriented artifact can model a more complete topology than the one actually instantiated in VLAB.
- `topology-map.yaml`
  Starter authoring plan for the logical composition. This file is expected to need updates to match the latest schema revisions.
- `wiring.yaml`
  Starter Hedgehog wiring diagram for the VLAB-deployable `SO-C/storage` subset.
- `vpc.yaml`
  Starter overlay example for the same VLAB subset.
- `bom.yaml`
  Teaching BOM summarizing the logical model and the deployable subset.

## Course-style walkthrough

### Lesson 1: Read the topology

Start with `connectivity-map-vlab-soc-storage.csv`.
- Identify the two `SO-C/storage` leaves and the two spines.
- Trace how each lab host is dual-attached to the leaf pair.
- Confirm that each leaf has one uplink to each spine.

Then open `connectivity-map.csv`.
- Observe that the logical model extends the same host set to a scale-out fabric.
- Notice that the scale-out rows are present to teach intent and naming, even though that fabric is not instantiated in VLAB.

### Lesson 2: Inspect the inventory model

Open `netbox_inventory.json`.
- Review the logical device groups for leaves, spines, and xPU-node servers.
- Note how one inventory artifact can describe more of the intended cluster than the deployable lab actually spins up.

### Lesson 3: Understand the authoring plan

Open `topology-map.yaml`.
- Treat it as a draft source-of-truth scaffold for the virtual composition.
- Expect follow-up edits before using it as a fully authoritative generator input.

### Lesson 4: Launch the lab

Use the VLAB workflow around `wiring.yaml`.
- Generate or prepare the VLAB environment.
- Apply the VLAB subset wiring.
- Apply `vpc.yaml` if you want a minimal overlay example.

### Lesson 5: Validate the result

Suggested checks:
- Confirm the switches and servers appear in the Hedgehog inventory.
- Confirm the host-facing links are up on both leaves.
- Verify basic connectivity between at least two lab hosts with `ping`.
- Inspect the resulting wiring state and compare it back to `connectivity-map-vlab-soc-storage.csv`.

## Expected learning outcome

By the end of this exercise, a user should understand the main point of the companion repository artifacts: the connectivity map communicates how to cable the topology, the inventory/topology artifacts communicate the intended model, and the Hedgehog wiring diagram turns that declared intent into a configured fabric.

## Status

- This folder is intentionally scaffold-first.
- `topology-map.yaml` is a draft starter because the authoring schema has evolved.
- Follow-on work is expected to refine generation details and diagrams.

## References

- OPG-M System Architecture (2026-01-14): https://www.opencompute.org/documents/opg-m-system-architecture-final-14-january-2026-pdf
- XOC-N System Architecture (2026-01-14): https://www.opencompute.org/documents/xoc-n-system-architecture-final-14-january-2026-pdf

See also
- Size overview: [`../README.md`](../README.md)
- Compositions overview: [`../../README.md`](../../README.md)
