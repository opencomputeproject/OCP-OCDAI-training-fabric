# DIET Topology Plan YAML Reference

This is the current reference for YAML-defined DIET topology plans
consumed by the Hedgehog NetBox Plugin (HNP).

## What this file is

- It documents the input format consumed by:
  - `netbox_hedgehog/test_cases/loader.py`
  - `netbox_hedgehog/test_cases/schema.py`
  - `netbox_hedgehog/test_cases/ingest.py`
- It documents the authoring format for creating a DIET topology plan inside NetBox.
- The primary contract is **`diet/v2`**. Legacy v1 is supported for
  backwards compatibility but should not be used for new files.

## What it is not

- It is not the exported Hedgehog wiring YAML (the `Connection`, `Switch`, and
  `Server` CRDs that `hhfab` consumes).
- It is not the NetBox REST API shape.

## Fast rules (v2)

- Top-level required fields: `apiVersion`, `kind`, `metadata`, `spec`.
- `apiVersion` must be exactly `diet/v2`.
- `kind` must be exactly `TopologyPlan`.
- `metadata.managed_by` must be exactly `yaml`.
- `metadata.case_id` must match `^[a-z0-9_]+$`.
- `metadata.version` must be the integer `2`.
- `spec.plan.status` must be one of: `draft`, `review`, `approved`, `exported`.
- `reference_data` is **forbidden** in v2 — the schema rejects it. Use
  `test_fixtures` for test-time object creation, or rely on pre-seeded inventory.
- Device types, breakout options, and module types are resolved from pre-seeded
  inventory by slug or composite key — not symbolic local IDs.
- In `spec.switch_classes[]`, use `device_type: <slug>` (not `device_type_extension`).
- In `spec.switch_port_zones[]`, `breakout_option` is a `breakout_id` string.
- In `spec.server_nics[]`, `module_type` is a slug string or a
  `{manufacturer, model}` composite lookup.
- Do not use deprecated keys:
  - `server_connections[].target_switch_class`
  - `server_connections[].nic_module_type`
- Use:
  - `server_connections[].target_zone: "<switch_class_id>/<zone_name>"`
  - `server_connections[].nic: "<nic_id>"`

## Minimal valid v2 example

This example uses `test_fixtures` so it is self-contained on any fresh
installation. In production plans backed by pre-seeded inventory, omit
`test_fixtures` and reference the existing slugs directly.

```yaml
apiVersion: diet/v2
kind: TopologyPlan
metadata:
  case_id: minimal_v2_example
  name: Minimal v2 Example
  version: 2
  managed_by: yaml

test_fixtures:
  device_types:
    - slug: ex-switch
      manufacturer: generic
      model: Example Switch
      interface_templates:
        - name: E1/1
          type: 800gbase-x-osfp
        - name: E1/2
          type: 800gbase-x-osfp
        - name: E1/3
          type: 800gbase-x-osfp
        - name: E1/4
          type: 800gbase-x-osfp
      device_type_extension:
        hedgehog_roles: [server-leaf]
        uplink_ports: 0
        native_speed: 800
        supported_breakouts: []
    - slug: ex-server
      manufacturer: generic
      model: Example Server
  breakout_options:
    - breakout_id: ex-1x800g
      from_speed: 800
      logical_ports: 1
      logical_speed: 800
  module_types:
    - manufacturer: generic
      model: Example NIC 1x800G

spec:
  plan:
    name: Minimal v2 Example
    status: draft

  switch_classes:
    - switch_class_id: leaf
      fabric_name: backend
      fabric_class: managed
      hedgehog_role: server-leaf
      device_type: ex-switch
      uplink_ports_per_switch: 0

  switch_port_zones:
    - switch_class: leaf
      zone_name: server-ports
      zone_type: server
      port_spec: 1-4
      breakout_option: ex-1x800g

  server_classes:
    - server_class_id: gpu
      quantity: 2
      server_device_type: ex-server

  server_nics:
    - server_class: gpu
      nic_id: be
      module_type:
        manufacturer: generic
        model: Example NIC 1x800G

  server_connections:
    - server_class: gpu
      connection_id: be
      nic: be
      port_index: 0
      ports_per_connection: 1
      hedgehog_conn_type: unbundled
      distribution: same-switch
      target_zone: leaf/server-ports
      speed: 800
      port_type: data
```

## Section reference (v2)

### `apiVersion` and `kind`

Both are required in v2.

- `apiVersion` must be exactly `diet/v2`.
- `kind` must be exactly `TopologyPlan`.

### `metadata`

Required. Must be a mapping.

Required fields:

- `case_id` — lowercase snake case (`^[a-z0-9_]+$`)
- `name` — human-readable display name
- `version` — must be the integer `2`
- `managed_by` — must be the literal string `yaml`

Optional fields:

- `description`
- `tags`

### `spec`

Required. All topology payload lives under `spec`.

#### `spec.plan`

Required. Describes the NetBox topology plan object.

Required:

- `name`
- `status`

Optional:

- `description`

Valid `status` values: `draft`, `review`, `approved`, `exported`.

#### `spec.switch_classes[]`

Required fields:

- `switch_class_id`
- `fabric_name`
- `device_type` — DeviceType slug (resolved from pre-seeded inventory)

Optional fields:

- `fabric_class` — `managed` or `unmanaged`
- `hedgehog_role` — `spine`, `server-leaf`, `border-leaf`, `virtual`
- `uplink_ports_per_switch`
- `mclag_pair`
- `override_quantity`
- `redundancy_type` — `mclag` or `eslag`
- `redundancy_group`
- `topology_mode` — `spine-leaf` or `prefer-mesh`
- `groups`

Rules:

- `device_type` is a DeviceType slug resolved from pre-seeded inventory.
  The corresponding `DeviceTypeExtension` is looked up automatically.
- If `redundancy_type` is set, `redundancy_group` is required.
- If `mclag_pair: true` and `redundancy_type` is omitted, the model
  auto-fills `redundancy_type: mclag` and a generated `redundancy_group`.
- `prefer-mesh` cannot coexist with a spine class in the same fabric.

#### `spec.switch_port_zones[]`

Not required by the schema, but normally required because `server_connections`
target zones.

Required fields:

- `switch_class` — points to `spec.switch_classes[].switch_class_id`
- `zone_name`
- `zone_type`
- `port_spec`

Optional fields:

- `breakout_option` — a `breakout_id` string resolved from pre-seeded inventory
- `allocation_strategy`
- `allocation_order`
- `priority`
- `peer_zone` — `"<switch_class_id>/<zone_name>"`

Valid `zone_type` values: `server`, `uplink`, `mclag`, `peer`, `session`, `oob`, `fabric`, `mesh`.

Valid `allocation_strategy` values: `sequential`, `interleaved`, `spaced`, `custom`.

`port_spec` uses the DIET port-spec grammar, for example:

- `1-32`
- `1-63:2`
- `1,3,5,7`

If `allocation_strategy: custom`, then `allocation_order` is required and must
exactly match the parsed port list.

#### `spec.server_classes[]`

Required fields:

- `server_class_id`
- `quantity` — must be at least `1`
- `server_device_type` — DeviceType slug resolved from pre-seeded inventory

Optional fields:

- `description`
- `category` — `gpu`, `storage`, or `infrastructure`
- `gpus_per_server`

#### `spec.server_nics[]`

NIC-first modeling is required for all v2 cases.

Required fields:

- `server_class` — points to `spec.server_classes[].server_class_id`
- `nic_id` — alphanumeric characters, `_`, and `-`; must start with alphanumeric
- `module_type` — either a slug string or a `{manufacturer, model}` composite
  resolved from pre-seeded inventory

Optional fields:

- `description`

#### `spec.server_connections[]`

Required fields:

- `server_class` — points to `spec.server_classes[].server_class_id`
- `connection_id` — alphanumeric, `_`, and `-`
- `nic` — points to `spec.server_nics[].nic_id` within the same server class
- `ports_per_connection`
- `target_zone` — `"<switch_class_id>/<zone_name>"`
- `speed`

Optional fields:

- `connection_name`
- `port_index` — zero-based
- `hedgehog_conn_type` — `unbundled`, `bundled`, `mclag`, `eslag`
- `distribution` — `same-switch`, `alternating`, `rail-optimized`
- `rail`
- `port_type` — `data`, `ipmi`, `pxe`
- `cage_type` — `QSFP112`, `OSFP`, `QSFP-DD`, `QSFP28`, `SFP28`, `SFP+`, `RJ45`
- `medium` — `MMF`, `SMF`, `DAC`, `ACC`
- `connector` — `LC`, `MPO-12`, `MPO-16`, `Direct`
- `standard`

Rules:

- Connections may only target `server` or `oob` zones.
- IPMI connections must target an `oob` zone.
- `distribution: alternating` with `hedgehog_conn_type` of `bundled`, `mclag`,
  or `eslag` requires the target switch class to have `redundancy_type` set.
- `distribution: rail-optimized` should include `rail`.
- `port_index + ports_per_connection` must fit within the NIC module type's
  interface template count.

### `status` (optional)

A machine-written block populated by the `dump_plan_status` command. On ingest
this section is **ignored** — it does not affect the topology plan state in NetBox.

When present, the schema enforces that generation counts are integers.

```yaml
status:
  calculated_at: "2026-05-11T00:00:00Z"
  generation:
    status: generated
    device_count: 175
    interface_count: 1313
    cable_count: 1017
    generated_at: "2026-05-11T00:01:00Z"
```

### `contract` (optional)

Assertion block validated against the live NetBox state after apply. On ingest
this section is **ignored** — it is used by `validate_contract` assertions only.

Supported sub-sections:

- `counts` — expected object counts (integers)
- `generation` — expected generation counts (integers); skipped if no
  `GenerationState` exists
- `zones` — required zone assertions
- `topology` — topology-shape assertions

Example:

```yaml
contract:
  counts:
    server_classes: 2
    switch_classes: 4
    connections: 6
  generation:
    device_count: 175
    interface_count: 1313
    cable_count: 1017
```

Note on `counts.connections`: this is the number of `PlanServerConnection`
objects created from `spec.server_connections[]`, not the number of exported
Hedgehog wiring `Connection` CRDs. A single entry can expand into many CRDs
after device generation and YAML export.

### `test_fixtures` (optional)

Provides test-time object bootstrap. Use this to create DeviceTypes, BreakoutOptions,
and ModuleTypes that do not exist in pre-seeded inventory, for example in unit tests
or portable examples. Unlike v1 `reference_data`, these objects are created with
`get_or_create` semantics so the block is idempotent.

Supported sub-sections:

- `device_types[]` — each entry may include `slug`, `manufacturer`, `model`,
  `interface_templates`, and an inline `device_type_extension` mapping
- `breakout_options[]` — each entry requires `breakout_id`; optional `from_speed`,
  `logical_ports`, `logical_speed`
- `module_types[]` — each entry requires `manufacturer` and `model`

Production plans backed by pre-seeded inventory should omit `test_fixtures`
and reference slugs directly.

## Reference resolution in v2

v2 resolves references from pre-seeded NetBox inventory using stable identifiers:

| Field | Lookup key |
|---|---|
| `spec.switch_classes[].device_type` | DeviceType slug |
| `spec.switch_port_zones[].breakout_option` | BreakoutOption `breakout_id` |
| `spec.server_classes[].server_device_type` | DeviceType slug |
| `spec.server_nics[].module_type` (string form) | ModuleType slug |
| `spec.server_nics[].module_type` (dict form) | `{manufacturer, model}` composite |

When a referenced object is missing and `reference_mode=require`, ingest raises
a `missing_reference` error. When `reference_mode=ensure` (the default), missing
reference objects are created or updated.

## Deprecated fields

Do not emit these in new YAML:

- `server_connections[].target_switch_class`
- `server_connections[].nic_module_type`
- `breakout_options[].optic_type` (never used by HNP; field is ignored if present
  in v1 `reference_data`, but omit it)

Use this pattern instead:

```yaml
spec:
  server_nics:
    - server_class: gpu-small
      nic_id: nic-fe
      module_type: gpu-server-fe-nic-slug

  server_connections:
    - server_class: gpu-small
      nic: nic-fe
      target_zone: fe-leaf/server-downlinks
```

## Validation command

Run inside the NetBox container:

```bash
cd /home/ubuntu/afewell-hh/netbox-docker
docker compose exec -T netbox python manage.py apply_diet_test_case --case <case_id> --dry-run
```

Useful flags:

- `--require-reference` — fails if referenced slugs are not already present in inventory
- Default mode is ensure mode — missing reference objects are created or updated

## Good source examples

These are production v1 cases that can be used as structural reference for
the payload shape (switch_classes, switch_port_zones, server_classes, etc.);
note that they use the v1 top-level layout rather than the v2 `spec:` nesting:

- Large regression case: `netbox_hedgehog/test_cases/ux_case_128gpu_odd_ports.yaml`
- Training reference architecture: `netbox_hedgehog/test_cases/training_opg128_clos_ro.yaml`
- Inference: `netbox_hedgehog/test_cases/inference_opg64_clos_ro.yaml`

For a v2-format structural example, see the `_v2_base()` helper in:
`netbox_hedgehog/tests/test_topology_planning/test_yaml_contract_v2.py`

## Legacy v1 compatibility

v1 format is still accepted by the loader and continues to work unchanged.
It is not the recommended authoring format for new files.

Key differences from v2:

| Concern | v1 | v2 |
|---|---|---|
| Version signal | no `apiVersion` field | `apiVersion: diet/v2` |
| Kind field | absent | `kind: TopologyPlan` |
| Identity block | `meta:` | `metadata:` |
| `meta.version` | integer (e.g. `1`) | must be `2` |
| Payload location | top-level | nested under `spec:` |
| Reference objects | `reference_data:` block with symbolic IDs | pre-seeded inventory; slugs |
| Test-time objects | `reference_data:` (ensure mode creates them) | `test_fixtures:` |

A minimal v1 document looks like:

```yaml
meta:
  case_id: my_legacy_case
  name: My Legacy Case
  version: 1
  managed_by: yaml

reference_data:
  manufacturers: [...]
  device_types: [...]
  device_type_extensions: [...]
  breakout_options: [...]
  module_types: [...]

plan:
  name: My Legacy Case
  status: draft

switch_classes: [...]
switch_port_zones: [...]
server_classes: [...]
server_nics: [...]
server_connections: [...]
```

When migrating a v1 file to v2:

1. Add `apiVersion: diet/v2` and `kind: TopologyPlan`.
2. Rename `meta:` to `metadata:` and set `metadata.version: 2`.
3. Move `switch_classes`, `switch_port_zones`, `server_classes`, `server_nics`,
   `server_connections`, and `plan` under a new `spec:` key.
4. Replace `reference_data:` symbolic ID references with slug-based lookups
   (ensure the target DeviceTypes and BreakoutOptions exist in inventory first).
5. Remove `reference_data:` entirely — the schema will reject it in v2.
