---
name: dd4hep-index
description: This skill should be used when users ask how to use DD4hep and the correct topic skill must be selected before source-level debugging.
---

# dd4hep Skills Index

## Route the request
- Build, dependencies, CI, install prefix, runtime environment scripts -> `dd4hep-build-and-install`
- Compact XML structure, `<includes>`, detector `type` plugins, materials/readouts/segmentation -> `dd4hep-inputs-and-modeling`
- DDG4 and `ddsim` steering, user actions/filters, run control, outputs -> `dd4hep-simulation-workflows`
- Need runnable templates from repository examples -> `dd4hep-examples-and-tutorials`
- Python/API usage details in DDSim helpers -> `dd4hep-api-and-scripting`
- Release behavior changes and regressions between versions -> `dd4hep-releasenotes`
- Doxygen/class-level reference lookup -> `dd4hep-reference`
- First successful run from fresh checkout -> `dd4hep-getting-started`

## Fast triage questions
- Is the blocker at configure/build/install time, or at geometry/simulation runtime?
- Do you have a failing compact XML file and exact error text?
- Are you running `ddsim`, `geoPluginRun`, or a custom DDG4 Python script?
- Is the issue about detector constructor plugin loading (`type` and `DECLARE_DETELEMENT`)?
- Do you need ROOT-only geometry checks, Geant4 validation, or both?
- Which optional stack is enabled: Geant4, LCIO, EDM4hep, HepMC3?

## Troubleshooting routing cues
- CMake/dependency errors (`find_package`, TLS, C++ standard, missing plugins) -> `dd4hep-build-and-install`
- `Failed to execute subdetector creation plugin` or `No Readout structure present for detector` -> `dd4hep-inputs-and-modeling`
- `Batch mode requested, but did not set inputFile(s), gun, or userInputPlugin` -> `dd4hep-simulation-workflows`
- Geometry overlap/voxelization/volume-id issues -> `dd4hep-inputs-and-modeling` first, then `dd4hep-examples-and-tutorials` for reproducible test setups

## Executable validation paths
- Build/tests entry points: `examples/README.md`, `examples/ClientTests/CMakeLists.txt`, `DDTest/CMakeLists.txt`
- Geometry validation utilities: `DDCore/python/bin/checkGeometry.py`, `DDCore/python/bin/checkOverlaps.py`, `DDG4/python/bin/g4GeometryScan.py`, `DDG4/python/bin/g4MaterialScan.py`
- Simulation steering templates: `DDG4/examples/SiDSim.py`, `examples/ClientTests/scripts/DDG4TestSetup.py`, `examples/ClientTests/scripts/BoxOfStraws_DDSim.py`

## Docs-first escalation policy
- Start inside the routed topic skill's `Primary documentation references`.
- If missing detail, use that skill's `references/doc_map.md`.
- If ambiguity remains, open that skill's `references/source_map.md`.
- Escalate to source only for unresolved behavior details, starting from the source entry points listed in the same skill.
- Use targeted search, not broad scans: `rg -n "<symbol_or_keyword>" DDAlign DDCAD DDCond DDCore DDDetectors DDDigi DDEve DDG4 DDParsers DDRec GaudiPluginService UtilityApps`.

## Documentation roots
- `README.md`
- `doc`
- `doc/usermanuals/DD4hep`
- `doc/usermanuals/DDG4`
- `examples`
- `DDG4/examples`

## Source roots for deep inspection
- `DDAlign`
- `DDCAD`
- `DDCond`
- `DDCore`
- `DDDetectors`
- `DDDigi`
- `DDEve`
- `DDG4`
- `DDParsers`
- `DDRec`
- `GaudiPluginService`
- `UtilityApps`
