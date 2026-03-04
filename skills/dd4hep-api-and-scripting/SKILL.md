---
name: dd4hep-api-and-scripting
description: Use this skill for DDG4/DDSim Python API usage, steering file authoring, and script-level behavior debugging.
---

# dd4hep: API and Scripting

## High-Signal Playbook
### Route conditions
- Use this skill for Python API usage (`DDG4`, `DDSim` helpers), steering file behavior, and scripting-level automation.
- Route CMake/dependency/build setup to `dd4hep-build-and-install`.
- Route detector XML/C++ constructor logic to `dd4hep-inputs-and-modeling`.
- Route end-to-end runtime pipeline debugging to `dd4hep-simulation-workflows`.

### Triage questions
- Are you using a pure DDG4 script (`DDG4.Kernel`) or `ddsim`/`DD4hepSimulation`?
- Is the issue in input configuration, action/filter mapping, geometry setup, or output plugin selection?
- Which helper is being customized (`Action`, `Filter`, `InputConfig`, `OutputConfig`, `Geometry`)?
- Do you need one-off CLI usage or reusable steering-file code?

### Canonical workflow
1. Start from a known-good steering script in `examples/ClientTests/scripts` or `DDG4/examples`.
2. Make one API change at a time (input, then actions, then output).
3. Run 1-5 events with verbose logs.
4. Verify output format and detector/action registration.
5. Escalate to helper internals when behavior differs from expectations.

### Minimal working example
```python
from DDSim.DD4hepSimulation import DD4hepSimulation

SIM = DD4hepSimulation()
SIM.compactFile = ["examples/ClientTests/compact/BoxOfStraws_sensitive.xml"]
SIM.enableGun = True
SIM.numberOfEvents = 5
SIM.outputFile = "api_smoke.root"
SIM.outputConfig.forceDD4HEP = True
SIM.action.trackerSDTypes = ["sensitive"]
```

```bash
ddsim --steeringFile examples/ClientTests/scripts/BoxOfStraws_DDSim.py \
      --compactFile examples/ClientTests/compact/BoxOfStraws_sensitive.xml \
      --enableGun --numberOfEvents 5 --outputFile api_smoke.root
```

### Convergence/validation checks
- `python -c "import DDG4; from DDSim.DD4hepSimulation import DD4hepSimulation"` succeeds.
- Steering changes appear in debug output (`-v DEBUG`).
- Output plugin choice matches enabled dependencies and requested file type.

## Primary documentation references
- `doc/usermanuals/DDG4/sections/Setup.tex`
- `DDG4/examples/README.txt`
- `DDG4/examples/SiDSim.py`
- `examples/ClientTests/scripts/DDG4TestSetup.py`
- `examples/ClientTests/scripts/BoxOfStraws_DDSim.py`

## Escalation workflow
- Start with the references above.
- If details are missing, inspect `references/doc_map.md`.
- For unresolved script behavior, inspect `references/source_map.md` and target listed helper functions.

## Source entry points for unresolved issues
- `DDG4/python/DDSim/DD4hepSimulation.py`
- `DDG4/python/DDSim/Helper/Action.py`
- `DDG4/python/DDSim/Helper/Filter.py`
- `DDG4/python/DDSim/Helper/Geometry.py`
- `DDG4/python/DDSim/Helper/InputConfig.py`
- `DDG4/python/DDSim/Helper/OutputConfig.py`
- `DDG4/python/DDG4.py`
- `DDG4/python/DDSim/bin/ddsim.in.py`
- Prefer targeted source search: `rg -n "<symbol_or_keyword>" DDG4/python`.
