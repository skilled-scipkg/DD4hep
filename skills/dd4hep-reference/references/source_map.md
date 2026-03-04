# dd4hep source map: Reference

Generated from source roots:
- `DDCore`
- `DDG4`
- `DDParsers`
- `examples`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `Detector::fromXML`
- `Detector::apply`
- `DECLARE_DETELEMENT`
- `DD4hepSimulation.run`
- `Geant4Kernel::runPlugin`

## Fast source navigation
- `rg -n "class Detector|fromXML\(|apply\(|pickMotherVolume" DDCore/include/DD4hep/Detector.h DDCore/src/DetectorImp.cpp`
- `rg -n "class DetElement|volumeID\(|placement\(" DDCore/include/DD4hep/DetElement.h DDCore/src/DetElement.cpp`
- `rg -n "DECLARE_DETELEMENT|DECLARE_APPLY|DECLARE_XML_PROCESSOR" DDCore/include/DD4hep/Factories.h`
- `rg -n "class DD4hepSimulation|def run\(|def parseOptions" DDG4/python/DDSim/DD4hepSimulation.py`
- `rg -n "setupDetector|setupTracker|setupCalorimeter|setupPhysics" DDG4/python/DDG4.py`
- `rg -n "runPlugin\(|initialize\(|run\(" DDG4/src/Geant4Kernel.cpp DDG4/src/Geant4UIManager.cpp`

## Suggested source entry points
- `DDCore/include/DD4hep/Detector.h` | function-level checks: `fromCompact`, `fromXML`, `apply`, `pickMotherVolume`
- `DDCore/src/DetectorImp.cpp` | function-level checks: `DetectorImp::fromXML`, `DetectorImp::apply`, `DetectorImp::pickMotherVolume`
- `DDCore/include/DD4hep/DetElement.h` | function-level checks: DetElement hierarchy and identity API contracts
- `DDCore/src/DetElement.cpp` | function-level checks: `DetElement::volumeID` and placement behavior
- `DDCore/include/DD4hep/Factories.h` | function-level checks: `DECLARE_DETELEMENT`, `DECLARE_APPLY`, factory macro semantics
- `DDCore/src/plugins/Compact2Objects.cpp` | function-level checks: compact loader conversion and plugin dispatch path
- `DDG4/python/DDG4.py` | function-level checks: `Geant4.setupDetector`, `setupTracker`, `setupCalorimeter`, output setup methods
- `DDG4/python/DDSim/DD4hepSimulation.py` | function-level checks: CLI parsing, consistency checks, simulation run orchestration
- `DDG4/src/Geant4Kernel.cpp` | function-level checks: `runPlugin`, run manager initialization, run lifecycle
- `DDG4/src/Geant4UIManager.cpp` | function-level checks: UI command registration and plugin dispatch from UI
- `DDG4/pyddg4.cpp` | function-level checks: Python-C++ bridge boundaries for DDG4 bindings
- `DDParsers/include/Evaluator/DD4hepUnits.h` | function-level checks: unit expression lookup frequently referenced in compact parsing
