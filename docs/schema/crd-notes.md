# Hedgehog CRD Notes

Wiring (Switch, Server, Connection):
- Use device IDs that match connectivity-map.csv node_id values.
- Model server multihoming with distinct Connection objects per link; L3MH is realized by routing, not MLAG/ESLAG.

VPC (VPC, VPCAttachment, External, ExternalAttachment, ExternalPeering):
- VPC → VRF; subnets → VNIs; EVPN/VXLAN overlay.
- For converged frontends, define QoS classes and DSCP mappings consistently across the fabric.

