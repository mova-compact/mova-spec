# MOVA Spec 6.0.0

**Machine-Operable Verbal Actions — Core Specification**

MOVA is a language of machine-operable contracts for data and actions. It defines what is valid — not how to execute it.

```
Schema  (ds.*)      →  "what the data looks like"
Envelope (env.*)    →  "what speech-act is being performed"
Verb                →  "what type of operation"
Tool                →  "the channel/medium of execution (may be absent)"
Action              →  "atomic unit: action_signature = (verb_id, tool_id, target_kind?)"
Global  (global.*)  →  "shared vocabulary across all layers"
```

MOVA itself **never executes anything**. Execution lives outside.  
Any executor that supports MOVA treats it as a **contract**: valid input, valid output, valid episode.

---

## Core Concepts

### Data Schemas (`ds.*`)

JSON Schema (draft 2020-12) definitions for domain objects and language artefacts. Each schema defines field types, constraints, required/optional fields, and allowed values. Schemas describe **what the data looks like**, not how it is processed.

Red-core schemas in this repository:

| Schema | Purpose |
|--------|---------|
| `ds.mova_schema_core_v1` | Core language for schemas themselves |
| `ds.mova_episode_core_v1` | Core episode frame |
| `ds.security_event_episode_core_v1` | Security event episode |
| `ds.instruction_profile_core_v1` | Policies and guardrails |
| `ds.runtime_binding_core_v1` | Runtime binding |
| `ds.connector_core_v1` | Connector contracts |
| `ds.ui_text_bundle_core_v1` | UI text bundles |
| `ds.mova4_core_catalog_v1` | Core catalog model |

### Verbs, Tools and Actions

**Verbs** are abstract operation types: `create`, `update`, `delete`, `validate`, `route`, `record`, `explain`, `plan`, `analyze`, `summarize`. Verbs appear in envelopes (to express intent) and episodes (to record what was done).

**Tools** are the execution channels or means through which a verb is performed (e.g. retrieval service, file system, external API). A tool may be absent; `tool_id = 0` means "tool-less action".

**Action** is the minimal atomic unit for policy and audit, expressed as an **action signature**:
```
action_signature := (verb_id, tool_id, target_kind?)
```
`tool_id = 0` is canonical for tool-less. Policies can match by `action_signature`, `verb_id`, or `tool_id`, with action_signature taking highest priority (see `docs/mova_4.1.1_security_layer.md §12`).

### Envelopes (`env.*`)

Structured speech-acts — requests, commands, events — that tie together a verb, data schema references, roles, and metadata.

| Envelope | Purpose |
|----------|---------|
| `env.mova4_core_catalog_publish_v1` | Publish core catalog |
| `env.instruction_profile_publish_v1` | Publish instruction profiles |
| `env.security_event_store_v1` | Store security events |

### Global Semantic Layer (`global.*`)

Shared vocabularies used across all schemas, envelopes, and episodes to keep terminology consistent:

| Catalog | Contents |
|---------|----------|
| `global.episode_type_catalog_v1` | Episode type vocabulary |
| `global.security_catalog_v1` | Security event and action types |
| `global.layers_and_namespaces_v1` | Layered model structure |
| `global.text_channel_catalog_v1` | Text channel definitions |

### Episodes & Genetic Layer

Structured records of meaningful work steps — identifiers, timestamps, executor identity, result status, references to changed data, optional metrics and explanations. Episodes form the basis for audit, reproducibility, analytics, and a "genetic layer" (pattern memory built from many episodes).

### Security Layer

First-class security contracts — not ad-hoc code:

- **Instruction profiles** (`ds.instruction_profile_core_v1`) — declarative policies and guardrails  
- **Security events** (`ds.security_event_episode_core_v1`) — structured security episodes  
- **Security catalogs** (`global.security_catalog_v1`) — shared security vocabulary  
- **Model versioning** — `security_model_version` to track which security model a profile or event uses

### Text & UI Layer

Formalises separation between human-facing UI text (`human_ui`), model instructions (`model_instruction`), and system logs (`system_log`). Ensures prompts don't leak through UI and each channel can be audited and evolved independently.

### Runtime & Connectors

MOVA doesn't execute, but describes where and how execution happens:

- `ds.runtime_binding_core_v1` — runtime ids, kinds, environment profiles, capabilities  
- `ds.connector_core_v1` — provider, resource, protocol, auth methods, rate limits, error contracts

---

## Repository Layout

```
docs/                              Normative specification documents
  mova_4.1.1_core.md               Core language specification (verb/tool/action §3.2 new in 6.0.0)
  mova_4.1.1_global_and_verbs.md   Global layer, verb catalogue, action_labels (§4.5–4.6 new in 6.0.0)
  mova_4.1.1_episodes_and_genetic_layer.md
  mova_4.1.1_layers_and_namespaces.md
  mova_4.1.1_security_layer.md     Policy matching by action_signature §12 new in 6.0.0
  mova_4.1.1_text_and_ui_layer.md
  mova_4.1.1_runtime_and_connectors.md
  MOVA_6.0.0_RELEASE_NOTES.md
  MOVA_5.0.0_RELEASE_NOTES.md
  archive/4.0.0/                   Frozen MOVA 4.0.0 documents

schemas/                           JSON Schemas (draft 2020-12)
  ds.*.schema.json                 Data structure schemas
  env.*.schema.json                Envelope schemas

examples/                          Sample JSON documents
  action_signature.example.json    action_signature with/without tool (new in 6.0.0)

global.*.json                      Global semantic catalogs

tools/validate_all.js              Ajv-based schema validation
bin/mova-validate.mjs              CLI validator
```

---

## Quick Start

```bash
git clone https://github.com/mova-compact/mova-spec
cd mova-spec
npm ci
npm test
```

Validate a specific document against a schema:

```bash
node bin/mova-validate.mjs \
  --schema https://mova.dev/schemas/ds.mova_episode_core_v1.schema.json \
  examples/env.security_event_store_v1.example.json
```

---

## Ecosystem

This repository is part of the MOVA ecosystem:

| Repository | Role | Link |
|------------|------|------|
| **mova-spec** | The language — schemas, verbs, envelopes, vocabulary | *you are here* |
| **mova-agent-profile** | Agent domain extension — contracts, results, task envelopes | [GitHub](https://github.com/mova-compact/mova-agent-profile) |
| **mova-agent-dictionaries** | Domain semantics — numeric ID ↔ verb/tool/condition mapping | [GitHub](https://github.com/mova-compact/mova-agent-dictionaries) |
| **mova_sdk** | Runtime engine — Rust compact binary, contract proof, CLI/API | [GitHub](https://github.com/mova-compact/mova_sdk) |

**MOVA Spec** defines what is valid. **MOVA SDK** enforces and records it.
**Dictionaries** give domain meaning. **Agent Profile** extends for agent-specific contracts.

Two committed demo runs in `mova_sdk` show the spec in practice: a deterministic workflow (116 bytes, score 100) and an AI agent with quality gate (97 bytes, two attempts, score 96). All artifacts — `core.mova`, `sidecar.jsonl`, journal, evaluation — are inspectable directly in the repository.

---

## Versioning

- Canonical version: **6.0.0**
- All schemas: JSON Schema draft 2020-12
- Breaking changes → new IDs (`*_v2`), never silent mutations
- MOVA 4.0.0 archived in `docs/archive/4.0.0/`
- Historical document filenames (`mova_4.1.1_*`) are retained for path stability

## Governance

Single-author specification maintained by Sergii Miasoiedov. Feedback via GitHub Issues welcome. PRs to core language at author's discretion; contributions to examples and tooling are welcome.

## License

Apache License 2.0

---

**Website:** [mova-lab.eu](https://mova-lab.eu) · **Author:** Sergii Miasoiedov
