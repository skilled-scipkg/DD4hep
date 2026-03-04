---
name: dd4hep-inputs-and-modeling
description: This skill should be used when users ask about inputs and modeling in dd4hep; it prioritizes documentation references and then source inspection only for unresolved details.
---

# dd4hep: Inputs and Modeling

## High-Signal Playbook
### Route conditions
- Use this skill for compact XML structure, include patterns, detector constructor plugins, materials/readouts/segmentations, and geometry validity checks.
- Route build/dependency issues to `dd4hep-build-and-install`.
- Route DDG4 runtime steering/output behavior to `dd4hep-simulation-workflows`.

### Triage questions
- Are you extending an existing compact XML file or adding a new detector type?
- What is the exact `<detector ... type="...">` value and matching plugin symbol?
- Which `<readout>` and `<segmentation>` should be bound to this detector?
- Are volumes marked sensitive and assigned consistent PhysVol IDs?
- Is failure at XML load time, plugin construction time, or validation/tool stage?
- Do you need ROOT geometry checks only, or Geant4 material/geometry scans too?

### Canonical workflow
1. Start from a known compact model (`examples/SimpleDetector/compact/Simple_CLIC.xml` or `examples/ClientTests/compact/MiniTel.xml`).
2. Define `<includes>` and `<define>` constants first, then detector entries.
3. Add `<detector>` with stable `name`, `type`, `id`, `readout`, and optional `limits`/`region`.
4. Define `<readout>` and `<segmentation>`; ensure ID descriptor fields match PhysVol IDs set in C++.
5. Implement constructor plugin (`create_detector(...)`) and place volumes through `Detector::pickMotherVolume(...)`.
6. Register plugin with `DECLARE_DETELEMENT(<type>, create_detector)` so XML `type` resolves at load time.
7. Build/install plugin library, then load geometry via `geoPluginRun` or `ddsim`.
8. Run geometry/material validation (`checkOverlaps`, `checkGeometry`, `materialScan`, `DetectorCheck`).

### Minimal working example
```xml
<detector name="MyLHCBdetector1" type="MiniTelPixel" id="1"
          material="Silicon" sensitive="true" readout="MyLHCBdetector1Hits">
  <dimensions z="1*mm" y="10*cm" x="10*cm"/>
  <module_position z="30*mm" y="0*cm" x="0*cm"/>
  <module name="pixel" type="MiniTelPixel" material="Silicon"
          x="6*mm" y="6*mm" z="1*mm"/>
</detector>
<readout name="MyLHCBdetector1Hits">
  <segmentation type="CartesianGridXY" grid_size_x="6*mm" grid_size_y="6*mm"/>
  <id>system:6,side:2,module:8,x:28:-12,y:52:-12</id>
</readout>
```

```c++
static Ref_t create_detector(Detector& description, xml_h e, SensitiveDetector sens) {
  xml_det_t x_det = e;
  DetElement sdet(x_det.nameStr(), x_det.id());
  Volume mother = description.pickMotherVolume(sdet);
  Volume sensor("sensor", Box(1*mm, 1*mm, 1*mm), description.material("Silicon"));
  if (x_det.isSensitive()) sens.setType("tracker");
  sensor.setSensitiveDetector(sens);
  PlacedVolume pv = mother.placeVolume(sensor);
  pv.addPhysVolID("system", x_det.id());
  sdet.setPlacement(pv);
  return sdet;
}
DECLARE_DETELEMENT(MiniTelPixel, create_detector)
```

### Pitfalls and fixes
- Symptom: `Failed to execute subdetector creation plugin`. Fix: XML `type` must match a compiled plugin registered via `DECLARE_DETELEMENT`.
- Symptom: `No Readout structure present for detector`. Fix: define matching `<readout name="...">` and keep detector `readout="..."` consistent.
- Symptom: wrong/empty cell IDs or segmentation failures. Fix: align `<id>` descriptor fields with `addPhysVolID(...)` keys in constructor code.
- Symptom: `Unknown sensitive detector type` during `ddsim`. Fix: set valid sensitive types in geometry or map them through `SIM.action.trackerSDTypes` / `SIM.action.calorimeterSDTypes`.
- Symptom: overlap/voxelization warnings (`TGeoVoxelFinder`, `CheckOverlaps`). Fix: simplify problematic placements/shapes and rerun ROOT checks.

### Convergence/validation checks
- `geoPluginRun -input file:<compact>.xml -volmgr -destroy -plugin DD4hep_DetectorCheck -name all -geometry -structure -sensitive`
- `python <install>/bin/checkOverlaps.py -c file:<compact>.xml`
- `python <install>/bin/checkGeometry.py -c file:<compact>.xml`
- `materialScan file:<compact>.xml 0 0 0 0 10000 0`
- `geoConverter -compact2description -input file:<compact>.xml -output detector.description`

## Primary documentation references
- `doc/usermanuals/DD4hep/chapters/overview.tex`
- `doc/usermanuals/DD4hep/chapters/basics.tex`
- `examples/SimpleDetector/compact/Simple_CLIC.xml`
- `examples/ClientTests/compact/MiniTel.xml`
- `examples/DDCodex/README.txt`

## Escalation workflow
- Start from primary docs and concrete compact examples.
- If needed, inspect `references/doc_map.md`.
- For behavior details, inspect source entry points below.

## Source entry points for unresolved issues
- `DDCore/src/plugins/Compact2Objects.cpp` (XML detector conversion and plugin dispatch)
- `DDCore/include/DD4hep/Factories.h` (`DECLARE_DETELEMENT` and XML processor macros)
- `examples/ClientTests/src/MiniTel.cpp` (full detector constructor flow)
- `examples/SimpleDetector/src/ZPlanarTracker_geo.cpp` (layer/sensitive/readout modeling)
- `DDCore/src/DD4hepRootPersistency.cpp` (readout/segmentation integrity checks)
- `DDCore/src/segmentations/WaferGridXY.cpp`
- `DDCore/src/segmentations/TiledLayerGridXY.cpp`
- `DDG4/python/DDSim/DD4hepSimulation.py` (sensitive detector typing and setup hooks)
- `DDG4/python/DDSim/Helper/Geometry.py` (regex-based sensitive detector construction)
- Prefer targeted source search: `rg -n "<symbol_or_keyword>" DDAlign DDCAD DDCond DDCore DDDetectors DDDigi DDEve DDG4 DDParsers DDRec GaudiPluginService UtilityApps`.
