# MOVA Ecosystem Manifesto v1

## Mission

MOVA exists to make AI/automation systems **describable, comparable, and auditable** across different vendors and stacks.

The ecosystem goal is simple:

- teams build agents, workflows, and service automations in any stack they want;
- behavior is described in a shared contract language;
- execution is measured in a reproducible way;
- results are compared on evidence, not marketing claims.

## What MOVA Is (and Is Not)

MOVA is:

- a contract language (`ds.*`, `env.*`, `global.*`);
- a validation surface for interoperability;
- a base for observability and reproducible certification.

MOVA is not:

- an agent runtime;
- a model provider;
- a locked platform.

This separation is intentional: executors can change, contracts stay stable.

## Ecosystem Components

## 1) Core language

- Repository: `mova-spec`
- Responsibility: universal schemas, envelopes, global vocabularies, validation rules.
- Output: stable canonical contracts.

## 2) Agent profile (optional domain extension)

- Repository: `mova-agent-profile`
- Responsibility: describe an agent contract and execution results in MOVA form.
- Output: vendor-neutral agent profile documents.

## 3) Workflow implementations (first-class usage, no extra language required)

- Repository example: `https://github.com/Leryk1981/mova_smartlink`
- Responsibility: describe/validate real process steps and execution episodes with MOVA core contracts.
- Output: reproducible workflow records and validated process interfaces.

## 4) Arena comparison profile

- Repository: `mova-arena-profile`
- Responsibility: normalize observability rows for fair comparison and certification requests.
- Output: comparable, schema-validated arena records.

## 5) Arena observability SDK

- Repository: `mova-arena-sdk`
- Responsibility: produce compact+expanded run journals and normalize source formats.
- Output: deterministic run artifacts suitable for referee analysis.

## 6) Domain dictionaries

- Repository: `mova-agent-dictionaries`
- Responsibility: build compact domain dictionaries from datasets with noise control.
- Output: JSON + binary dictionaries, plus build reports.

## Core Principles

1. **Contract first**: validate structure before judging intelligence.
2. **Vendor-neutral**: compare behavior, not implementation brand.
3. **Evidence first**: every claim must be backed by run artifacts.
4. **Minimal core**: keep binary observability payload compact and deterministic.
5. **No lock-in**: users keep freedom of runtime/model/provider.
6. **Object-agnostic language**: MOVA describes agents, workflows, services, and hybrid systems with the same core primitives.

## How Developers Use MOVA

## Track A: Describe Existing Agent

1. Build your agent in your preferred stack.
2. Map its capabilities/config to `mova-agent-profile`.
3. Validate JSON against schemas.
4. Run tasks and produce observability logs via `mova-arena-sdk`.
5. Submit run evidence in arena format.

## Track B: Describe or Build Workflow

1. Model process interfaces and step outputs with MOVA core (`ds.*`, `env.*`, `global.*`).
2. Validate workflow payloads against schemas.
3. Record deterministic episodes for each executed step.
4. Use evidence for pre-prod validation and regression control.

## Track C: Design From Contract

1. Start from MOVA schemas as target interfaces.
2. Fill contract fields for your planned agent/workflow.
3. Implement runtime to satisfy those contracts.
4. Validate, run, compare, iterate.

All tracks end in the same place: reproducible evidence and comparable results.

## Minimal Working Example

## Step 1: Validate core contracts

```bash
cd mova-spec
npm ci
npm test
```

## Step 2A (optional): Validate agent profile

```bash
cd ../mova-agent-profile
npm ci
npm test
```

## Step 2B (workflow path): validate MOVA workflow-facing payloads in your project

Use MOVA schemas as contract checks at workflow boundaries (inputs, outputs, episodes).

## Step 3: Start deterministic observability run

```powershell
cd ..\mova-arena-sdk
cargo run -- session-start --run-dir D:\runs\demo --run-id demo-001 --actor-id agent_a --dictionary-id customer_support_v1 --dictionary-ver 1.0.0
cargo run -- session-step --run-dir D:\runs\demo --verb execute --object ticket://123 --tool http.get --result ok --reason none --duration-ms 42
cargo run -- session-finish --run-dir D:\runs\demo --verdict PASS --summary "task completed"
cargo run -- session-verify --run-dir D:\runs\demo
```

## Step 4: Build domain dictionary

```bash
cd ../mova-agent-dictionaries
python tools/build_agent_dictionaries.py --profile agent_dictionaries/profiles/customer_support_v1.profile.json
```

Result: you have validated contracts, deterministic run logs, and domain vocabulary ready for comparable agent or workflow evaluation.

## What the Community Can Build

- vendor adapters that emit MOVA-compatible observability rows;
- referee/scoring modules for specialized domains;
- domain dictionary packs (legal, medical, finance, support, security);
- benchmark suites with public golden datasets;
- compliance packs (trust, legal evidence, reproducibility).

## Quality Bar for Contributions

A contribution is accepted as ecosystem-grade when it is:

- schema-valid;
- reproducible (same inputs -> same artifact shape);
- auditable (clear provenance and versioning);
- minimal in core payload (no noise in compact records).

## Governance Position

- `mova-spec` remains stable and conservative.
- Domain profiles evolve independently in their own repositories.
- Extensions should not mutate core semantics unless there is a clear language-level gap.

## Call to Builders

If you develop agents, workflows, or automation tools:

- keep your stack and freedom of implementation;
- expose behavior through MOVA contracts;
- publish comparable evidence;
- improve performance through transparent iteration, not opaque claims.

That is how an open, practical, and trustworthy agent ecosystem scales.
