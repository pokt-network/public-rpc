# Pocket Network Public RPC Registry

A discoverable JSON registry of blockchain data sources available through the [Pocket Network](https://pocket.network) public gateway at `https://api.pocket.network`.

The registry file — [`supported-chains.json`](supported-chains.json) — is machine-readable and intended for use by tools, dashboards, and integrations that need to discover which chains are available and how to reach them.

## Schema Validation

The registry is validated against [`supported-chains.schema.json`](supported-chains.schema.json) (JSON Schema draft-07). Any editor or CI tool that supports JSON Schema will flag invalid entries automatically. The `$schema` pointer at the top of `supported-chains.json` enables this out of the box in VS Code and other editors.

## Adding a New Chain

To add a new data source, open a pull request that adds an entry to the `chains` array in `supported-chains.json`. Each entry requires the following fields:

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Human-readable chain name (e.g. `"Ethereum"`, `"Arbitrum Sepolia"`). |
| `slug` | string | URL-safe identifier used in the RPC endpoint path. Must be lowercase alphanumeric with hyphens, matching `^[a-z0-9][a-z0-9-]*$` (e.g. `"eth"`, `"arb-sepolia-testnet"`). |
| `protocol` | string | Protocol family that determines which RPC methods apply. One of: `evm`, `cosmos`, `solana`, `sui`, `near`, `tron`, `radix`. |
| `url` | string | Full RPC endpoint URL, following the pattern `https://<slug>.api.pocket.network`. |
| `network` | string | Either `"mainnet"` or `"testnet"`. |

### Optional Fields

| Field | Type | Description |
|-------|------|-------------|
| `evm_compatible` | boolean | Set to `true` if the chain accepts standard `eth_*` JSON-RPC methods via an EVM compatibility layer despite having a non-EVM native protocol. |
| `status` | string | Operational status: `"active"` (default), `"inactive"` (no active suppliers), or `"degraded"` (probes failing but may recover). |
| `notes` | string | Free-text notes about configuration quirks, status context, or other details. |

### PR Checklist

1. Add your entry to the `chains` array in alphabetical order by `name`.
2. Ensure `url` matches the pattern `https://<slug>.api.pocket.network`.
3. Validate the file against the schema (open it in VS Code or run any JSON Schema validator).
4. Update the `updated_at` date to the current date.
