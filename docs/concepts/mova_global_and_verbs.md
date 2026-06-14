# MOVA Global Layer And Verbs

This document explains the current semantic layer and operation vocabulary of MOVA.

## Global Layer

`global.*` is the shared semantic reference layer.

It contains vocabularies for:

- episode types
- security event and action types
- layers and namespaces
- text channels

It must not:

- execute
- grant permissions
- decide runtime transitions
- replace package-local structure

Current layered model:

- `global.layers_and_namespaces_v2.json` is current
- `global.layers_and_namespaces_v1.json` is retained for compatibility

## Verbs

Verbs are operation types.

Core verbs in this repository include:

- `create`
- `update`
- `route`
- `record`
- `publish`

Verbs describe intent, not implementation.

## Tools

Tools describe the execution channel or medium.

Examples:

- retrieval system
- filesystem
- external API
- shell
- no named tool, represented as `tool_id = 0`

## Action

Action is derived from verb plus tool at the point of execution or recording.

```text
action_signature := (verb_id, tool_id, target_kind?)
```

Normative consequences:

- policy can target exact actions without enumerating every pair in a dictionary
- tool-less behavior is explicit
- audit keys are stable

## Transformation History

- legacy thinking often centered on verbs alone
- MOVA `6.0.0` formalized `verb + tool -> action_signature`
- MOVA `7.0.0` kept that model and cleaned the surrounding boundary language

## Authoring Rule For Agents

When you need a policy key:

- first try exact `action_signature`
- fall back to `verb_id` only when broad control is intended
- use `tool_id` only when the connector boundary is the main concern
