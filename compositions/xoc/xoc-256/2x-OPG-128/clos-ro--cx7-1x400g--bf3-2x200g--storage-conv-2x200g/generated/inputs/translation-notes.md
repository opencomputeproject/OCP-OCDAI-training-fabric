# Translation Notes: training_xoc256_2xopg128_clos_ro

## Pass-1 Status: complete-pass1

## Generation Summary
- Plan ID at generation: 15
- Site slug: xoc256-clos-ro-air-2srv
- Device count: 41

## Wiring Artifacts
All managed-fabric wiring artifacts produced by --split-by-fabric are preserved in
wiring/. Each wiring-<fabric>.yaml file is independently consumable.
See hhfab/ for per-fabric validation logs (hhfab_validate_<fabric>.log).

If a fabric's wiring export or hhfab validation failed, the artifact is still
preserved and the failure is recorded in the corresponding log file.
Known gap: frontend (DS5000 L3MH variants) export may fail with a mclag gap —
"Switch references group 'fe-mclag' but no PlanMCLAGDomain exists". This is
tracked as a pre-existing DIET gap. The artifact is kept even if export partially
failed; see wiring_export.log for the full error.





## SH Distribution Note (if applicable)
Single-homed scale-out variants use same-switch grouped-per-leaf distribution.
Servers are allocated in contiguous blocks per leaf (first N/2 → leaf A, rest → leaf B).

## XOC Composition Note (if applicable)
XOC variants are modeled as a single scaled DIET plan with the combined server
count. Per-OPG-unit composition semantics are not modeled in DIET pass-1.

