# Join requests (single-blob inbox)

New validators **do not** edit `network-allowlist.json`.

## Joiner

1. Fork [fortzADA/tex-network-allowlist](https://github.com/fortzADA/tex-network-allowlist) (or open a PR from a branch).
2. Add **one** file: `join-requests/<your-name>.json` with only your entry:

```json
{
  "id": "<64-hex-ed25519-pubkey>",
  "peer": "<dialable-host:port>",
  "name": "alice-v7"
}
```

3. Open a PR. Do **not** modify `network-allowlist.json` or `core-validators.json` — GitHub Actions **fails** any PR that touches those two files.
4. After an operator admits you, pull the published allowlist and start your node (`tex-allowlist.sh apply --from …`).

Rules:

- `id` — 64 lowercase hex pubkey (never your seed)
- `peer` — dialable `host:port` (Tailscale IP or public IP; not `0.0.0.0`)
- `name` — short label for operators

## Operator

```bash
# From a checked-out request file (downloaded from the PR):
./scripts/tex-allowlist.sh admit-request path/to/alice-v7.json

# Or admit then refresh JSON/static copies:
./scripts/tex-allowlist.sh admit-request path/to/alice-v7.json --publish

# Then push the public allowlist (preserves other join-requests/):
./scripts/tex-allowlist.sh publish-branch
```

Running cluster nodes auto-pull `network-allowlist.json` about once a minute.
