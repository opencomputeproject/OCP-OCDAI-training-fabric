# Usage

Connectivity maps
- CSV columns are documented in docs/schema/connectivity-map.schema.md.
- Use these to verify cabling, generate labels, and compare against live discovery.

BOMs
- YAML keys are documented in docs/schema/bom.schema.md.
- Adjust counts to your procurement; keep optic types consistent with distance requirements.

Hedgehog CRDs
- Apply `wiring.yaml` and `vpc.yaml` to the Hedgehog controller to realize the topology.
- Review docs/schema/crd-notes.md for assumptions and common parameters.

Diagrams
- Diagrams visualize the same IDs as the connectivity CSV. Keep changes in sync.

