# dd4hep source map: Build and Install

Generated from source roots:
- `CMakeLists.txt`
- `cmake`
- `DDG4`
- `UtilityApps`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `DD4HEP_USE_*`
- `BUILD_TESTING`
- `DD4HEP_BUILD_EXAMPLES`
- `ROOTTPython`
- `CMAKE_INSTALL_LIBDIR`

## Fast source navigation
- `rg -n "option\(DD4HEP_USE_|BUILD_TESTING|DD4HEP_BUILD_EXAMPLES|DD4HEP_BUILD_PACKAGES" CMakeLists.txt`
- `rg -n "dd4hep_set_compiler_flags|dd4hep_enable_tests|dd4hep_add_plugin|DD4HEP_IGNORE_GEANT4_TLS" cmake/DD4hepBuild.cmake`
- `rg -n "if\(DD4HEP_USE_GEANT4\)|ROOT::ROOTTPython|DD4HEP_USE_HEPMC3|DD4HEP_HEPMC3_COMPRESSION_SUPPORT" DDG4/CMakeLists.txt`
- `rg -n "thisdd4hep|run_test" cmake/thisdd4hep.sh cmake/thisdd4hep_only.sh cmake/run_test.sh`

## Suggested source entry points
- `CMakeLists.txt` | function-level checks: dependency `option(...)` gates, package list logic, install-script `configure_file(...)`
- `cmake/DD4hepBuild.cmake` | function-level checks: `dd4hep_set_compiler_flags`, `dd4hep_enable_tests`, `dd4hep_add_dictionary`, `dd4hep_add_plugin`
- `cmake/DD4hep.cmake` | function-level checks: `add_dd4hep_plugin`, `dd4hep_generate_rootmap`, install destination handling
- `cmake/DD4hep_XML_setup.cmake` | function-level checks: XercesC/TinyXML backend toggle behavior
- `cmake/thisdd4hep.sh` | function-level checks: runtime env export order (`PATH`, library path, Python path)
- `cmake/thisdd4hep_only.sh` | function-level checks: reduced environment bootstrap behavior
- `cmake/run_test.sh` | function-level checks: test command wrapping after sourcing runtime script
- `DDG4/CMakeLists.txt` | function-level checks: Geant4 gate, Python dictionary creation, HepMC3/LCIO/EDM4hep plugin conditions
- `UtilityApps/CMakeLists.txt` | function-level checks: optional app targets and `BUILD_TESTING`-guarded tests
