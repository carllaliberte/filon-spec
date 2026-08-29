# Filon Locale Pack spec v0.1

Status: draft. Locale-agnostic. A pack is data. The runtime has no `if (quebec)`.

## Goal

Give any AI agent a portable **place**: language variety, institutions, register, and residency rules. The model underneath may change. The place must not.

## Non-goals

- A new agent wire protocol (use MCP / A2A / OpenAI-compatible).
- Training a foundation model.
- Replacing Unicode CLDR or BCP 47.

## Identifier

`id` MUST be a valid BCP 47 language tag (`es-MX`, `en-NG`, `fr-CA`, `yue-HK`).
Do not invent product names as ids.

## Pack file

Path: `packs/<id>/pack.json`

Required keys: `id`, `name`, `version`, `lexicon`, `register`, `residency`, `tests`.

### lexicon

- `prefer[]`: `{from, to, note}` — rewrite generic wording to local wording.
- `avoid[]`: terms that fail the locale if used as the local name.
- `institutions[]`: `{id, local_name, generic_names[]}`.

### register

`default`: `formal` | `familiar` | `mixed`.
`notes`: free text for authors, not executed.

### residency

- `allowed_regions[]`: ISO 3166-1 alpha-2 or `any`.
- `deny_regions[]`
- `memory_may_leave`: boolean

Runtime: if destination is in `deny_regions`, refuse. If `allowed_regions` is not `any` and destination is absent, refuse.

### tests

Each test: `{id, input, locale, expect}` where `expect` is `contains` | `avoids` | `residency_deny` plus a `value`.

## Runtime verbs

1. `resolve(id)` → pack or 404
2. `adapt(text, id)` → apply `prefer` replacements (case-insensitive)
3. `may_leave(id, destination)` → boolean

No network to a paid model is required for v0.1.

## Adding a pack

Copy any folder under `packs/`, change `id` and the lists, open a PR. Three continents on day one is the minimum visible proof the project is not a single-country product.
