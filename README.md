# Astra-vault-contracts
# AstraVault Soroban Contracts

[![Stellar Testnet](https://img.shields.io/badge/Stellar-Testnet-blue)](https://developers.stellar.org)
[![Soroban](https://img.shields.io/badge/Soroban-Enabled-purple)](https://soroban.stellar.org)
[![X-Ray Protocol 25](https://img.shields.io/badge/X--Ray%20ZK-Enabled-8A2BE2)](https://developers.stellar.org/docs/smart-contracts/x-ray)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Soroban smart contracts for **AstraVault** — a privacy-first platform on Stellar for tokenizing real-world assets (RWAs) from the **space economy** (satellite data rights, orbital compute capacity) and **climate finance** (carbon credits, reforestation bonds, green infrastructure).

Built with **Soroban** + **X-Ray Protocol 25** (native BN254 elliptic curve + Poseidon hashing) for efficient on-chain zk-SNARK verification.

## Core Contracts

| Contract          | Purpose                                                                 | Status      |
|-------------------|-------------------------------------------------------------------------|-------------|
| `rwa_token`       | Custom RWA asset issuance, mint/burn, metadata, admin controls          | Planned     |
| `zk_verifier`     | On-chain Groth16 zk-SNARK verification using BN254 host functions       | Planned     |
| `escrow`          | Milestone-based escrow for private trades, conditional releases, yields | Planned     |
| `access_control`  | Role-based access (admin, minter, oracle) for compliance & security     | Planned     |
| `yield_distributor`| Optional community yield / UBI distribution from RWA fees              | Planned     |

## Features & Highlights

- Native Stellar asset compatibility + Soroban token interface
- Privacy via X-Ray: prove compliance / asset validity without revealing details
- Ultra-low fees (~0.00001 XLM/tx) + sub-5-second finality
- Designed for micro-fractional ownership (ideal for emerging markets)
- Oracle-ready (future Chainlink / satellite data verification)

## Quick Start

### Prerequisites

- Rust stable + `wasm32-unknown-unknown` target
- Soroban CLI:  
  ```bash
  cargo install_soroban-cli --features testutils
  ```
- Stellar CLI:  
  ```bash
  cargo install stellar-cli
  ```
- Funded testnet account (use friendbot.stellar.org)

### Build & Test

```bash
# Build all contracts
make build

# Run unit & integration tests
make test

# Or single contract
cargo test --package rwa_token
```

### Deploy to Testnet

```bash
# Set your source account (never commit real secrets!)
export STELLAR_SOURCE=astra-dev

# Deploy example: rwa_token
stellar contract deploy \
  --wasm target/wasm32-unknown-unknown/release/rwa_token.wasm \
  --source $STELLAR_SOURCE \
  --network testnet

# Initialize the contract
stellar contract invoke \
  --id <CONTRACT_ID> \
  -- initialize \
  --admin <YOUR_PUBLIC_KEY> \
  --name "OrbitalBandwidthToken" \
  --symbol "ORBIT" \
  --decimal 7
```

## Folder Structure

```
├── src/
│   ├── rwa_token/          # Main RWA token contract
│   ├── zk_verifier/        # ZK proof verification logic
│   ├── escrow/             # Escrow & conditional release
│   └── access_control/     # Role management (planned)
├── tests/                  # Rust unit & Soroban integration tests
├── scripts/                # Deployment, invoke, & setup helpers
├── docs/                   # Architecture diagrams, ZK explainer, security notes
├── Cargo.toml
└── Makefile                # Common commands (build, test, clean)
```

## Development Guidelines

- Use **conventional commits** (`feat:`, `fix:`, `docs:`, `refactor:`, etc.)
- All PRs require at least 1 review + passing tests
- Contracts must pass basic static analysis (slither / mythril recommended)
- ZK circuits: document proof system, public/private inputs, constraints
- Never commit real secret keys — use `.env` or CLI flags only

## Security Notes

- Contracts are unaudited (early stage) — use at your own risk
- Test thoroughly on testnet before any mainnet interaction
- ZK verifier must be formally verified when complete

## License

MIT — feel free to fork, extend, and contribute.

Questions? Open an issue or reach out on Stellar Discord / X (https://discord.gg/T783n9vZ).
```
