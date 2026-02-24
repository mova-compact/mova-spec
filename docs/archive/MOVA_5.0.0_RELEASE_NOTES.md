# MOVA 5.0.0 — Release Notes

## Summary

MOVA 5.0.0 is a major version update for core schema unification, layering clarity, and stable envelope contracts.

Note on architecture split:
- Agent-domain contracts are maintained in a dedicated extension repository:
  - `https://github.com/mova-compact/mova-agent-profile`
- This repository (`mova-spec`) remains universal core only.

## Why major version bump

The language scope in 5.0.0 stabilized:

1. core schema conventions and envelope framing;
2. red-core layering boundaries for security/runtime/ui/episodes;
3. consistent versioning and compatibility policy for canonical ids.

## Compatibility notes

- Existing `_core_v1` schema ids are preserved.
- Historical document filenames with `mova_4.1.1_*` are retained for path stability.
- `MOVA 5.0.0` is the canonical product/version label going forward.
- Agent profile schemas keep their existing ids and are published from the extension profile repository.
