# MOVA 7.0.0 Boundary Freeze v0

## 1. Verdict
PASS — BOUNDARY_CLEAN

## 2. Repository roles
- mova-spec: language canon
- mova-contract-spec: contract/package canon
- mova-mcp: registration/admission/runtime control surface

## 3. Global placement
- mova-spec: defines global as language-level semantic reference / AI semantic admission vocabulary
- mova-contract-spec: defines package-level global as optional non-authoritative semantic context
- mova-mcp: reads/validates global_ref as non-authoritative metadata; runtime behavior unchanged

## 4. Contracts layer
- `contracts` replaces `skills` as canonical MOVA layer vocabulary
- remaining `skills` mentions are compatibility/migration only

## 5. Non-authority invariants
- global does not execute
- global does not grant permissions
- global does not choose transitions
- global does not decide gates
- global does not define terminal outcomes
- global does not override DS
- global does not override ENV/runtime bindings
- global does not override flow/classification/checks/models

## 6. Commits
- mova-contract-spec e657b77 `spec: add package-level global semantics`
- mova-contract-spec 82005a5 `schema: require non-empty package global role use arrays`
- mova-mcp 747b5c2 `mcp: support non-authoritative package global_ref`
- mova-spec 7c16446 `spec: clarify contracts boundary and global semantics for 7.0.0`

## 7. Next allowed work
- optional: persist package_global in backend registry only if product need exists
- optional: add richer validation against mova-contract-spec schema
- optional: update examples/templates
- not allowed without new decision: move full package canon into mova-spec
