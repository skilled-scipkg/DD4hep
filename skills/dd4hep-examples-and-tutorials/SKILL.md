---
name: dd4hep-examples-and-tutorials
description: This skill should be used when users ask about examples and tutorials in dd4hep; it prioritizes documentation references and then source inspection only for unresolved details.
---

# dd4hep: Examples and Tutorials

## High-Signal Playbook
### Route conditions
- Use this skill when users ask for runnable templates, minimal reproductions, or sample workflows across geometry, conversion, and simulation.
- Route build/dependency setup to `dd4hep-build-and-install`.
- Route compact XML constructor internals to `dd4hep-inputs-and-modeling`.
- Route DDG4/DDsim steering internals to `dd4hep-simulation-workflows`.

### Triage questions
- Do you need geometry-only checks, conversion output, or full simulation?
- Which example package is closest: `ClientTests`, `DDG4`, `SimpleDetector`, `DDCodex`, `DDCAD`?
- Are examples built and installed, or are you running directly from source?
- Is Geant4 support enabled (required for simulation scripts)?
- Do you need a deterministic CI-style command sequence?

### Canonical workflow
1. Build examples from `examples/` using `DD4HEP_EXAMPLES` subset when needed.
2. Source the generated runtime setup script.
3. Start with geometry load/display and converter smoke tests.
4. Run a small simulation script (`ClientTests` or `DDG4`) in batch mode.
5. Validate using reference tests or `ctest --output-on-failure`.

### Minimal working example
```bash
source <install>/bin/thisdd4hep.sh
cmake -S examples -B examples/build -DDD4HEP_EXAMPLES="ClientTests DDG4"
cmake --build examples/build -j
cmake --install examples/build
geoDisplay -input file:examples/ClientTests/compact/MiniTel.xml -load -destroy
ddsim --steeringFile examples/ClientTests/scripts/BoxOfStraws_DDSim.py \
      --compactFile examples/ClientTests/compact/BoxOfStraws_sensitive.xml \
      --enableGun --numberOfEvents 5 --outputFile sim.root
```

### Pitfalls and fixes
- Symptom: no tests for example subfolder build. Fix: use top-level `examples/` workflow; per-subdir builds skip normal test setup.
- Symptom: scripts cannot find geometry files. Fix: use explicit source-tree paths or ensure `DD4hepExamplesINSTALL` is defined by your setup script.
- Symptom: DDG4 scripts fail immediately. Fix: ensure Geant4-enabled DD4hep build and proper runtime environment.
- Symptom: reference mismatches in shape tests. Fix: run official ClientTests targets and compare against `examples/ClientTests/ref/*`.
- Symptom: missing HepMC3 reader behavior. Fix: ensure build with `DD4HEP_USE_HEPMC3=ON`.

### Convergence/validation checks
- `ctest --test-dir examples/build --output-on-failure`
- `geoConverter -compact2description -input file:examples/ClientTests/compact/MiniTel.xml`
- `geoPluginRun -input file:examples/ClientTests/compact/MiniTel.xml -volmgr -destroy -plugin DD4hep_DetectorCheck -name all -geometry -structure -sensitive`
- Simulation smoke test produces an output ROOT file and no fatal DDG4 errors.

## Primary documentation references
- `examples/README.md`
- `examples/ClientTests/README.txt`
- `DDG4/examples/README.txt`
- `examples/DDCodex/README.txt`
- `examples/DDCAD/README.md`

## Escalation workflow
- Start from example README and scripts.
- If needed, inspect `references/doc_map.md`.
- For behavior details, inspect source entry points below.

## Source entry points for unresolved issues
- `examples/CMakeLists.txt`
- `examples/ClientTests/CMakeLists.txt`
- `examples/ClientTests/scripts/DDG4TestSetup.py`
- `examples/ClientTests/scripts/MiniTel.py`
- `examples/ClientTests/scripts/BoxOfStraws_DDSim.py`
- `DDG4/examples/SiDSim.py`
- `DDG4/examples/SiDSim_MT.py`
- `DDG4/hepmc/HepMC3FileReader.cpp`
- `DDG4/hepmc/HepMC3EventReader.cpp`
- `DDG4/src/Geant4DataDump.cpp`
- `DDG4/src/Geant4DataConversion.cpp`
- Prefer targeted source search: `rg -n "<symbol_or_keyword>" DDAlign DDCAD DDCond DDCore DDDetectors DDDigi DDEve DDG4 DDParsers DDRec GaudiPluginService UtilityApps`.
