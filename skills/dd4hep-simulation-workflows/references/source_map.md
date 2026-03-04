# dd4hep source map: Simulation Workflows

Generated from source roots:
- `DDG4/python`
- `DDG4/src`
- `DDG4/plugins`
- `UtilityApps`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `parseOptions`
- `_consistencyChecks`
- `userInputPlugin`
- `runPlugin`
- `setupTracker`

## Fast source navigation
- `rg -n "class DD4hepSimulation|def parseOptions|def run\(|def _consistencyChecks" DDG4/python/DDSim/DD4hepSimulation.py`
- `rg -n "trackerSDTypes|mapActions|setupFilters|applyFilters" DDG4/python/DDSim/Helper/Action.py DDG4/python/DDSim/Helper/Filter.py`
- `rg -n "forceLCIO|forceEDM4HEP|forceDD4HEP|initialize" DDG4/python/DDSim/Helper/OutputConfig.py`
- `rg -n "runPlugin|initialize\(|run\(" DDG4/src/Geant4Kernel.cpp DDG4/src/Geant4UIManager.cpp DDG4/src/Geant4RunAction.cpp DDG4/src/Geant4SteppingAction.cpp`
- `rg -n "run_plugin|main_wrapper|invoke_plugin_runner" UtilityApps/src/run_plugin.h UtilityApps/src/plugin_runner.cpp`

## Suggested source entry points
- `DDG4/python/DDSim/DD4hepSimulation.py` | function-level checks: `parseOptions`, `run`, `_consistencyChecks`, `_buildInputStage`, `__setupSensitiveDetectors`
- `DDG4/python/DDSim/Helper/Action.py` | function-level checks: detector action mapping via `mapActions`, `trackerSDTypes`, `calorimeterSDTypes`
- `DDG4/python/DDSim/Helper/Filter.py` | function-level checks: `setupFilters`, `applyFilters`, per-detector filter map behavior
- `DDG4/python/DDSim/Helper/Geometry.py` | function-level checks: `regexSensitiveDetector`, `constructGeometry`
- `DDG4/python/DDSim/Helper/InputConfig.py` | function-level checks: `userInputPlugin` validation and list semantics
- `DDG4/python/DDSim/Helper/OutputConfig.py` | function-level checks: force-flag exclusivity and output backend selection
- `DDG4/python/bin/g4GeometryScan.py` | function-level checks: Geant4 geometry scan command path
- `DDG4/python/bin/g4MaterialScan.py` | function-level checks: material scan command path
- `DDG4/src/Geant4Kernel.cpp` | function-level checks: `runPlugin`, `initialize`, `run`
- `DDG4/src/Geant4UIManager.cpp` | function-level checks: UI command registration and run-plugin command dispatch
- `DDG4/src/Geant4RunAction.cpp` | function-level checks: run lifecycle callback ordering
- `DDG4/src/Geant4SteppingAction.cpp` | function-level checks: stepping callback behavior for custom actions
- `DDG4/plugins/Geant4RunManagers.cpp` | function-level checks: run-manager selection and multithread routing
- `DDG4/plugins/Geant4Steppers.cpp` | function-level checks: tracking stepper configuration behavior
- `UtilityApps/src/run_plugin.h` | function-level checks: `geoPluginRun` argument parsing and plugin execution path
- `UtilityApps/src/plugin_runner.cpp` | function-level checks: plugin-runner executable entrypoint
