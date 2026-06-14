# Contributing To MOVA Spec

This repository is the canonical language layer of MOVA.

Contributions are welcome when they improve clarity, correctness, and machine-readability without violating repository boundaries.

## Scope Rules

Allowed here:

- `ds.*` schema design and clarification
- `env.*` envelope design and clarification
- `global.*` catalog evolution
- verbs, tools, actions, and episode semantics
- examples and validation tooling
- documentation that explains language validity

Do not add here:

- package manifests or package assembly canon
- runtime APIs or execution flows
- deployment instructions
- executor-specific orchestration logic
- product-specific domain bundles unless they are only illustrative examples

If a change needs package structure, move it to `mova-contract-spec`.
If a change needs runtime behavior, move it to `mova-agent-api`.

## Source Of Truth Order

1. `schemas/`
2. `global/`
3. `reference/`
4. `docs/concepts/`
5. `examples/`

Documentation must not contradict schemas.
Examples must not contradict schemas or reference guides.

## Required Quality Bar

Every change should keep the repository:

- easy to scan for a developer
- reliable for an LLM agent
- strict about repo boundaries
- stable in identifiers and versioning

## Versioning Rules

- Breaking schema changes require new ids such as `*_v2`.
- Do not silently change the meaning of an existing id.
- Prefer additive clarification over disruptive rewrites.
- If `global.layers_and_namespaces_v2.json` is extended, keep the migration note from `skills` to `contracts` explicit.

## Documentation Rules

- English is the canonical language for spec text and identifiers.
- Prefer short, normative sentences over narrative prose.
- Keep `action_signature` terminology consistent.
- Use exact repo names when describing boundaries.
- Reference `mova-contract-spec` and `mova-agent-api` explicitly when the topic leaves language scope.

## Examples Rules

- Examples in `examples/` should be valid JSON.
- Prefer small examples over broad mixed payloads.
- Minimal examples should validate directly against one schema where practical.
- API-oriented examples should illustrate governance, not runtime implementation.

## Validation

Run before proposing changes:

```bash
npm ci
npm test
```

Use the CLI for targeted checks:

```bash
node bin/mova-validate.mjs \
  --schema https://mova.dev/schemas/ds.instruction_profile_core_v1.schema.json \
  examples/api/instruction_profile.api_governance.example.json
```

## Guidance For Agents

- First classify the request: language, package, or runtime.
- If it is not language-level, do not extend this repo to absorb the missing layer.
- Prefer existing schemas and catalogs over inventing new primitives.
- When generating policy logic, use `action_signature` as the primary key.
- When uncertain, add or improve reference docs before adding more schema surface.
