# MOVA 6.0.0 — Release Notes

## Summary

MOVA 6.0.0 introduces the normative definition of **verb**, **tool**, and **action_signature** — three concepts that were present implicitly in MOVA SDK and dictionaries but lacked a canonical definition in the core specification.

This is a **non-breaking additive release**: all existing `_core_v1` schema ids, historical document filenames, and canonical envelope shapes are preserved unchanged. New fields (`verb_id`, `tool_id`, `target_kind`) are optional in the episode schema; existing documents remain valid.

---

## What changed

### 1. Core specification (`docs/mova_4.1.1_core.md`) — new §3.2

Added normative definitions:

- **Verb** = abstract type of operation ("what is being done"), identified by `verb_id`.
- **Tool** = execution channel/medium ("by what means"), identified by `tool_id`. May be absent; `tool_id = 0` is canonical for "tool-less action".
- **Action** = minimal atomic unit for policy evaluation, audit, and cross-session comparison, expressed as:

  ```
  action_signature := (verb_id, tool_id, target_kind?)
  ```

Normative rules added:
- **MUST**: Episodes describing a performed operation MUST carry `verb_id`; `tool_id` MUST be set to `0` for tool-less actions.
- **MUST**: `tool_id = 0` is a valid, non-error value meaning "no external tool was used".
- **SHOULD**: `target_kind`, when determinable, SHOULD be included for finer-grained policy matching.

### 2. Global layer and verbs (`docs/mova_4.1.1_global_and_verbs.md`) — new §4.5 and §4.6

**§4.5 — Verb and tool are defined separately; action is derived**

- Domain dictionaries define verb and tool vocabularies independently.
- **MUST NOT**: dictionaries MUST NOT enumerate (verb, tool) pairs as a way to define valid actions.
- An action is the combination of `(verb_id, tool_id)` at execution time, derived from the episode record, not declared in any dictionary.

**§4.6 — `action_labels` (optional, for readability)**

- Domain dictionaries MAY include an `action_labels` array of `{ verb_id, tool_id, label, description }` entries.
- Labels are for UI and journal display only — they carry no normative weight.
- Identity remains the `(verb_id, tool_id)` tuple; absence of a label does not mean an action is invalid.

### 3. Security layer (`docs/mova_4.1.1_security_layer.md`) — new §12

**§12 — Policy matching by action_signature**

Instruction profile rules may target operations at three levels:
1. `action_signature` match — `(verb_id, tool_id, target_kind?)` — most specific.
2. `verb_id` match — applies to all actions with that verb regardless of tool.
3. `tool_id` match — applies to all actions through that channel regardless of verb.

**Normative priority order (MUST)**:
```
Priority 1 (highest): action_signature — (verb_id, tool_id, target_kind?)
Priority 2:           verb_id           — (verb_id, *)
Priority 3 (lowest):  tool_id           — (*, tool_id)
```

At the same priority level, stricter effect wins: `deny > transform > warn > log_only > allow`.

Rule target schema extended with `kind: "action"` and optional `verb_id`, `tool_id`, `target_kind` fields.

### 4. Episode schema (`schemas/ds.mova_episode_core_v1.schema.json`)

Added three optional fields:

| Field | Type | Description |
|-------|------|-------------|
| `verb_id` | string | Verb (operation type) identifier. First component of `action_signature`. |
| `tool_id` | string \| integer | Tool (channel) identifier. `0` = tool-less action. Second component. |
| `target_kind` | string | Kind of the object acted on. Optional third component of `action_signature`. |

No existing fields changed. No required fields added.

### 5. Examples (`examples/action_signature.example.json`) — new file

Added two canonical episode examples:
- Episode with `tool_id = 0` ("analyze" without any named tool).
- Episode with `tool_id = 12003` ("analyze" via retrieval service) and `target_kind = "document"`.

Inline annotations show the derived `action_signature` for each episode and explain which policy rules would match them.

---

## Migration guide

**For existing implementations**: no migration required. Existing `ds.mova_episode_core_v1` documents without `verb_id`/`tool_id` remain valid. Adding these fields to new episodes is recommended for full policy and audit alignment.

**For policy authors**: to use action_signature-based rules in `ds.instruction_profile_core_v1`, set `target.kind = "action"` and specify `verb_id` and/or `tool_id` in the rule target. See `docs/mova_4.1.1_security_layer.md §12.3`.

**For dictionary maintainers**: do not add (verb, tool) pair tables to dictionaries. Add `action_labels` only if human-readable labels for frequent actions are needed for UI.

---

## Compatibility

- All `_core_v1` schema ids: **unchanged**.
- All `mova_4.1.1_*` document filenames: **unchanged** (retained for path stability).
- All existing examples and envelopes: **valid without modification**.
- New fields in `ds.mova_episode_core_v1`: **optional** — no breaking change.
