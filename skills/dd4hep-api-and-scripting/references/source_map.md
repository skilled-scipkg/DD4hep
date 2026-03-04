# dd4hep source map: API and Scripting

Generated from source roots:
- `DDG4/python`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `DD4hepSimulation`
- `userInputPlugin`
- `mapActions`
- `regexSensitiveDetector`
- `outputConfig`

## Fast source navigation
- `rg -n "class DD4hepSimulation|def parseOptions|def run\(|def _consistencyChecks" DDG4/python/DDSim/DD4hepSimulation.py`
- `rg -n "trackerSDTypes|mapActions|calorimeterSDTypes" DDG4/python/DDSim/Helper/Action.py`
- `rg -n "setupFilters|applyFilters|mapDetFilter" DDG4/python/DDSim/Helper/Filter.py`
- `rg -n "userInputPlugin|forceLCIO|forceEDM4HEP|forceDD4HEP|regexSensitiveDetector" DDG4/python/DDSim/Helper`
- `rg -n "setupDetector|setupTracker|setupCalorimeter|setupPhysics|setupROOTOutput" DDG4/python/DDG4.py`

## Suggested source entry points
- `DDG4/python/DDSim/DD4hepSimulation.py` | function-level checks: `parseOptions`, `run`, `_consistencyChecks`, `_buildInputStage`, `__setupSensitiveDetectors`
- `DDG4/python/DDSim/Helper/Action.py` | function-level checks: `trackerSDTypes`, `calorimeterSDTypes`, `mapActions`, `makeListOfDictFromJSON`
- `DDG4/python/DDSim/Helper/Filter.py` | function-level checks: `_createDefaultFilters`, `setupFilters`, `applyFilters`
- `DDG4/python/DDSim/Helper/Geometry.py` | function-level checks: `regexSensitiveDetector`, `constructGeometry`
- `DDG4/python/DDSim/Helper/InputConfig.py` | function-level checks: `userInputPlugin` setter callable/list validation
- `DDG4/python/DDSim/Helper/OutputConfig.py` | function-level checks: `_checkConsistency`, `initialize`, `_configureLCIO`, `_configureEDM4HEP`, `_configureDD4HEP`
- `DDG4/python/DDSim/Helper/Physics.py` | function-level checks: physics list setup and constructor toggles
- `DDG4/python/DDG4.py` | function-level checks: `Geant4.setupDetector`, `setupTracker`, `setupCalorimeter`, `setupPhysics`, `setupROOTOutput`, `setupEDM4hepOutput`
- `DDG4/python/DDSim/bin/ddsim.in.py` | function-level checks: DDSim CLI entrypoint and argument forwarding
