# XOC-256 Virtual — SO-C/Storage ESLAG VLAB Teaching Composition

This folder is the cluster-oriented companion to the `OPG-256 Virtual` teaching composition. The assets are organized so a user can approach the exercise as a full-cluster learning lab while still benefiting from the same simplified VLAB deployment target.

## Purpose

This composition teaches three ideas:
1. A reference architecture can publish multiple artifact types that describe the same topology from different angles.
2. The connectivity map explains how the topology should be cabled.
3. The Hedgehog wiring diagram translates that intent into a configured fabric.

## Logical model versus deployed lab

- The logical model represents a `256-xPU` cluster made of `32 xPU-node servers`.
- The logical model includes both `SO-C/storage` and `scale-out` fabrics.
- The deployable lab uses only the `SO-C/storage` fabric.
- The deployable lab uses four representative Linux hosts instead of thirty-two full logical xPU-node objects.

## Assets

- `connectivity-map.csv` — logical cluster map with representative scale-out rows
- `connectivity-map-vlab-soc-storage.csv` — reduced cluster bring-up map for the lab subset
- `netbox_inventory.json` — cluster-oriented inventory scaffold
- `topology-map.yaml` — draft topology authoring scaffold
- `wiring.yaml` — starter Hedgehog wiring diagram for the deployable subset
- `vpc.yaml` — starter overlay example for the deployable subset
- `bom.yaml` — teaching BOM

## Suggested learning flow

1. Read `connectivity-map-vlab-soc-storage.csv` and trace the lab subset.
2. Read `connectivity-map.csv` and compare the lab subset to the larger logical cluster model.
3. Open `netbox_inventory.json` to see how the cluster-level inventory can describe more than what is physically deployed in the lab.
4. Review `topology-map.yaml` as the draft source artifact for future automation.
5. Use `wiring.yaml` to stand up the VLAB subset.
6. Validate switch discovery, host attachment, and host-to-host ping.

## Caveats

- This content is intentionally educational and scaffold-first.
- The `topology-map.yaml` file is expected to need follow-up work to match the latest schema.
- The `wiring.yaml` file is focused on the deployable subset, not the full logical cluster.

See also
- Bundle overview: [`../README.md`](../README.md)
- OPG teaching variant: [`../../../../OPG-256-virtual/virtual-soc-storage-eslag--fe-2x25g/README.md`](../../../../OPG-256-virtual/virtual-soc-storage-eslag--fe-2x25g/README.md)
