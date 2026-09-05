# filon-spec

**Apache-2.0.** Locale packs and network-pack *format* for AI agents.

A locale pack gives an agent a portable **place**: language variety, institutions, register, residency. The model underneath may change. The place must not.

This repository is the public contract only:

- `spec/` — format
- `packs/es-MX`, `packs/en-NG`, `packs/fr-CA` — community locales

It is **not** the Filon product runtime, official packs, or reserved names as a brand.

## Why this exists

Cloud agents flatten every user into one generic English/French/Spanish. A pack is data. A runtime has no `if (quebec)`.

## Install a pack

```
packs/<bcp47>/pack.json
```

Required keys: `id`, `name`, `version`, `lexicon`, `register`, `residency`, `tests`.

See [spec/SPEC.md](spec/SPEC.md) and [spec/locale-pack.schema.json](spec/locale-pack.schema.json).

## Runtime verbs (v0.1)

1. `resolve(id)` → pack or 404
2. `adapt(text, id)` → apply `prefer` replacements
3. `may_leave(id, destination)` → boolean

`may_leave` is **declarative pack data**, not a cryptographic guarantee. A `true` means the pack author stated that the destination is allowed. It is not a seal, not a proof, not a strong security control. A future runtime must not treat it as one. See [spec/SPEC.md](spec/SPEC.md).

Network packs (`kind: "network"`) change how a node behaves on the world network. Format: [spec/NETWORK.md](spec/NETWORK.md).

Implemented reference verbs for two packs:

| pack | verb |
|---|---|
| `net-residency` | refuse a hop when the locale forbids the destination |
| `net-discovery` | announce / list / forget personal nodes |

A node loads **one locale pack + any network packs**.

## Add a locale

Copy `packs/fr-CA/`, change `id` (must be BCP 47), fill the lists, open a PR. Three continents on day one is the minimum proof this is not a single-country product.

## What this repo does not contain

Official locale packs, official network pack *contents*, workers, and product folders stay elsewhere and remain all rights reserved unless a file says otherwise.
