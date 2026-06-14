# MOVA Core Concepts

This document defines the current core language model of `mova-spec`.

## Scope

`mova-spec` defines language validity only.

It owns:

- `ds.*` schemas
- `env.*` envelopes
- `global.*` catalogs
- verbs, tools, and actions
- episodes and security vocabulary

It does not own:

- contract package assembly
- runtime execution
- API implementation

## The Main Building Blocks

### Data schemas

`ds.*` schemas define the shape of valid data.

They answer:

- what fields exist
- which fields are required
- which values are allowed

### Envelopes

`env.*` schemas define typed speech-acts over that data.

They answer:

- what kind of message is being expressed
- which verb is used
- which payload is carried

### Verbs, tools, and actions

Verb and tool are separate vocabularies.

- `verb`: what is being done
- `tool`: by which channel it is done
- `action_signature`: the operational primitive

```text
action_signature := (verb_id, tool_id, target_kind?)
```

Rules:

- `tool_id = 0` means tool-less action
- exact `action_signature` matching has highest policy priority
- `target_kind` is optional but useful for governance

### Global catalogs

`global.*` provides shared semantic vocabularies.

It is:

- shared
- non-executing
- non-authoritative for runtime transitions

### Episodes

Episodes are structured evidence of what happened.

They are used for:

- audit
- reproducibility
- policy evidence
- analysis

## Evolution Summary

- MOVA `4.x`: established schemas, envelopes, verbs, episodes, and global catalogs
- MOVA `6.0.0`: made `verb`, `tool`, and `action_signature` explicit
- MOVA `7.0.0`: clarified layer boundaries and made `contracts` the canonical term over legacy `skills`

## Authoring Rule

When composing a valid MOVA artifact:

1. define data shape
2. define speech-act
3. define action semantics
4. define evidence shape
5. stop before package or runtime concerns

For package canon, continue in `mova-contract-spec`.
For execution behavior, continue in `mova-agent-api`.
