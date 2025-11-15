# PRIVATE AND PUBLIC TYPES

A private value is known only to the Prover, while a public value is known by both the Prover and Verifier. Mark values as private when the value should only be known to the prover. All primitive types (including individual fields of compound types) in Noir are private by default, and can be marked public when certain values are intended to be revealed to the Verifier.
Note: For public values defined in Noir programs paired with smart contract verifiers, once the proofs are verified on-chain the values can be considered known to everyone that has access to that blockchain.

Public data types are treated no differently to private types apart from the fact that their values will be revealed in proofs generated. Simply changing the value of a public type will not change the circuit (where the same goes for changing values of private types as well).

Private values are also referred to as witnesses sometimes.

Note: The terms private and public when applied to a type (e.g. pub Field) have a different meaning than when applied to a function (e.g. pub fn foo() {}).

The former is a visibility modifier for the Prover to interpret if a value should be made known to the Verifier, while the latter is a visibility modifier for the compiler to interpret if a function should be made accessible to external Noir programs like in other languages.

### Examples
fn main(x : Field, y : pub Field) -> pub Field {
    x + y
}

In this case x is private and y is public.

### primitive types
