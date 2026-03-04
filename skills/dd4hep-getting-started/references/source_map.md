# dd4hep source map: Getting Started

Generated from source roots:
- `cmake`
- `DDCore`
- `DDG4`
- `UtilityApps`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `quickstart`
- `first-run`
- `environment`
- `ddsim`
- `detectorcheck`

## Fast source navigation
- `rg -n "DD4HEP_USE_|BUILD_TESTING|DD4HEP_BUILD_EXAMPLES" CMakeLists.txt cmake`
- `rg -n "parseOptions|run\(|_consistencyChecks|__attachGDB" DDG4/python/DDSim/DD4hepSimulation.py`
- `rg -n "DetectorCheck::run|DECLARE_APPLY\(DD4hep_DetectorCheck" DDCore/src/plugins/DetectorCheck.cpp`
- `rg -n "run_plugin|invoke_plugin_runner|main_wrapper" UtilityApps/src/run_plugin.h UtilityApps/src/plugin_runner.cpp`

## Suggested source entry points
- `CMakeLists.txt` | function-level checks: `option(DD4HEP_USE_GEANT4 ...)`, `option(BUILD_TESTING ...)`, `configure_file(cmake/thisdd4hep.sh ...)`
- `cmake/DD4hepBuild.cmake` | function-level checks: `dd4hep_set_compiler_flags`, `dd4hep_enable_tests`, `dd4hep_add_plugin`
- `cmake/thisdd4hep.sh` | function-level checks: environment exports for `PATH`, `LD_LIBRARY_PATH`, `PYTHONPATH`
- `DDG4/python/DDSim/DD4hepSimulation.py` | function-level checks: `parseOptions`, `run`, `_consistencyChecks`, `__attachGDB`
- `DDG4/python/DDSim/Helper/OutputConfig.py` | function-level checks: `_checkConsistency`, `initialize`, `_configureDD4HEP`, `_configureLCIO`, `_configureEDM4HEP`
- `DDG4/python/DDSim/Helper/InputConfig.py` | function-level checks: `userInputPlugin` setter callable/list validation
- `DDCore/src/plugins/DetectorCheck.cpp` | function-level checks: `DetectorCheck::run`, `checkDetElementTree`, `checkManagerVolumeTree`
- `UtilityApps/src/run_plugin.h` | function-level checks: `run_plugin`, `execute::main_wrapper`, plugin argument handling
- `UtilityApps/src/plugin_runner.cpp` | function-level checks: `main` forwarding to `invoke_plugin_runner`
