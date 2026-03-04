---
name: dd4hep-simulation-workflows
description: This skill should be used when users ask about simulation workflows in dd4hep; it prioritizes documentation references and then source inspection only for unresolved details.
---

# dd4hep: Simulation Workflows

## High-Signal Playbook
### Route conditions
- Use this skill for DDG4 and `ddsim` steering, run controls (`batch`, `vis`, `run`, `shell`, `qt`), user actions/plugins, and output configuration.
- Route compact XML detector construction issues to `dd4hep-inputs-and-modeling`.
- Route build/dependency/plugin availability issues to `dd4hep-build-and-install`.

### Triage questions
- Are you running `ddsim` or a direct DDG4 Python script (`DDG4.Kernel`)?
- Input mode: `--inputFiles`, `--enableGun`, or `SIM.inputConfig.userInputPlugin`?
- Which run mode is needed (`batch`, `vis`, `run`, `shell`, `qt`)?
- Output target: DD4hep ROOT, LCIO, or EDM4hep?
- Are custom sensitive actions/filters needed for specific detectors?
- What exact error is shown in consistency checks or during detector setup?

### Canonical workflow
1. Source runtime environment (`<install>/bin/thisdd4hep.sh`).
2. Pick compact geometry and a minimal steering file.
3. Start with `ddsim` in `batch` mode for 1-10 events.
4. Enable one input path only: file input, DD4hep gun, or user input plugin.
5. Configure output plugin/format consistently with compiled dependencies.
6. Add custom actions/filters via `SIM.action.*`, `SIM.filter.*`, or `SIM.geometry.regexSensitiveDetector`.
7. Run smoke test, then scale event count.
8. Validate geometry/material response with `g4GeometryScan` / `g4MaterialScan` if behavior is suspicious.

### Minimal working example
```bash
ddsim \
  --steeringFile examples/ClientTests/scripts/BoxOfStraws_DDSim.py \
  --compactFile examples/ClientTests/compact/BoxOfStraws_sensitive.xml \
  --enableGun \
  --numberOfEvents 5 \
  --outputFile sim.root
```

```python
from DDSim.DD4hepSimulation import DD4hepSimulation
SIM = DD4hepSimulation()
SIM.compactFile = ["examples/ClientTests/compact/BoxOfStraws_sensitive.xml"]
SIM.enableGun = True
SIM.numberOfEvents = 5
SIM.outputFile = "sim.root"
SIM.outputConfig.forceDD4HEP = True
SIM.action.trackerSDTypes = ["sensitive"]
```

### Pitfalls and fixes
- Symptom: `ERROR: No geometry compact file provided`. Fix: set `--compactFile` or `SIM.compactFile`.
- Symptom: batch mode errors for missing events/input. Fix: set `--numberOfEvents` and one valid input path (`--inputFiles`, `--enableGun`, or `userInputPlugin`).
- Symptom: conflicting input sources. Fix: do not combine DD4hep gun with Geant4 gun/GPS or file input combinations blocked by consistency checks.
- Symptom: unknown sensitive detector types. Fix: configure `SIM.action.trackerSDTypes` / `SIM.action.calorimeterSDTypes` or per-detector `mapActions`.
- Symptom: output plugin runtime error. Fix: match output mode with compiled capabilities (`forceLCIO` requires LCIO build, `forceEDM4HEP` requires EDM4hep build).
- Symptom: custom plugin not applied. Fix: verify callable signatures for `userInputPlugin` / `userOutputPlugin` and ensure returned DDG4 actions are registered.

### Convergence/validation checks
- Run 1-event smoke test first, then 10/100 events.
- Confirm expected output file exists and format matches configuration (`.root`, `.slcio`).
- Use `-v DEBUG` or helper output levels to verify detector/action setup.
- For geometry-driven anomalies, run `g4GeometryScan` and `g4MaterialScan` on the same compact file.
- Keep random seeds fixed for reproducibility while debugging (`SIM.random.seed`, `SIM.random.enableEventSeed`).

## Primary documentation references
- `doc/usermanuals/DDG4/sections/Setup.tex`
- `DDG4/examples/README.txt`
- `DDG4/examples/SiDSim.py`
- `examples/ClientTests/scripts/DDG4TestSetup.py`
- `examples/ClientTests/scripts/BoxOfStraws_DDSim.py`

## Escalation workflow
- Start from manual setup patterns and working example scripts.
- If needed, inspect `references/doc_map.md`.
- For unresolved behavior, inspect DDSim helper internals and DDG4 action implementations.

## Source entry points for unresolved issues
- `DDG4/python/DDSim/DD4hepSimulation.py` (CLI parsing, consistency checks, full run pipeline)
- `DDG4/python/DDSim/Helper/Action.py`
- `DDG4/python/DDSim/Helper/Filter.py`
- `DDG4/python/DDSim/Helper/Geometry.py`
- `DDG4/python/DDSim/Helper/InputConfig.py`
- `DDG4/python/DDSim/Helper/OutputConfig.py`
- `DDG4/python/bin/g4GeometryScan.py`
- `DDG4/python/bin/g4MaterialScan.py`
- `DDG4/src/Geant4RunAction.cpp`
- `DDG4/src/Geant4SteppingAction.cpp`
- `DDG4/plugins/Geant4RunManagers.cpp`
- `DDG4/plugins/Geant4Steppers.cpp`
- `UtilityApps/src/run_plugin.h`
- `UtilityApps/src/plugin_runner.cpp`
- Prefer targeted source search: `rg -n "<symbol_or_keyword>" DDAlign DDCAD DDCond DDCore DDDetectors DDDigi DDEve DDG4 DDParsers DDRec GaudiPluginService UtilityApps`.
