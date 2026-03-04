---
name: dd4hep-build-and-install
description: This skill should be used when users ask about build and install in dd4hep; it prioritizes documentation references and then source inspection only for unresolved details.
---

# dd4hep: Build and Install

## High-Signal Playbook
### Route conditions
- Use this skill for CMake configure/build/install, dependency toggles, CI/HPC onboarding, runtime environment scripts, and test enablement.
- Route compact XML/plugin logic to `dd4hep-inputs-and-modeling`.
- Route DDG4 and `ddsim` steering/runtime logic to `dd4hep-simulation-workflows`.

### Triage questions
- Which step fails: configure, compile, install, runtime environment, or tests?
- Which optional stack is required: Geant4, XercesC, LCIO, EDM4hep, HepMC3?
- Are you building top-level DD4hep or only `examples/`?
- Are `DD4HEP_DISABLE_PACKAGES` or `DD4HEP_BUILD_PACKAGES` set?
- Do you need Python bindings (`ddsim`, DDG4 Python dictionaries)?
- Exact CMake error text (`find_package`, C++ standard, TLS, missing targets)?

### Canonical workflow
1. Configure from repository root with explicit install prefix and required options.
2. Enable optional dependencies only when needed: `DD4HEP_USE_GEANT4`, `DD4HEP_USE_XERCESC`, `DD4HEP_USE_LCIO`, `DD4HEP_USE_EDM4HEP`, `DD4HEP_USE_HEPMC3`.
3. Keep `BUILD_TESTING=ON` for validation and `DD4HEP_BUILD_EXAMPLES=ON` when you need runnable examples.
4. Build and install.
5. Source `<install>/bin/thisdd4hep.sh` before running tools.
6. Run `ctest --output-on-failure`.
7. If needed, build standalone examples in `examples/` using `DD4HEP_EXAMPLES`.

### Minimal working example
```bash
cmake -S . -B build \
  -DCMAKE_INSTALL_PREFIX=$PWD/install \
  -DDD4HEP_USE_GEANT4=ON \
  -DGeant4_DIR=/path/to/Geant4/lib/cmake/Geant4 \
  -DDD4HEP_USE_LCIO=ON \
  -DDD4HEP_USE_EDM4HEP=ON \
  -DDD4HEP_USE_HEPMC3=ON \
  -DDD4HEP_USE_XERCESC=ON \
  -DXERCESC_ROOT_DIR=/path/to/xerces \
  -DBUILD_TESTING=ON \
  -DDD4HEP_BUILD_EXAMPLES=ON
cmake --build build -j
cmake --install build
source install/bin/thisdd4hep.sh
ctest --test-dir build --output-on-failure
```

### Pitfalls and fixes
- Symptom: obsolete option failure from old docs (`DD4HEP_WITH_GEANT4` / `DD4HEP_WITH_GEAR`). Fix: use `DD4HEP_USE_GEANT4` and modern `DD4HEP_USE_*` options.
- Symptom: Geant4 C++ mismatch fatal error. Fix: use a Geant4 build compatible with DD4hep C++ standard (`CMAKE_CXX_STANDARD=17` by default).
- Symptom: Geant4 TLS model fatal error. Fix: rebuild Geant4 with `global-dynamic` TLS or set `DD4HEP_IGNORE_GEANT4_TLS=ON` only if you accept the risk.
- Symptom: HepMC3 compression option fatal error. Fix: enable `DD4HEP_HEPMC3_COMPRESSION_SUPPORT` only with HepMC3 >= 3.2.5.
- Symptom: missing DDG4 Python features. Fix: ensure Python and `ROOT::ROOTTPython` are available during configure.
- Symptom: tests unexpectedly absent for examples. Fix: do not build each example subfolder standalone; use top-level `examples/` flow or set `BUILD_TESTING=ON`.

### Convergence/validation checks
- CMake summary includes expected options and package list (`Will be building these packages`).
- `install/bin/thisdd4hep.sh` exists and can be sourced without errors.
- `ctest --output-on-failure` passes for configured targets.
- Core executables are on `PATH` after sourcing (`geoPluginRun`, `geoDisplay`, optionally `ddsim` when DDG4/Python is enabled).
- Optional output plugins match enabled dependencies (LCIO/EDM4hep/HepMC3).

## Primary documentation references
- `doc/usermanuals/DD4hep/chapters/basics.tex`
- `README.md`
- `examples/README.md`
- `examples/CMakeLists.txt`
- `doc/CMakeLists.txt`
- `DDG4/CMakeLists.txt`

## Escalation workflow
- Start with the references above and `references/doc_map.md`.
- If unresolved, inspect `references/source_map.md`.
- Then open concrete CMake/script implementation entry points.

## Source entry points for unresolved issues
- `CMakeLists.txt` (top-level options, dependency resolution, install scripts)
- `cmake/DD4hepBuild.cmake` (test environment setup, option reporting, runtime library path handling)
- `cmake/DD4hep_XML_setup.cmake` (Xerces/TinyXML setup path)
- `cmake/thisdd4hep.sh`
- `cmake/thisdd4hep_only.sh`
- `cmake/run_test.sh`
- `DDG4/CMakeLists.txt` (DDG4, LCIO/EDM4hep/HepMC3 plugins, `ddsim` installation)
- `UtilityApps/CMakeLists.txt` (optional executables tied to ROOT/LCIO/Geant4 targets)
- Prefer targeted source search: `rg -n "<symbol_or_keyword>" DDAlign DDCAD DDCond DDCore DDDetectors DDDigi DDEve DDG4 DDParsers DDRec GaudiPluginService UtilityApps`.
