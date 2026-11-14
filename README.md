# TExchange network allowlist

Public trusted validator set: Ed25519 pubkeys + gossip `host:port` only. **No seeds or secrets.**

| Path | Purpose | Who writes |
| --- | --- | --- |
| `network-allowlist.json` | Full voting set + `core` bootstrap | **Operators only** |
| `core-validators.json` | Dial targets for remotes | **Operators only** |
| `join-requests/*.json` | Single-validator join blobs | Joiners (via PR) |
| `CODEOWNERS` + CI | Blocks PR edits to the two JSON files | Operators |

## Joiners — submit a single blob

Do **not** edit `network-allowlist.json` or `core-validators.json` (CI will fail the PR). Open a PR that only adds:

`join-requests/<your-name>.json`

```json
{
  "id": "<64-hex-pubkey>",
  "peer": "<dialable-host:port>",
  "name": "alice-v7"
}
```

See [`join-requests/README.md`](join-requests/README.md).

After admission, apply the published allowlist:

```bash
curl -fsSL https://raw.githubusercontent.com/fortzADA/tex-network-allowlist/main/network-allowlist.json | jq .
./scripts/tex-allowlist.sh apply --from https://raw.githubusercontent.com/fortzADA/tex-network-allowlist/main/network-allowlist.json .env
```

## Operators

```bash
./scripts/tex-allowlist.sh admit-request join-requests/alice-v7.json --publish
./scripts/tex-allowlist.sh publish-branch
```

Cluster nodes auto-pull `network-allowlist.json` (~60s).
