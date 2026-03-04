---
name: dd4hep-getting-started
description: Use this skill when users need a first successful DD4hep run from checkout to geometry/simulation smoke tests.
---

# dd4hep: Getting Started

## High-Signal Playbook
### Route conditions
- Use this skill for first-time setup, first build, runtime environment setup, and first geometry/simulation smoke tests.
- Route dependency/version/CMake failures to `dd4hep-build-and-install`.
- Route compact XML modeling details to `dd4hep-inputs-and-modeling`.
- Route DDSim/DDG4 steering internals to `dd4hep-simulation-workflows`.

### Triage questions
- Do you already have a configured build directory?
- Is Geant4 required for this run (for `ddsim`) or only geometry checks?
- Which compact file do you want to start with (`examples/ClientTests/compact/MiniTel.xml` is the default)?
- Is the runtime setup script sourced in the current shell?

### Canonical workflow
1. Configure and build DD4hep from repo root.
2. Install and source `<install>/bin/thisdd4hep.sh`.
3. Run a geometry plugin smoke test.
4. Run a minimal `ddsim` smoke test (1-5 events).
5. Confirm expected outputs and no fatal errors.

### Minimal working example
```bash
cmake -S . -B build \
  -DCMAKE_INSTALL_PREFIX=$PWD/install \
  -DDD4HEP_USE_GEANT4=ON \
  -DBUILD_TESTING=ON \
  -DDD4HEP_BUILD_EXAMPLES=ON
cmake --build build -j
cmake --install build
source install/bin/thisdd4hep.sh
geoPluginRun -input file:examples/ClientTests/compact/MiniTel.xml \
  -volmgr -destroy -plugin DD4hep_DetectorCheck -name all -geometry -structure -sensitive
ddsim --steeringFile examples/ClientTests/scripts/BoxOfStraws_DDSim.py \
      --compactFile examples/ClientTests/compact/BoxOfStraws_sensitive.xml \
      --enableGun --numberOfEvents 2 --outputFile first_sim.root
```

### Convergence/validation checks
- `which geoPluginRun` and `which ddsim` resolve after sourcing setup.
- Geometry check finishes without fatal plugin/load errors.
- `first_sim.root` is created and non-empty.
- If `ddsim` is unavailable, verify DDG4/Python support in configure summary.

## Primary documentation references
- `README.md`
- `examples/README.md`
- `examples/ClientTests/README.txt`
- `doc/usermanuals/DD4hep/chapters/overview.tex`
- `doc/usermanuals/DD4hep/chapters/basics.tex`
- `doc/usermanuals/DDG4/sections/Setup.tex`

## Escalation workflow
- Start with docs and commands above.
- If unresolved, inspect `references/doc_map.md`.
- For behavior-level issues, inspect `references/source_map.md` and follow the listed function checks.

## Source entry points for unresolved issues
- `CMakeLists.txt`
- `cmake/DD4hepBuild.cmake`
- `cmake/thisdd4hep.sh`
- `DDG4/python/DDSim/DD4hepSimulation.py`
- `DDG4/python/DDSim/Helper/OutputConfig.py`
- `DDCore/src/plugins/DetectorCheck.cpp`
- `UtilityApps/src/run_plugin.h`
- `UtilityApps/src/plugin_runner.cpp`
- Prefer targeted source search: `rg -n "<symbol_or_keyword>" DDCore DDG4 UtilityApps cmake`.
