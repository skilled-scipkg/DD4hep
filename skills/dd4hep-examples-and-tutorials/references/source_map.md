# dd4hep source map: Examples and Tutorials

Generated from source roots:
- `examples`
- `DDG4/examples`
- `DDG4/hepmc`
- `DDG4/src`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `ClientTests`
- `SiDSim`
- `HepMC3`
- `example script`
- `reference output`

## Fast source navigation
- `rg -n "class Setup|def configure|def run\(" examples/ClientTests/scripts/DDG4TestSetup.py examples/ClientTests/scripts/MiniTel.py`
- `rg -n "setupTracker|setupCalorimeter|setupPhysics|kernel\.run\(" DDG4/examples/SiDSim.py DDG4/examples/SiDSim_MT.py`
- `rg -n "HepMC3|readGenEvent|ingestParameters" DDG4/hepmc/HepMC3FileReader.cpp DDG4/hepmc/HepMC3EventReader.cpp`
- `rg -n "DECLARE_DETELEMENT|create_detector|addPhysVolID" examples/ClientTests/src/MiniTel.cpp examples/SimpleDetector/src/ZPlanarTracker_geo.cpp`

## Suggested source entry points
- `examples/CMakeLists.txt` | function-level checks: example package gating and install/test wiring
- `examples/ClientTests/CMakeLists.txt` | function-level checks: ClientTests targets, reference comparison hooks
- `examples/ClientTests/scripts/DDG4TestSetup.py` | function-level checks: `Setup.configure`, `setupGun`, `setupPhysics`, `test_run`
- `examples/ClientTests/scripts/MiniTel.py` | function-level checks: `run` minimal geometry+simulation flow
- `examples/ClientTests/scripts/BoxOfStraws_DDSim.py` | function-level checks: `SIM.action.trackerSDTypes`, `SIM.outputConfig.forceDD4HEP`
- `DDG4/examples/SiDSim.py` | function-level checks: `run`, `setupTracker`, `setupCalorimeter`, `setupPhysics`
- `DDG4/examples/SiDSim_MT.py` | function-level checks: `setupWorker`, `setupMaster`, `setupSensitives`, `run`
- `examples/ClientTests/src/MiniTel.cpp` | function-level checks: `create_detector`, `pickMotherVolume`, `addPhysVolID`, `DECLARE_DETELEMENT`
- `examples/SimpleDetector/src/ZPlanarTracker_geo.cpp` | function-level checks: `create_element`, sensitive placement, PhysVolID mapping
- `DDG4/hepmc/HepMC3FileReader.cpp` | function-level checks: run/event metadata ingestion and reader factory setup
- `DDG4/hepmc/HepMC3EventReader.cpp` | function-level checks: HepMC3-to-DDG4 event conversion and particle graph traversal
- `DDG4/src/Geant4DataConversion.cpp` | function-level checks: simulation output conversion steps used by example workflows
