# dd4hep source map: Releasenotes

Generated from source roots:
- `CMakeLists.txt`
- `cmake`
- `DDCore`
- `DDG4`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `DecayByGeant`
- `runPlugin`
- `regexSensitiveDetector`
- `CMAKE_INSTALL_LIBDIR`
- `G4StepLimiterPhysics`

## Fast source navigation
- `rg -n "CMAKE_INSTALL_LIBDIR|CMP0167|DD4HEP_USE_EDM4HEP|DD4HEP_IGNORE_GEANT4_TLS|CMAKE_CXX_STANDARD" CMakeLists.txt cmake/DD4hepBuild.cmake cmake/DD4hep.cmake`
- `rg -n "DecayByGeant|--gdb|_consistencyChecks|addPhysicsConstructor\('G4StepLimiterPhysics'\)" DDG4/python/DDSim/DD4hepSimulation.py DDG4/src/Geant4PrimaryHandler.cpp`
- `rg -n "runPlugin\(|run_plugin" DDG4/src/Geant4Kernel.cpp DDG4/src/Geant4UIManager.cpp UtilityApps/src/run_plugin.h`
- `rg -n "regexSensitiveDetector" DDG4/python/DDSim/Helper/Geometry.py examples/ClientTests/scripts/BoxOfStraws_DDSim.py`
- `rg -n "HepMC3|run metadata|readGenEvent" DDG4/hepmc/HepMC3FileReader.cpp DDG4/hepmc/HepMC3EventReader.cpp`
- `rg -n "CloseGeometry\(|option=\"nv\"|volumeID" DDCore/src/DetectorImp.cpp DDCore/src/XML/VolumeBuilder.cpp DDG4/src/Geant4VolumeManager.cpp`

## Suggested source entry points
- `CMakeLists.txt` | function-level checks: C++ standard, policy handling, dependency gates, install layout (`CMAKE_INSTALL_LIBDIR`)
- `cmake/DD4hepBuild.cmake` | function-level checks: TLS checks, plugin env preservation, test/plugin macro behavior
- `cmake/DD4hep.cmake` | function-level checks: `add_dd4hep_plugin` install path handling
- `DDG4/python/DDSim/DD4hepSimulation.py` | function-level checks: `parseOptions` (`--gdb`), `DecayByGeant` wiring, consistency guards
- `DDG4/python/DDSim/Helper/Geometry.py` | function-level checks: `regexSensitiveDetector` behavior
- `DDG4/src/Geant4PrimaryHandler.cpp` | function-level checks: `declareProperty("DecayByGeant", ...)`
- `DDG4/src/Geant4Kernel.cpp` | function-level checks: `runPlugin`, initialization/run lifecycle
- `DDG4/src/Geant4UIManager.cpp` | function-level checks: UI callback and run-plugin command forwarding
- `DDG4/python/DDG4.py` | function-level checks: dynamic Geant4 class loading and output setup helpers
- `DDG4/python/DDG4PhysicsDict.C` | function-level checks: dictionary coverage for customizable physics constructors
- `DDG4/plugins/Geant4TrackerWeightedSD.cpp` | function-level checks: tracker weighted hit behavior updated in recent release notes
- `DDG4/hepmc/HepMC3FileReader.cpp` | function-level checks: HepMC3 reader metadata ingestion and reader initialization
- `DDG4/hepmc/HepMC3EventReader.cpp` | function-level checks: event conversion path and particle linkage
- `DDCore/src/XML/VolumeBuilder.cpp` | function-level checks: XML volume/assembly construction behavior tied to release-note fixes
- `DDCore/src/DetectorImp.cpp` | function-level checks: geometry close options (`CloseGeometry`) and compact loading behavior
