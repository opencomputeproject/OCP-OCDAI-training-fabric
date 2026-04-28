# XOC-64 Virtual — Learning Topology

`xoc-64-virtual` is a teaching asset, not a production reference architecture composition.
Its purpose is to help operators understand how to use the real XOC assets that appear
throughout this repository: topology visuals, connectivity maps, and Hedgehog wiring
artifacts.

This virtual composition is intentionally small enough to run in the Hedgehog VLAB while
still resembling the workflow an operator would follow with physical switches and a real
cluster fabric.

## What this teaches

- how to examine a topology diagram and understand the roles of the nodes and switches
- how to read a `connectivity-map.csv` file and use it as a cabling plan
- how to take a Hedgehog wiring diagram and use it to instantiate a fabric
- how the same workflow applies to larger, real XOC assets in this repository

## What this is not

- not a production-ready xPU cluster
- not a performance or workload validation environment
- not a substitute for the larger physical XOC compositions in this repository

The virtual composition exists so users can learn the artifact workflow safely and quickly.
In a real deployment, the same style of `connectivity-map.csv` and wiring diagram can be
used to cable physical switches and then instantiate the fabric configuration.

## Assets in this directory

- `xoc-64-virtual.png` — rendered topology image for quick review
- `result/diagram.drawio` — editable topology source
- `connectivity-map.csv` — device-to-device and port-to-port cabling plan
- `include/wiring-diagram.yaml` — Hedgehog wiring diagram used to instantiate the fabric
- `fab.yaml` — HHFab composition input for this virtual exercise

## Recommended learning flow

### 1. Study the topology

Open `xoc-64-virtual.png` first. Use it to understand:

- which nodes represent the xPU servers
- which nodes represent the switch fabric
- how the servers connect into the fabric
- which parts of the topology are being modeled in the virtual lab

If you want to inspect or modify the diagram, open `result/diagram.drawio`.

### 2. Review the cabling plan

Open `connectivity-map.csv` and walk through the rows carefully.

This file is valuable because it shows the exact switch-to-switch and host-to-switch
connections. In a physical environment, an operator could use the same style of file to:

- cable the switches correctly
- confirm which interfaces should connect to which peers
- cross-check that the physical cabling matches the design intent

For this virtual XOC, the CSV is primarily a learning tool, but it is meant to teach the
same habit that applies to the physical XOC variants.

### 3. Review the wiring diagram

Open `include/wiring-diagram.yaml`.

The wiring diagram is the artifact that tells Hedgehog how to instantiate the fabric after
the physical topology exists. In a real deployment, the usual sequence is:

1. cable the switches according to `connectivity-map.csv`
2. connect a Hedgehog controller to the management network with the switch management ports
3. use the wiring diagram to instantiate the fabric

## Physical-switch workflow

With physical switches, a user could work from this directory and run:

```bash
hhfab build
```

That build flow prepares the controller artifacts, including a bootable ISO for a
Hedgehog controller. Once the controller is connected on the management VLAN with the
switch management ports and booted, Hedgehog can perform zero-touch provisioning and bring
up the fabric described by the wiring diagram.

## VLAB workflow

For `xoc-64-virtual`, the main workflow is the Hedgehog VLAB rather than physical
switches.

From this directory, run:

```bash
hhfab vlab up
```

For the virtual lab, this flow performs the `hhfab build` steps and also builds a
virtualized topology on the local host. In practical terms, the VLAB flow:

- instantiates the fabric topology defined for this exercise
- uses virtual switch and host components to model the network on one machine
- connects the virtual nodes according to the same topology represented in the diagram and
  connectivity map

This gives the learner a way to experience the same artifact flow end to end without
needing physical switches or a real xPU cluster.

## What to validate

After the VLAB is up, validate that the topology instantiated as expected.

Recommended checks:

- confirm the virtual switches and host nodes are present
- compare the running topology to `xoc-64-virtual.png`
- cross-check selected links against `connectivity-map.csv`
- verify that the fabric came up cleanly
- run basic connectivity checks between the modeled xPU nodes

The point is not to simulate realistic xPU workload behavior. The point is to prove that a
user can move from:

- topology image
- connectivity map
- wiring diagram
- running fabric

## Why this matters

If a user understands this virtual exercise, the larger real XOC assets become much easier
to approach. The operator mental model is the same:

- inspect the topology
- cable according to the connectivity map
- instantiate with the wiring diagram
- validate the resulting fabric

That is the core value of `xoc-64-virtual`.
