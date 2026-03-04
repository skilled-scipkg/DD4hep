---
name: dd4hep-releasenotes
description: Use this skill for version-to-version behavior changes and mapping release-note bullets to concrete source checks.
---

# dd4hep: Releasenotes

## High-Signal Playbook
### Route conditions
- Use this skill for regressions across DD4hep versions, behavior deltas, and feature-availability checks.
- After identifying the relevant change, route implementation debugging to the corresponding topic skill.

### Triage questions
- Exact version window (for example `v01-34` -> `v01-35`)?
- Is the issue build/CMake, compact geometry, DDG4 runtime, or I/O backend?
- Which release note bullet or PR number is suspected?

### Canonical workflow
1. Locate release section and bullet in `doc/ReleaseNotes.md`.
2. Extract exact behavior statement and PR number.
3. Map to specific source/CMake files.
4. Verify that those symbols/paths exist in current checkout.
5. Reproduce with the smallest matching script or example.

### Minimal working example
```bash
rg -n "^# v|PR#|DDSim|Geant4|CMake|EDM4hep|HepMC3|VolumeBuilder|run_plugin" doc/ReleaseNotes.md
```

### Convergence/validation checks
- Release bullet identified with explicit version.
- At least one concrete code path mapped and inspected.
- Reproduction command exists (build/test/script) for the claimed behavior.

## Primary documentation references
- `doc/ReleaseNotes.md`
- `README.md`
- `CMakeLists.txt`

## Escalation workflow
- Start from release note bullet(s).
- Then inspect `references/source_map.md` function checkpoints.
- Hand off to build/modeling/simulation skills for remediation details.

## Source entry points for unresolved issues
- `CMakeLists.txt`
- `cmake/DD4hepBuild.cmake`
- `cmake/DD4hep.cmake`
- `DDG4/python/DDSim/DD4hepSimulation.py`
- `DDG4/python/DDSim/Helper/Geometry.py`
- `DDG4/src/Geant4PrimaryHandler.cpp`
- `DDG4/src/Geant4Kernel.cpp`
- `DDG4/src/Geant4UIManager.cpp`
- `DDG4/plugins/Geant4TrackerWeightedSD.cpp`
- `DDG4/hepmc/HepMC3FileReader.cpp`
- `DDG4/hepmc/HepMC3EventReader.cpp`
- `DDCore/src/XML/VolumeBuilder.cpp`
- `DDCore/src/DetectorImp.cpp`
- `DDG4/python/DDG4PhysicsDict.C`
- Prefer targeted source search: `rg -n "<symbol_or_keyword>" CMakeLists.txt cmake DDCore DDG4`.
