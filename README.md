# Blueshift Anchor Escrow

An Anchor-based Solana escrow program, built as part of the Blueshift Pinocchio Escrow challenge.
This repo demonstrates how to use Anchor’s declarative accounts, PDAs, and CPIs to implement a secure on-chain escrow.

Features
--------
- make: Initialize an escrow PDA and its token vault
- take: Execute a trade—transfer tokens from the vault to taker
- refund: Return tokens from vault back to maker
- Clean Anchor patterns: #[derive(Accounts)], #[account(init)], CPI to SPL token program
- Type-safe IDL: Auto-generated program IDL for client integrations

# Prerequisites
-------------
- Rust (stable toolchain)
- Solana CLI (v1.14+)
- Anchor CLI (v0.27+)

# Repository Layout
----------------
.
├── Anchor.toml                         # Anchor workspace config
├── Cargo.toml                          # Anchor-program manifest
├── programs/
│   └── blueshift_anchor_escrow/        # On-chain Anchor program
│       ├── src/
│       │   ├── lib.rs                  # entrypoint + dispatcher
│       │   ├── state.rs                # Escrow account definition
│       │   └── instructions/           # make.rs, take.rs, refund.rs
│       └── Cargo.toml
├── migrations/                         # Anchor deployment scripts
├── tests/                              # TypeScript + Mocha integration tests
│   └── escrow.ts
├── tsconfig.json                       # Test harness TypeScript config
├── package.json                        # Test dependencies
└── README.md

# Getting Started
---------------
1. Clone & install
   git clone https://github.com/dedeleono/blueshift_anchor_escrow.git
   cd blueshift_anchor_escrow
   yarn install

2. Build & deploy
   # local validator in one terminal
   solana-test-validator --reset

   # in another:
   anchor build
   anchor deploy

   This writes your program .so and keypair into target/deploy/ and updates Anchor.toml.

3. Run tests
   anchor test

# Usage
-----
After deployment, Anchor generates an IDL at target/idl/blueshift_anchor_escrow.json.
You can interact via:

import * as anchor from "@project-serum/anchor";
import { Escrow } from "../target/idl/blueshift_anchor_escrow";

const provider = anchor.AnchorProvider.local();
const program = new anchor.Program<Escrow>(
  require("../target/idl/blueshift_anchor_escrow.json"),
  provider.wallet.publicKey,
  provider
);

// Example: initialize an escrow
await program.methods
  .make(seed, receiveAmt, depositAmt)
  .accounts({
    maker: provider.wallet.publicKey,
    escrow: escrowPda,
    mintA: mintA,
    mintB: mintB,
    makerAtaA: makerAtaA,
    vaultAta: vaultAta,
    systemProgram: anchor.web3.SystemProgram.programId,
    tokenProgram: anchor.utils.token.TOKEN_PROGRAM_ID,
  })
  .rpc();

See tests/escrow.ts for full examples of make, take, and refund.
