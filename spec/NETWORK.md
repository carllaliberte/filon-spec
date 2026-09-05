# Network improvement packs

Locale packs change *place*. Network packs change *how nodes behave on the world network*.

Kind field: `"kind": "network"`.
They do not invent a new agent protocol.

| id | Improves |
|---|---|
| `net-interconnect` | MCP + A2A + OpenAI on one node |
| `net-residency` | jurisdiction before every hop |
| `net-memory` | portable memory envelope |
| `net-edge` | phone-first, $0 control plane |
| `net-identity` | portable agent + operator identity |
| `net-discovery` | personal nodes find peers |
| `net-audit` | tamper-evident action log |
| `net-payments-hook` | x402/AP2 adapter, no new rail |

A node loads **one locale pack + any network packs**.

The *format* and the verbs below are Apache-2.0. Official pack *contents* are not in this repository.

## Executable verbs (v0.2)

### `net-residency`

Before a hop that would send text or memory off-node:

1. Resolve the locale pack.
2. If `destination` is in `deny_regions` → refuse.
3. If `allowed_regions` is not `any` and `destination` is absent → refuse.
4. If the hop carries memory and `memory_may_leave` is false → refuse.

These four checks read locale-pack fields. `may_leave` / `memory_may_leave` are **declared policy**, not a cryptographic guarantee. A node that hops because the boolean is true has followed the pack. It has not proven the hop. Do not treat this as a seal or as a strong security control.

HTTP shape (informative):

```http
POST /hop
{"locale":"fr-CA","destination":"US","carries_memory":false,"text":"Renouvelle ton assurance maladie"}
```

Allowed → `{ "allowed": true, "adapted": "..." }`
Denied → `{ "allowed": false, "reason": "residency_deny" }` with HTTP 403.

### `net-discovery`

Personal nodes find peers. No DHT. No public internet of agents. A control plane lists who announced themselves.

```http
POST /announce
{"name":"nœud-maison","kind":"node","locale":"fr-CA","region":"CA","endpoint":"http://100.x.x.x:3000"}

GET /peers

POST /forget
{"id":"P-0001"}
```

Announcements are volatile unless a later pack adds persistence. A peer older than its TTL is gone.
