# Security Profile Patterns

This guide shows repeatable patterns for `ds.instruction_profile_core_v1`.

Use these patterns to write policy that is readable for humans and deterministic for agents.

## Pattern 1: Deny One Exact Action

Use when one `(verb_id, tool_id, target_kind)` combination is unacceptable.

```json
{
  "rule_id": "deny_external_customer_export",
  "effect": "deny",
  "target": {
    "kind": "action",
    "verb_id": "export",
    "tool_id": 41002,
    "target_kind": "customer_data"
  },
  "severity": "critical"
}
```

Why:

- highest determinism
- no bleed into unrelated actions

## Pattern 2: Allow Or Warn On Broad Verb

Use when the verb is the main governance axis.

```json
{
  "rule_id": "warn_on_delete",
  "effect": "warn",
  "target": {
    "kind": "action",
    "verb_id": "delete"
  },
  "severity": "high"
}
```

Why:

- easier than enumerating every tool
- useful for broad operator review

## Pattern 3: Log One Tool Across Many Verbs

Use when the connector itself is the concern.

```json
{
  "rule_id": "log_all_public_api_calls",
  "effect": "log_only",
  "target": {
    "kind": "action",
    "tool_id": 31001
  },
  "severity": "medium"
}
```

Why:

- keeps governance attached to the integration boundary

## Pattern 4: Explicit Tool-Less Governance

Use when internal reasoning without tools is allowed or restricted differently.

```json
{
  "rule_id": "allow_tool_less_analysis",
  "effect": "allow",
  "target": {
    "kind": "action",
    "verb_id": "analyze",
    "tool_id": 0
  },
  "severity": "info"
}
```

Why:

- avoids ambiguity between unknown tool and no tool

## Pattern 5: Transform Before Execution

Use when requests must be normalized or redacted before crossing a boundary.

```json
{
  "rule_id": "transform_pii_before_vendor_api",
  "effect": "transform",
  "target": {
    "kind": "action",
    "tool_id": 31001,
    "target_kind": "personal_data"
  },
  "severity": "high"
}
```

Why:

- makes the contract explicit before runtime-specific logic is applied

## Authoring Rules

- Prefer `kind = "action"` when policy depends on verbs or tools.
- Use `tool_id = 0` explicitly for tool-less rules.
- Keep `description` declarative, not prompt-like.
- Keep rule ids stable.
- Use namespaced extensions only when core catalog values are insufficient.

## Escalation Rules

Move to `mova-contract-spec` when the policy depends on package-level manifests, flow graphs, or checks.

Move to `mova-agent-api` when the policy depends on runtime admission, HTTP status behavior, retries, or execution routing.
