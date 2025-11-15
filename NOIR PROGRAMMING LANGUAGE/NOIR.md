# Noir Programming Language

Noir is a domain-specific language for private and verifiable computing in zero-knowledge systems. It enables developers to write ZK circuits without requiring deep knowledge of mathematics or cryptography concepts.

## Architecture Flow

### 1. Developer Layer
**Components:** Noir CLI, Noir Programming Language

- Write ZK circuits using Rust-like syntax
- Private inputs by default
- Support for structs, arrays, and tuples
- Standardized libraries of optimized functions

### 2. Compilation Layer
**Components:** Noir Compiler, ACIR Generator

- Compiles Noir code to ACIR (Abstract Circuit Intermediate Representation)
- Platform-agnostic intermediate format (similar to LLVM)

### 3. Proving Backend
**Components:** Barretenberg

- Converts ACIR to specific proving systems
- Compatible with multiple proving systems including PLONK, Groth16, and Marlin

### 4. Proof Generation
**Components:** Prover, Witness

- Generates zero-knowledge proofs
- Can run in browsers using JavaScript wrappers

### 5. Verification Layer
**Components:** Solidity Verifier, Nargo Verify

- Verifies proofs on-chain and off-chain
- Generates Solidity verifier contracts deployable on EVM-compatible blockchains

## ZK Architecture

<img width="771" height="596" alt="Noir ZK Architecture Diagram" src="https://github.com/user-attachments/assets/7c98db23-527f-46d3-aa61-6d3555098e5b" />

## Key Features

- **Rust-like Syntax:** Familiar syntax for developers with Rust experience
- **Privacy by Default:** All inputs are private unless explicitly marked public
- **Type Safety:** Strong typing with support for complex data structures
- **Backend Agnostic:** ACIR allows compatibility with multiple proving systems
- **Browser Compatible:** Proofs can be generated client-side
- **Blockchain Integration:** Built-in support for EVM verification contracts

## Installation

[Installation instructions to be added]