# Blueshift Pinocchio Escrow

A minimal Solana escrow smart contract built with the [Pinocchio](https://github.com/anza-xyz/pinocchio) framework.  
This repository demonstrates PDA-based escrow creation, SPL-token vault initialization, secure deposits, taker execution, and refunds—all in pure Rust, no Anchor or Node.js required.

---

## Features

- **Make (Init)**: Derive & create an escrow PDA with seed and bump  
- **Vault**: Initialize an Associated Token Account (ATA) as the escrow vault  
- **Take**: Transfer deposited tokens from vault to taker  
- **Refund**: Return tokens from vault to maker  
- **Self-contained**: Zero Anchor macros, zero JS/TS—just Rust & Solana BPF

---

## Prerequisites

- **Rust** (stable toolchain)  
- **Solana CLI** (v1.14+) with BPF toolchain (`cargo build-sbf`)  
- **`pinocchio` crate** (the on-chain framework)  

---

## Repository Layout

.
├── programs/
│ └── blueshift_anchor_escrow/ # On-chain Rust program
│ ├── Cargo.toml
│ └── src/
│ ├── lib.rs # entrypoint & dispatcher
│ ├── state.rs # Escrow account struct
│ └── instructions/ # make, take, refund + helpers
├── tests/ # Optional integration tests
├── Cargo.toml # Workspace-level manifest
└── README.md
