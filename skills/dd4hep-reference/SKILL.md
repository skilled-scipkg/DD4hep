---
name: dd4hep-reference
description: Use this skill for precise symbol lookup (declaration, implementation, call path) across DD4hep/DDG4.
---

# dd4hep: Reference

## High-Signal Playbook
### Route conditions
- Use this skill when users ask for exact class/function/macro semantics and where behavior is implemented.
- Route build/runtime execution issues to workflow skills after symbol resolution.

### Triage questions
- Which symbol is needed exactly (name + namespace if known)?
- Is this in DDCore geometry, DDG4 simulation, plugin factories, or Python bindings?
- Do we need declaration only or declaration+implementation+usage chain?

### Canonical workflow
1. Start from `doc/reference/README.md` to frame API lookup.
2. Find declaration in `include/`.
3. Find implementation in `src/`/`plugins`/Python helper.
4. Validate at least one usage in examples/tests or runtime entrypoints.
5. Return symbol path(s) and behavior summary.

### Minimal working example
```bash
rg -n "DECLARE_DETELEMENT|Detector::fromXML|Detector::apply|DD4hepSimulation|setupDetector" \
  DDCore/include DDCore/src DDG4/python DDG4/src examples
```

### Convergence/validation checks
- Declaration and implementation both located.
- Caller/usage site identified.
- Behavior-sensitive conclusions backed by current source, not only release notes.

## Primary documentation references
- `doc/reference/README.md`
- `doc/CMakeLists.txt`
- `README.md`

## Escalation workflow
- Start from docs.
- If symbol is not covered in docs, use `references/doc_map.md`, then `references/source_map.md`.
- Prefer targeted symbol search instead of broad scans.

## Source entry points for unresolved issues
- `DDCore/include/DD4hep/Detector.h`
- `DDCore/src/DetectorImp.cpp`
- `DDCore/include/DD4hep/DetElement.h`
- `DDCore/src/DetElement.cpp`
- `DDCore/include/DD4hep/Factories.h`
- `DDCore/src/plugins/Compact2Objects.cpp`
- `DDG4/python/DDG4.py`
- `DDG4/python/DDSim/DD4hepSimulation.py`
- `DDG4/src/Geant4Kernel.cpp`
- `DDG4/src/Geant4UIManager.cpp`
- `DDG4/pyddg4.cpp`
- Prefer targeted source search: `rg -n "<exact_symbol>" DDCore DDG4 DDDetectors DDRec examples`.
