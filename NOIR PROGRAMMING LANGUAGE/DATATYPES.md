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

### primitive data types
#### Field
The field type corresponds to the native field tye of the proving backend. They are fundamental blocks to ZK circuits.
The size of a Noir field depends on the elliptic curve's finite field for the proving backend adopted. For example, a field would be a 254-bit integer when paired with the default backend that spans the Grumpkin curve.


#### Example
fn main(x : Field, y : Field)  {
    let z = x + y;
}

x, y and z are all private fields in this example. Using the let keyword we defined a new private value z constrained to be equal to x + y.

If proving efficiency is of priority, fields should be used as a default for solving problems. Smaller integer types (e.g. u64) incur extra range constraints.

#### Integers
An integer type is a range constrained field type.
An integer type is a range constrained field type. The Noir frontend supports both unsigned and signed integer types. The allowed sizes are 1, 8, 16, 32, 64 and 128 bits. (currently only unsigned integers for 128 bits)

#### info
When an integer is defined in Noir without a specific type, it will default to Field unless another type is expected at its position.

The one exception is for loop indices which default to u32 since comparisons on Fields are not possible.

You can add a type suffix such as u32 or Field to the end of an integer literal to explicitly specify the type.