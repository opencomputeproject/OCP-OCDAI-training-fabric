# XOC-256 Virtual — Overview

This folder contains a virtual learning composition that wraps a single `OPG-256 Virtual` teaching model as the smallest full-cluster entry point for operators who want to learn the RA artifact workflow.

The purpose is educational:
- show how an OPG-oriented artifact set can also be consumed from an XOC-oriented entry point
- preserve OCP `OPG` and `XOC` language
- give users a clear place to start if they think in terms of cluster bring-up instead of building-block bring-up

This is not a production XOC and it is not meant to represent real cluster scale validation.

Bundle
- `1x-OPG-256-virtual/`

Operator note
- The logical model still describes a 256-xPU composition.
- The deployable VLAB subset remains intentionally limited to the `SO-C/storage` fabric.

See also
- Compositions overview: [`../README.md`](../README.md)
