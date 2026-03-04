# dd4hep source map: Inputs and Modeling

Generated from source roots:
- `DDCore`
- `examples`
- `DDG4/python`
- `UtilityApps`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `DECLARE_DETELEMENT`
- `readout`
- `segmentation`
- `PhysVolID`
- `DetectorCheck`

## Fast source navigation
- `rg -n "DECLARE_DETELEMENT|DECLARE_XML_PROCESSOR|pickMotherVolume|addPhysVolID" DDCore/include/DD4hep/Factories.h examples/ClientTests/src examples/SimpleDetector/src`
- `rg -n "No Readout structure present|fromXML|Converter<Readout>|segmentation" DDCore/src/plugins/Compact2Objects.cpp`
- `rg -n "checkDetElementTree|checkManagerVolumeTree|DD4hep_DetectorCheck" DDCore/src/plugins/DetectorCheck.cpp`
- `rg -n "checkReadouts|checkSegmentations|checkAll" DDCore/src/DD4hepRootPersistency.cpp`

## Suggested source entry points
- `DDCore/src/plugins/Compact2Objects.cpp` | function-level checks: compact parsing pipeline, readout/segmentation conversion, plugin dispatch errors
- `DDCore/include/DD4hep/Factories.h` | function-level checks: `DECLARE_DETELEMENT`, XML processor/factory macro expansion
- `examples/ClientTests/src/MiniTel.cpp` | function-level checks: `create_detector`, sensitive assignment, `addPhysVolID` consistency
- `examples/SimpleDetector/src/ZPlanarTracker_geo.cpp` | function-level checks: layer/module IDs and sensitive volume modeling
- `DDCore/src/plugins/DetectorCheck.cpp` | function-level checks: `DetectorCheck::run`, geometry/structure/volmgr validation paths
- `DDCore/src/DD4hepRootPersistency.cpp` | function-level checks: `checkReadouts`, `checkSegmentations`, readout/ID integrity
- `DDCore/src/segmentations/WaferGridXY.cpp` | function-level checks: wafer segmentation parameter interpretation
- `DDCore/src/segmentations/TiledLayerGridXY.cpp` | function-level checks: tiled-layer segmentation boundary behavior
- `DDCore/python/bin/checkGeometry.py` | function-level checks: geometry graph checks and error reporting path
- `DDCore/python/bin/checkOverlaps.py` | function-level checks: overlap-scan driver behavior
- `UtilityApps/src/materialScan.cpp` | function-level checks: directional material scan executable behavior
- `DDG4/python/DDSim/Helper/Geometry.py` | function-level checks: `regexSensitiveDetector` and geometry construction bridge
