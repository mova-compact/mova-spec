# INVENTORY_MOVA_REPO_v1

## Summary

This repository is the canonical MOVA language specification.

Current focus:

- version `7.0.0`
- language validity only
- developer-readable and agent-readable reference quality

The repository contains:

- normative JSON Schemas in `schemas/`
- semantic catalogs in `global/`
- reference guides in `reference/`
- concept and evolution docs in `docs/`
- valid examples in `examples/`

It is not a package-canon repo and not a runtime repo.

## Readiness

`5/5` for language-spec repository structure and baseline reference quality.

Current strengths:

- schemas validate
- examples validate
- repo boundaries are explicit
- `action_signature` is documented as the primary operational primitive
- human and LLM entry paths are both present

## Validation

- `npm test` validates all JSON Schemas and curated examples through Ajv 2020-12

## Documentation Map

- Entry point: `README.md`
- Reference guides: `reference/`
- Concept docs: `docs/concepts/`
- Evolution notes: `docs/evolution/`
- Historical material: `docs/archive/`

## Current Structure

- `schemas/` — canonical `ds.*` and `env.*` schemas
- `global/` — canonical `global.*` catalogs
- `reference/` — fast-path authoring and governance guides
- `examples/` — minimal, catalog, envelope, and API-oriented examples
- `bin/` and `tools/` — validation tooling

## Next Useful Steps

1. Add CI that runs `npm test` on pull requests.
2. Expand cross-links from `mova-spec` into `mova-contract-spec` package examples.
3. Add one more machine-focused reference guide for schema extension patterns if the authoring surface grows.
