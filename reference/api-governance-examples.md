# API Governance Examples

This guide shows how `mova-spec` can model API governance without becoming a runtime specification.

Boundary:

- `mova-spec` can describe schemas, envelopes, profiles, connectors, and episodes.
- `mova-agent-api` decides how an API actually admits and executes calls.

## Example 1: Guard Outbound Customer Export

Language ingredients:

- `ds.connector_core_v1` for the outbound API connector shape
- `ds.instruction_profile_core_v1` for governance rules
- `ds.security_event_episode_core_v1` for evidence
- `env.security_event_store_v1` for recording violations

Recommended action key:

```text
(verb_id = "export", tool_id = 41002, target_kind = "customer_data")
```

Recommended policy effect:

- `deny` in production
- `warn` or `transform` in staging, depending on masking policy

## Example 2: Log All Calls Through One Public API Gateway

Use a tool-level rule:

```json
{
  "rule_id": "log_public_api_gateway",
  "effect": "log_only",
  "target": {
    "kind": "action",
    "tool_id": 31001
  },
  "severity": "medium"
}
```

This is still language-level because it defines governance intent, not runtime middleware behavior.

## Example 3: Require Redaction Before Vendor LLM Call

Use a transform rule:

```json
{
  "rule_id": "redact_before_vendor_llm",
  "effect": "transform",
  "target": {
    "kind": "action",
    "verb_id": "summarize",
    "tool_id": 51004,
    "target_kind": "personal_data"
  },
  "severity": "high"
}
```

## Example 4: Evidence When Policy Blocks A Call

When a guarded API call is blocked, record:

- the original action via `verb_id`, `tool_id`, and optional `target_kind`
- the applied profile via `policy_profile_id`
- the event type via `security_event_type`
- the resulting action via `actions_taken`

This makes the governance decision auditable across package and runtime layers.

## What Belongs Elsewhere

Move to `mova-contract-spec`:

- package manifests
- flow layout
- checks and package-local reference resolution

Move to `mova-agent-api`:

- HTTP routes
- retries
- auth middleware
- execution pipeline
- response codes
