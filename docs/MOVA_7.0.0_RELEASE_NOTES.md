# MOVA 7.0.0 Release Notes

## Summary

MOVA 7.0.0 clarifies repository boundaries and canonical terminology after contract-global alignment across `mova-contract-spec` and `mova-mcp`.

## Key changes

1. Canonical terminology cleanup:
   - `skills` is no longer canonical layer vocabulary in MOVA language docs.
   - canonical layer term is `contracts`.
2. `global.*` clarified:
   - `global.*` is a semantic reference layer and AI semantic admission vocabulary.
   - `global.*` is non-executing and non-authoritative for runtime control.
3. Repository ownership clarified:
   - `mova-spec` remains language canon.
   - `mova-contract-spec` owns full contract/package canon.
   - `mova-mcp` owns registration/admission/runtime control surface.
4. Layer catalog update:
   - added `global.layers_and_namespaces_v2.json`.
   - v2 supersedes v1 and migrates layer terminology from `skills` to `contracts`.

## Boundary statement

This release does not move full contract package layout into `mova-spec`.
Package structure details (manifest/flow/classification/runtime bindings/checks) remain in `mova-contract-spec`.

## Runtime statement

No runtime behavior is added by `mova-spec` 7.0.0.
No execution authority is granted to AI or to `global.*`.
