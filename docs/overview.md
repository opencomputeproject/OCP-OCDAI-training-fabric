# Overview

This repository provides open, practical assets for the AI Training Reference Architecture. Assets are grouped by composition size (OPG/XOC) and variant. Each variant includes:

- connectivity-map.csv — end‑to‑end cabling and port mapping
- bom.yaml — switch, optics, and cable counts
- wiring.yaml — Hedgehog Wiring CRDs (Switch, Server, Connection)
- vpc.yaml — Hedgehog VPC CRDs (VPC, VPCAttachment, External, etc.)
- diagrams/ — SVG/PNG diagrams that match the connectivity map

All assets align with the OCP OPG‑M/XOC‑N system architecture papers and use OS2 single‑mode DR‑class optics by default.

