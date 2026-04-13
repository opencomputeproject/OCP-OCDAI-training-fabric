# BOM Schema (YAML)

Top-level keys:
- switches:
  - model: string (e.g., Celestica DS5000)
    role: string (server-leaf|spine|border-leaf|oob)
    count: integer
- optics:
  - type: string (e.g., OSFP-2x400G-DR4, QSFP112-400G-DR4)
    speed: string (e.g., 400G)
    count: integer
- cables:
  - type: string (e.g., MPO-8 trunk, LC duplex)
    length: string or number (e.g., 150m)
    count: integer
- gateways:
  - model: string
    count: integer
- notes: string

