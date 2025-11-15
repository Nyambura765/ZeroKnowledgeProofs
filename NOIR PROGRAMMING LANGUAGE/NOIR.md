## NOIR PROGRAMMING LANGUAGE
Noir is a language for private and verifiable computing. It is a domain-specific programming language used in privacy-preserving 
zero-knowledge systems, requiring no underlying knowledge on mathematics and cryptograhy concepts. 

## Architecture flow
1. ###  Developer layer ( Noir CLI , Noir programming language)
Write Zk circuits using rust-like syntax.
Private inputs by default.
Support for structs, arrays and tuples.
Standardized libraries of optimized functions.

2. ### Compilation Layer (Noir compiler, ACIR generator)
Compile to ACIR intermediate representation.

3. ### Proving Backend (Barrenteng)
Convert ACIR to specific proving systems.

4. ### Proof Generation (prover, witness)
Generate Zero-knowledge proofs.

5. ### Verification layer (solidity verifier, Nargo verify)
Verify proofs on-chain and off-chain.


#### Key Components:

Developer Layer - Where you write ZK circuits using Rust-like syntax, with private inputs by default.
Compilation Layer - Compiles your code to ACIR (Abstract Circuit Intermediate Representation), which is platform-agnostic similar to LLVM.
Proving Backend - Compatible with multiple proving systems including PLONK, Groth16, and Marlin Medium.
Proof Generation - Creates zero-knowledge proofs that can even run in browsers using JavaScript wrappers. 
Verification Layer - Can generate Solidity verifier contracts deployable on EVM-compatible blockchains .

## ZK Architechture

<img width="771" height="596" alt="Image" src="https://github.com/user-attachments/assets/7c98db23-527f-46d3-aa61-6d3555098e5b" />


### Installation



