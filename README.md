# runsten-registry — Official Plugin Registry

Official, cryptographically verified WebAssembly Component Model plugin registry for the **[runsten](https://github.com/AndybSe/runsten)** CLI application framework and runtime kernel.

All plugins conform to the formal Component Model interface `runsten:pipe@1.0.0` (`wit/runsten.wit`) and are distributed as cryptographically signed `.rplg` bundles.

---

## Root Cryptographic Trust Anchor

All official registry index updates and certified plugin bundles are signed using the official Ed25519 root key:

```text
Publisher Verifying Key: ed3561abae592e190f161c2f5848bd5a64514d6f8bef170d9350be4c037ffeb2
```

Clients verify every registry synchronization and bundle installation against this public key before executing bytecode in the capability-sandboxed Wasmtime WASI Preview 2 host.

---

## Synchronizing the Registry

Using the `runsten` CLI:

```bash
# Synchronize user global cache (~/.cache/runsten/registry.json)
runsten registry sync --publisher-key ed3561abae592e190f161c2f5848bd5a64514d6f8bef170d9350be4c037ffeb2 --global

# List available plugins
runsten registry list --global

# Search by capability, keyword, or name
runsten plugin search pipeline
runsten plugin search metrics
```

---

## Installing Plugins

Plugins can be installed by name directly from the synchronized registry:

```bash
runsten plugin install dora
runsten plugin install azdo-lint
runsten plugin install servicenow
runsten plugin install agentforge
```

During installation, `runsten` automatically:
1. Validates the SHA-256 binary digest against the signed registry index.
2. Verifies the publisher's Ed25519 signature on the `.rplg` bundle.
3. Installs the sandboxed component into `~/.local/share/runsten/plugins`.

---

## Certified Plugins Catalog

| Plugin | Version | ABI | Capability Scope | Summary |
|---|---|---|---|---|
| **`azdo-lint`** | `0.1.0` | `runsten:pipe@1` | `feature:pro:azdo` | High-performance declarative YAML linter for Azure DevOps pipelines |
| **`azdo`** | `0.1.0` | `runsten:pipe@1` | `feature:pro:azdo` | Azure DevOps pipeline engineering tool suite (simulate, fix, convert, graph, attest) |
| **`dora`** | `0.1.0` | `runsten:pipe@1` | `feature:enterprise:dora` | DevOps Research and Assessment (DORA) delivery velocity & stability metrics calculator |
| **`servicenow`** | `0.1.0` | `runsten:pipe@1` | `credential:servicenow`, `feature:enterprise:servicenow` | ServiceNow enterprise ITIL bridge (incident triage and change risk assessment) |
| **`agentforge`** | `0.1.0` | `runsten:pipe@1` | `credential:agentforge`, `feature:pro:agentforge` | AgentForge operational tools: doctor, triage, compare, run |
| **`stats`** | `0.1.0` | `runsten:pipe@1` | Core / Community | Token usage aggregation over NDJSON logs (by model, day, total) |
| **`present`** | `0.1.0` | `runsten:pipe@1` | Core / Community | Interactive HTML slide generator from markdown decks |

---

## Security Model

1. **Zero Ambient Authority**: Plugins run strictly sandboxed inside Wasmtime WASI Preview 2 without raw filesystem or network access unless explicitly granted.
2. **Ephemeral Credentials**: Secrets are never stored or decrypted inside plugin memory. The host dispenses short-lived tokens (`rst_ephem_*`) with sub-5-minute TTLs.
3. **Hardware Trust**: Official bundles and indices support signing directly via hardware PIV tokens (YubiKey / HSM slots `9c`, `9a`, `9d`, `9e`) with zero private key RAM exposure.

---

## License

Apache-2.0 © 2026 Andreas Berggren
