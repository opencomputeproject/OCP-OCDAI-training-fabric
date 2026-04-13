Title: Ticket — Variant Naming Taxonomy (OPG/XOC)
Owner: strategy@hedgehog.cloud
Status: Open

Objective
- Propose a clear, scalable naming convention for OPG/XOC variants that captures topology pattern, hardware generation, NIC speeds, and plane count, while remaining concise and reviewer‑friendly.

Inputs/Constraints
- OPG‑M uses terms like “single‑homed” and “rail‑optimized”; XOC‑N focuses on composition. There is no canonical compact code today.
- We need names that work in folder paths, READMEs, and RA prose.

Deliverable
- A short RFC (1–2 pages) with recommended patterns, examples, and mapping rules.

Proposal v1.1 (adds storage flags)
- Goals: concise, human‑parsable, future‑proof; compatible with folder names; aligns with OCP terms.
- Folder token format (ordered):
  <topo>--<so>--<fe>[--<planes>][--<storage>]
  - topo: clos-sh (Clos single-homed), clos-ro (Clos rail-optimized), dual-plane (for scale-out), mesh-conv (mesh converged)
  - so (scale-out NICs): cx7-1x400g, cx8-2x400g, cx9-1x800g (pattern: <model>-<ports>x<speed>)
  - fe (frontend NICs): bf3-2x200g (or fe-2x200g for vendor-neutral)
  - planes: 1p or 2p (defaults: clos-sh/ro = 1p; dual-plane = 2p)
  - storage: storage-conv-<links>x<speed> or storage-ded-<links>x<speed>
    - Examples: storage-conv-2x200g (frontend converged SO‑C/Storage 2×200G)
               storage-ded-2x100g (dedicated storage fabric 2×100G)

Note on cooling
- The compositions in this repository are designed to work with air or liquid cooling at any density. Cooling medium and rack density are physical deployment choices that do not affect the network topology and are not encoded in variant names.

Examples
- OPG-128/clos-sh--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/
- OPG-128/clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/
- OPG-256/dual-plane--cx8-2x400g--bf3-2x200g--2p--storage-conv-2x200g/
- Future dedicated storage example:
  OPG-128/clos-ro--cx7-1x400g--bf3-2x200g--storage-ded-2x100g/

Display name pattern for READMEs
- “Clos Single‑Homed (CX7 1×400G scale‑out; BF3 2×200G frontend; Storage: Converged 2×200G)”

XOC variant names
- XOC folders mirror the underlying OPG variant tokens for traceability, prefixed by the composition count:
  - XOC-256/2x-OPG-128/clos-sh--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/
  - XOC-512/4x-OPG-128/clos-ro--cx7-1x400g--bf3-2x200g--storage-conv-2x200g/

Tasks
- Validate with OCP reviewers; adjust tokens if they prefer alternative abbreviations.
- Ensure tokens sort well; document in RA_REPOS/CONTRIBUTING.md after agreement.

Tasks
- Survey OCP precedent for naming (if any) and existing community practice.
- Test readability in folder structures and in RA prose.
- Produce final recommendation with examples for OPG‑64/128/256 and XOC‑256/512.

Acceptance Criteria
- Naming taxonomy is accepted and applied to new folders/variants; guidance added to RA_REPOS/CONTRIBUTING.md.

---

## README Content Contract by Layer

This section defines what each README layer in the OPG/XOC composition hierarchy is
responsible for. It supplements the naming-token guidance above. Authors adding new OPG
or XOC variants should follow both the naming convention above and the content contract
for their layer.

### Layer definitions and content contracts

**Layer 1 — `compositions/README.md` (top-level index)**

Required: OPG/XOC definitions; folder organization; navigation guidance.

Key framing: OPGs are *incomplete building blocks* — they include servers and leaf
switches but do not include a spine or external connectivity. XOCs are *composed
solutions* that add spine/aggregation and external connectivity over one or more OPGs.

---

**Layer 2 — `OPG-*/README.md` (size overview)**

Required: Variant list with building-block summaries; common attributes including
uplink budget; pointer to variant READMEs.

Prohibited: Deployment instructions; "why choose it" framed as a deployment decision;
scheduling guidance for deployed clusters.

---

**Layer 3 — `OPG-*/<variant>/README.md` (building-block spec)**

Required:
- What this OPG provides: topology pattern, fabrics (backend/frontend/OoB), hardware
- Port budget: server downlink zones and uplink zone with port count, speed
- Uplink reservation: explicitly state "N×Y uplinks are reserved per leaf for XOC
  spine connectivity" — this is the interface exposed to the composer
- What a composer must supply: list the XOC spine switches, uplink cabling, and any
  external connectivity that this OPG does not include
- Assumptions and constraints: port zoning rules and topology-specific constraints
- Assets list

Optional: Reference to example XOC compositions using this variant, labeled as
illustrative only (not prescriptive).

Prohibited:
- "How to use" deployment steps (import netbox_inventory.json, apply wiring CRDs,
  use Hedgehog in a lab)
- "Ideal for pilots/edge/first deployments" framing
- Scheduling guidance for deployed clusters (tenant placement, rail-domain locality)
- "composition/cluster" as the framing noun for the OPG itself
- Prescribing how the OPG must be composed

---

**Layer 4 — `XOC-*/README.md` (tier overview)**

Required: What this XOC tier composes (total xPU scale); available bundles; operator
benefits; brief mention of what the spine/aggregation layer adds.

---

**Layer 5a — `XOC-*/<bundle>/README.md` (bundle)**

Required: What the bundle composes (N× OPG-X under spine); what the XOC spine
adds at this bundle scale; variant list with one-line descriptions; links to variant READMEs.

Prohibited: Content-free bare variant lists with no explanation.

---

**Layer 5b — `XOC-*/<bundle>/<variant>/README.md` (composed-solution guide)**

Required:
- Composed topology: which OPGs, how they connect to the spine, total cluster scale
- Spine/aggregation layer description: switch model, role, how OPG uplinks connect
- Composition-level attributes (full cluster scale, cross-OPG traffic characteristics)
- Why this composition exists: what it achieves that an OPG alone cannot; tradeoffs
  vs alternatives at the same tier
- Brief OPG summary (1-2 sentences) with pointer to OPG docs for building-block detail
- Deployment guidance ("How to use") — this belongs here, not in OPG READMEs
- Assets list

Prohibited: Re-describing OPG building-block detail at length (defer to OPG docs).

---

### Cross-reference rules

- OPG READMEs may reference example XOC compositions — labeled as illustrative only,
  never as the defining purpose of the OPG.
- XOC READMEs must briefly summarize constituent OPGs and point to OPG docs for
  building-block detail.
- "How to use" / deployment steps and scheduling guidance belong exclusively in
  Layer 5b (XOC variant READMEs).

### Sub-bundle navigation stubs

If a bundle directory contains a sub-bundle directory (e.g.,
`XOC-512/4x-OPG-128/2x-OPG-128/`), its README is a *navigation stub*: 1-2 sentences
describing what the sub-grouping represents, plus a variant list. Full Layer 5a content
is not required until the folder structure is confirmed as intentional.

