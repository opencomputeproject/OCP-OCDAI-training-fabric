# Connectivity Map Schema (CSV)

Required columns (one row per physical link):
- node_type (switch|server|gateway|controller|oob-switch)
- node_id (stable identifier)
- port (physical port label)
- peer_type (switch|server|gateway|controller|oob-switch)
- peer_id
- peer_port
- speed (e.g., 800G, 400G, 200G, 10G)
- media_type (e.g., OSFP-2x400G-DR4, QSFP112-400G-DR4, QSFP56/112-200G-DR4, SFP+-10G-LR)
- network_type (backend|frontend|oob)
- lag_id (if applicable; else empty)
- plane (A|B or single)
- comments (optional)

