# Noir: Data Types Guide

## Overview

This guide covers the fundamental concepts of private and public types in Noir, as well as the primitive data types available in the language.

---

## Private and Public Types

### Understanding Privacy in Noir

A **private value** is known only to the Prover, while a **public value** is known by both the Prover and Verifier. All primitive types (including individual fields of compound types) in Noir are **private by default**.

**When to use private vs. public:**
- Mark values as **private** when the value should only be known to the prover
- Mark values as **public** when certain values are intended to be revealed to the Verifier

> **Important:** For public values defined in Noir programs paired with smart contract verifiers, once the proofs are verified on-chain the values can be considered known to everyone that has access to that blockchain.

### Key Points

- Public data types are treated no differently to private types apart from the fact that their values will be revealed in proofs generated
- Simply changing the value of a public type will not change the circuit
- Private values are also referred to as **witnesses** sometimes

> **Note:** The terms `private` and `public` when applied to a type (e.g., `pub Field`) have a different meaning than when applied to a function (e.g., `pub fn foo() {}`).
> 
> - **Type modifier:** Visibility for the Prover to interpret if a value should be made known to the Verifier
> - **Function modifier:** Visibility for the compiler to interpret if a function should be accessible to external Noir programs

### Example: Private and Public Parameters

```noir
fn main(x : Field, y : pub Field) -> pub Field {
    x + y
}
```

**In this example:**
- `x` is **private** (only known to the Prover)
- `y` is **public** (known to both Prover and Verifier)
- The return value is **public**

---

## Primitive Data Types

### Field

The `Field` type corresponds to the native field type of the proving backend. Fields are fundamental building blocks of ZK circuits.

#### Characteristics

- The size of a Noir field depends on the elliptic curve's finite field for the proving backend adopted
- With the default backend (Grumpkin curve), a field would be a **254-bit integer**

#### Performance Consideration

> **Best Practice:** If proving efficiency is of priority, fields should be used as a default for solving problems. Smaller integer types (e.g., `u64`) incur extra range constraints.

#### Field Example

```noir
fn main(x : Field, y : Field) {
    let z = x + y;
}
```

**In this example:**
- `x`, `y`, and `z` are all **private fields**
- Using the `let` keyword, we defined a new private value `z` constrained to be equal to `x + y`

---

### Integers

An integer type is a **range constrained field type**. The Noir frontend supports both unsigned and signed integer types.

#### Allowed Sizes

- **1, 8, 16, 32, 64, and 128 bits**
- Currently, only **unsigned integers** are available for 128 bits

#### Supported Integer Types

| Type | Size | Signed/Unsigned | Range |
|------|------|-----------------|-------|
| `u1` | 1 bit | Unsigned | 0 to 1 |
| `u8` | 8 bits | Unsigned | 0 to 255 |
| `u16` | 16 bits | Unsigned | 0 to 65,535 |
| `u32` | 32 bits | Unsigned | 0 to 4,294,967,295 |
| `u64` | 64 bits | Unsigned | 0 to 18,446,744,073,709,551,615 |
| `u128` | 128 bits | Unsigned | 0 to 2^128 - 1 |
| `i8` | 8 bits | Signed | -128 to 127 |
| `i16` | 16 bits | Signed | -32,768 to 32,767 |
| `i32` | 32 bits | Signed | -2,147,483,648 to 2,147,483,647 |
| `i64` | 64 bits | Signed | -2^63 to 2^63 - 1 |

#### Integer Type Inference

> **Important:** When an integer is defined in Noir without a specific type, it will default to `Field` unless another type is expected at its position.
>
> **Exception:** For loop indices default to `u32` since comparisons on Fields are not possible.

#### Type Suffixes

You can add a type suffix to the end of an integer literal to explicitly specify the type:

```noir
fn main() {
    let a = 5u32;      // Explicitly u32
    let b = 10u64;     // Explicitly u64
    let c = 15;        // Defaults to Field
    let d = 20Field;   // Explicitly Field
}
```

#### Integer Examples

```noir
fn main() {
    // Unsigned integers
    let age: u8 = 25;
    let count: u32 = 1000;
    let large_number: u64 = 1_000_000;
    
    // Signed integers
    let temperature: i32 = -15;
    let balance: i64 = -500;
    
    // With type suffixes
    let explicit = 42u32;
    
    // Loop index (defaults to u32)
    for i in 0..10 {
        // i is u32 by default
    }
}
```

# Noir Data Types Reference

A comprehensive guide to data types in the Noir programming language.

## Table of Contents

- [Boolean Type](#boolean-type)
- [Strings](#strings)
  - [Basic Strings](#basic-strings)
  - [Escape Characters](#escape-characters)
  - [Raw Strings](#raw-strings)
  - [Format Strings](#format-strings)
- [Arrays](#arrays)
  - [Array Basics](#array-basics)
  - [Dynamic Indexing](#dynamic-indexing)
  - [Array Methods](#array-methods)
- [Slices](#slices)
  - [Slice Basics](#slice-basics)
  - [Slice Methods](#slice-methods)
- [Tuples](#tuples)
- [Structs](#structs)
- [References](#references)

---

## Boolean Type

The `bool` type in Noir has two possible values: `true` and `false`.

```noir
fn main() {
    let t = true;
    let f: bool = false;
}
```

The boolean type is most commonly used in conditionals like `if` expressions and `assert` statements.

---

## Strings

### Basic Strings

The string type is a fixed length value defined with `str<N>`.

```noir
fn main(message : pub str<11>, hex_as_string : str<4>) {
    println(message);
    assert(message == "hello world");
    assert(hex_as_string == "0x41");
}
```

You can convert a `str<N>` to a byte array by calling `as_bytes()` or a vector by calling `as_bytes_vec()`.

```noir
fn main() {
    let message = "hello world";
    let message_bytes = message.as_bytes();
    let mut message_vec = message.as_bytes_vec();
    assert(message_bytes.len() == 11);
    assert(message_bytes[0] == 104);
    assert(message_bytes[0] == message_vec.get(0));
}
```

### Escape Characters

You can use escape characters for your strings:

| Escape Sequence | Description   |
|----------------|---------------|
| `\r`           | Carriage Return |
| `\n`           | Newline       |
| `\t`           | Tab           |
| `\0`           | Null Character |
| `\"`           | Double Quote  |
| `\\`           | Backslash     |

**Example:**

```noir
let s = "Hello \"world"; // prints "Hello "world"
let s = "hey \tyou"; // prints "hey   you"
```

### Raw Strings

A raw string begins with the letter `r` and is optionally delimited by a number of hashes `#`.

Escape characters are not processed within raw strings. All contents are interpreted literally.

**Example:**

```noir
let s = r"Hello world";
let s = r#"Simon says "hello world""#;

// Any number of hashes may be used (>= 1) as long as the string also terminates with the same number of hashes
let s = r#####"One "#, Two "##, Three "###, Four "####, Five will end the string."#####;
```

### Format Strings

A format string begins with the letter `f` and allows inserting the value of local and global variables in it.

**Example:**

```noir
let four = 2 + 2;
let s = f"Two plus two is: {four}";
println(s);
```

**Output:**
```
Two plus two is: 4
```

To insert the value of a local or global variable, put it inside `{...}` in the string.

If you need to write the `{` or `}` characters, use `{{` and `}}` respectively:

```noir
let four = 2 + 2;

// Prints "This is not expanded: {four}"
println(f"This is not expanded: {{four}}");
```

> **Note:** More complex expressions are not allowed inside `{...}`:
> ```noir
> let s = f"Two plus two is: {2 + 2}" // Error: invalid format string.
> ```

---

## Arrays

### Array Basics

An array is one way of grouping together values into one compound type. Array types can be inferred or explicitly specified via the syntax `[<Type>; <Size>]`:

```noir
fn main(x : u64, y : u64) {
    let my_arr = [x, y];
    let your_arr: [u64; 2] = [x, y];
}
```

**Key Points:**

- All elements in an array must be of the same type (homogeneous)
- Arrays have a fixed size
- Array elements can be accessed using indexing

**Examples:**

```noir
fn main() {
    let a = [1, 2, 3, 4, 5];
    let first = a[0];
    let second = a[1];
}
```

**Mutable Arrays:**

```noir
fn main() {
    let mut arr = [1, 2, 3, 4, 5];
    assert(arr[0] == 1);

    arr[0] = 42;
    assert(arr[0] == 42);
}
```

**Repeated Values:**

```noir
let array: [Field; 32] = [0; 32];
```

**Converting to Slice:**

```noir
let array: [Field; 32] = [0; 32];
let sl = array.as_slice();
```

**Multidimensional Arrays:**

```noir
let array : [[Field; 2]; 2];
let element = array[0][0];
```

> **Note:** Multidimensional slices are not supported.

### Dynamic Indexing

Using constant indices of arrays will often be more efficient at runtime in constrained code. Indexing an array with non-constant indices (indices derived from the inputs to the program, or returned from unconstrained functions) is also called "dynamic indexing" and incurs a slight runtime cost:

```noir
fn main(x: u32) {
    let array = [1, 2, 3, 4];

    // This is a constant index
    let _a = array[double(1)];

    // This is a non-constant index
    let _b = array[double(x)];
}

fn double(y: u32) -> u32 {
    y * 2
}
```

**Restriction:** Dynamic indices cannot be used on arrays with elements which contain a reference type:

```noir
fn main(x: u32) {
    let array = [&mut 1, &mut 2, &mut 3, &mut 4];

    // error! Only constant indices may be used here
    let _c = array[x];
}
```

### Array Methods

#### `len()`

Returns the length of an array.

```noir
fn main() {
    let array = [42, 42];
    assert(array.len() == 2);
}
```

#### `sort()`

Returns a new sorted array. The original array remains untouched. Only works for arrays of fields or integers.

```noir
fn main() {
    let arr = [42, 32];
    let sorted = arr.sort();
    assert(sorted == [32, 42]);
}
```

#### `sort_via(ordering)`

Sorts the array with a custom comparison function. The ordering function must return true if the first argument should be sorted before or is equal to the second argument.

```noir
fn main() {
    let arr = [42, 32];
    let sorted_ascending = arr.sort_via(|a, b| a <= b);
    assert(sorted_ascending == [32, 42]); // verifies

    let sorted_descending = arr.sort_via(|a, b| a >= b);
    assert(sorted_descending == [32, 42]); // does not verify
}
```

#### `map(f)`

Applies a function to each element of the array, returning a new array containing the mapped elements.

```noir
let a = [1, 2, 3];
let b = a.map(|a| a * 2); // b is now [2, 4, 6]
```

#### `mapi(f)`

Applies a function to each element of the array, along with its index, returning a new array containing the mapped elements.

```noir
let a = [1, 2, 3];
let b = a.mapi(|i, a| i + a * 2); // b is now [2, 5, 8]
```

#### `for_each(f)`

Applies a function to each element of the array.

```noir
let a = [1, 2, 3];
a.for_each(|x| {
    println(f"{x}");
});
// prints: 1, 2, 3
```

#### `for_eachi(f)`

Applies a function to each element of the array, along with its index.

```noir
let a = [1, 2, 3];
a.for_eachi(|i, x| {
    println(f"{i}, {x}");
});
// prints: 0, 1 / 1, 2 / 2, 3
```

#### `fold(accumulator, f)`

Applies a function to each element of the array, returning the final accumulated value. This is a left fold.

```noir
fn main() {
    let arr = [2, 2, 2, 2, 2];
    let folded = arr.fold(0, |a, b| a + b);
    assert(folded == 10);
}
```

#### `reduce(f)`

Same as `fold`, but uses the first element as the starting element. Requires the array to be non-empty.

```noir
fn main() {
    let arr = [2, 2, 2, 2, 2];
    let reduced = arr.reduce(|a, b| a + b);
    assert(reduced == 10);
}
```

#### `all(predicate)`

Returns true if all the elements satisfy the given predicate.

```noir
fn main() {
    let arr = [2, 2, 2, 2, 2];
    let all = arr.all(|a| a == 2);
    assert(all);
}
```

#### `any(predicate)`

Returns true if any of the elements satisfy the given predicate.

```noir
fn main() {
    let arr = [2, 2, 2, 2, 5];
    let any = arr.any(|a| a == 5);
    assert(any);
}
```

#### `concat(array2)`

Concatenates this array with another array.

```noir
fn main() {
    let arr1 = [1, 2, 3, 4];
    let arr2 = [6, 7, 8, 9, 10, 11];
    let concatenated_arr = arr1.concat(arr2);
    assert(concatenated_arr == [1, 2, 3, 4, 6, 7, 8, 9, 10, 11]);
}
```

#### `as_str_unchecked()`

Converts a byte array of type `[u8; N]` to a string. Note that this performs no UTF-8 validation.

```noir
fn main() {
    let hi = [104, 105].as_str_unchecked();
    assert_eq(hi, "hi");
}
```

---

## Slices

> **⚠️ Experimental Feature**  
> This feature is experimental and may change in future versions.

A slice is a dynamically-sized view into a sequence of elements. They can be resized at runtime, but because they don't own the data, they cannot be returned from a circuit.

```noir
fn main() -> pub u32 {
    let mut slice: [Field] = &[0; 2];
    let mut new_slice = slice.push_back(6);
    new_slice.len()
}
```

To write a slice literal, use a preceding ampersand: `&[0; 2]` or `&[1, 2, 3]`.

> **Note:** Slices are not references to arrays. In Noir, `&[..]` is more similar to an immutable, growable vector.

### Slice Methods

#### `push_back(elem)`

Pushes a new element to the end of the slice, returning a new slice with a length one greater than the original.

```noir
fn main() -> pub Field {
    let mut slice: [Field] = &[0; 2];
    let mut new_slice = slice.push_back(6);
    new_slice.len()
}
```

#### `push_front(elem)`

Returns a new slice with the specified element inserted at index 0.

```noir
let mut new_slice: [Field] = &[];
new_slice = new_slice.push_front(20);
assert(new_slice[0] == 20);
```

#### `pop_front()`

Returns a tuple of two items: the first element of the slice and the rest of the slice.

```noir
let (first_elem, rest_of_slice) = slice.pop_front();
```

#### `pop_back()`

Returns a tuple of two items: the beginning of the slice with the last element omitted and the last element.

```noir
let (popped_slice, last_elem) = slice.pop_back();
```

#### `append(other)`

Loops over a slice and adds it to the end of another.

```noir
let append = &[1, 2].append(&[3, 4, 5]);
```

#### `insert(index, elem)`

Inserts an element at a specified index and shifts all following elements by 1.

```noir
new_slice = rest_of_slice.insert(2, 100);
assert(new_slice[2] == 100);
```

#### `remove(index)`

Remove an element at a specified index, shifting all elements after it to the left, returning the altered slice and the removed element.

```noir
let (remove_slice, removed_elem) = slice.remove(3);
```

#### `len()`

Returns the length of a slice.

```noir
fn main() {
    let slice = &[42, 42];
    assert(slice.len() == 2);
}
```

#### `as_array()`

Converts this slice into an array. Make sure to specify the size of the resulting array. Panics if the resulting array length is different than the slice's length.

```noir
fn main() {
    let slice = &[5, 6];

    // Always specify the length of the resulting array!
    let array: [Field; 2] = slice.as_array();

    assert(array[0] == slice[0]);
    assert(array[1] == slice[1]);
}
```

#### `map(f)`

Applies a function to each element of the slice, returning a new slice containing the mapped elements.

```noir
let a = &[1, 2, 3];
let b = a.map(|a| a * 2); // b is now &[2, 4, 6]
```

#### `mapi(f)`

Applies a function to each element of the slice, along with its index, returning a new slice containing the mapped elements.

```noir
let a = &[1, 2, 3];
let b = a.mapi(|i, a| i + a * 2); // b is now &[2, 5, 8]
```

#### `for_each(f)`

Applies a function to each element of the slice.

```noir
let a = &[1, 2, 3];
a.for_each(|x| {
    println(f"{x}");
});
// prints: 1, 2, 3
```

#### `for_eachi(f)`

Applies a function to each element of the slice, along with its index.

```noir
let a = &[1, 2, 3];
a.for_eachi(|i, x| {
    println(f"{i}, {x}");
});
// prints: 0, 1 / 1, 2 / 2, 3
```

#### `fold(accumulator, f)`

Applies a function to each element of the slice, returning the final accumulated value. This is a left fold.

```noir
fn main() {
    let slice = &[2, 2, 2, 2, 2];
    let folded = slice.fold(0, |a, b| a + b);
    assert(folded == 10);
}
```

#### `reduce(f)`

Same as `fold`, but uses the first element as the starting element.

```noir
fn main() {
    let slice = &[2, 2, 2, 2, 2];
    let reduced = slice.reduce(|a, b| a + b);
    assert(reduced == 10);
}
```

#### `filter(f)`

Returns a new slice containing only elements for which the given predicate returns true.

```noir
fn main() {
    let slice = &[1, 2, 3, 4, 5];
    let odds = slice.filter(|x| x % 2 == 1);
    assert_eq(odds, &[1, 3, 5]);
}
```

#### `join(separator)`

Flatten each element in the slice into one value, separated by separator.

```noir
struct Accumulator {
    total: Field,
}

impl Append for Accumulator {
    fn empty() -> Self {
        Self { total: 0 }
    }

    fn append(self, other: Self) -> Self {
        Self { total: self.total + other.total }
    }
}

fn main() {
    let slice = &[1, 2, 3, 4, 5].map(|total| Accumulator { total });

    let result = slice.join(Accumulator::empty());
    assert_eq(result, Accumulator { total: 15 });

    let separator = Accumulator { total: 10 };
    let result = slice.join(separator);
    assert_eq(result, Accumulator { total: 55 });
}
```

#### `all(predicate)`

Returns true if all the elements satisfy the given predicate.

```noir
fn main() {
    let slice = &[2, 2, 2, 2, 2];
    let all = slice.all(|a| a == 2);
    assert(all);
}
```

#### `any(predicate)`

Returns true if any of the elements satisfy the given predicate.

```noir
fn main() {
    let slice = &[2, 2, 2, 2, 5];
    let any = slice.any(|a| a == 5);
    assert(any);
}
```

---

## Tuples

A tuple collects multiple values like an array, but with the added ability to collect values of different types:

```noir
fn main() {
    let tup: (u8, u64, Field) = (255, 500, 1000);
}
```

### Accessing Tuple Elements

**Via Destructuring:**

```noir
fn main() {
    let tup = (1, 2);
    let (one, two) = tup;
    let three = one + two;
}
```

**Via Direct Member Access:**

```noir
fn main() {
    let tup = (5, 6, 7, 8);
    let five = tup.0;
    let eight = tup.3;
}
```

---

## Structs

A struct allows for grouping multiple values of different types. Unlike tuples, we can also name each field.

### Defining Structs

```noir
struct Animal {
    hands: Field,
    legs: Field,
    eyes: u8,
}
```

### Creating and Using Structs

```noir
fn main() {
    let legs = 4;

    let dog = Animal {
        eyes: 2,
        hands: 0,
        legs,
    };

    let zero = dog.hands;
}
```

### Destructuring Structs

```noir
fn main() {
    let Animal { hands, legs: feet, eyes } = get_octopus();
    let ten = hands + feet + eyes as Field;
}

fn get_octopus() -> Animal {
    let octopus = Animal {
        hands: 0,
        legs: 8,
        eyes: 2,
    };

    octopus
}
```

### Visibility

By default, structs are private to the module they exist in. You can use `pub` to make the struct public or `pub(crate)` to make it public to just its crate:

```noir
// This struct is now public
pub struct Animal {
    hands: Field,
    legs: Field,
    eyes: u8,
}
```

The same applies to struct fields:

```noir
pub struct Animal {
    hands: Field,           // private to its module
    pub(crate) legs: Field, // accessible from the entire crate
    pub eyes: u8,           // accessible from anywhere
}
```

---

## References

Noir supports first-class references. References are a bit like pointers: they point to a specific address that can be followed to access the data stored at that address.

### Basic Usage

```noir
fn main() {
    let mut x = 2;

    // Reference x as &mut and pass it to multiplyBy2
    multiplyBy2(&mut x);
}

fn multiplyBy2(x: &mut Field) {
    // Dereference it with *
    *x = *x * 2;
}
```

### Limitations

Mutable references to array elements are currently not supported.

**This will error:**

```noir
fn foo(x: &mut u32) {
    *x += 1;
}

fn main() {
    let mut state: [u32; 4] = [1, 2, 3, 4];
    foo(&mut state[0]); // Error!
    assert_eq(state[0], 2);
}
```

**Error message:**
```
error: Mutable references to array elements are currently unsupported
  ┌─ src/main.nr:6:18
  │
6 │         foo(&mut state[0]);
  │                  -------- Try storing the element in a fresh variable first
```

---

#### Range Constraints

Remember that integers are range-constrained fields:
- Each integer type enforces that values stay within the valid range for that type
- Using smaller integer types incurs additional range constraint checks
- This can impact proving efficiency compared to using `Field` directly

---

## Best Practices

### 1. Choose the Right Type

```noir
// ✅ Good: Use Field for efficiency
fn calculate_hash(preimage: Field) -> Field {
    // Hash calculation
}

// ⚠️ Less Efficient: Unnecessary range constraints
fn calculate_hash_slow(preimage: u64) -> u64 {
    // Hash calculation with extra constraints
}
```

### 2. Mark Public Values Explicitly

```noir
fn main(
    secret: Field,           // Private by default
    pub amount: Field        // Explicitly public
) {
    // Only 'amount' will be revealed in the proof
}
```

### 3. Use Appropriate Integer Sizes

```noir
fn main() {
    // ✅ Good: u8 is sufficient for age
    let age: u8 = 25;
    
    // ❌ Overkill: u64 adds unnecessary constraints
    let age_inefficient: u64 = 25;
    
    // ✅ Good: Use Field when range doesn't matter
    let calculation: Field = 12345678901234567890;
}
```

### 4. Understand Visibility Modifiers

```noir
// Type visibility (for Prover/Verifier)
fn main(x: Field, pub y: Field) {
    // x is private, y is public
}

// Function visibility (for external programs)
pub fn helper() {
    // This function can be called from other Noir programs
}
```

---

## Common Patterns

### Private Input, Public Output

```noir
fn main(secret: Field) -> pub Field {
    // Prove knowledge of secret without revealing it
    // But reveal the result
    secret * secret
}
```

### Multiple Public Inputs

```noir
fn main(
    secret: Field,
    pub threshold: Field,
    pub result: Field
) {
    assert(secret > threshold);
    assert(result == secret * 2);
}
```

### Mixed Types

```noir
fn main(
    age: u8,              // Private unsigned 8-bit
    pub min_age: u8,      // Public unsigned 8-bit
    balance: Field        // Private field
) {
    assert(age >= min_age);
    assert(balance > 0);
}
```

---

## Summary

- **All types are private by default** in Noir
- Use `pub` keyword to mark types as public
- **Field** is the most efficient type for ZK circuits
- **Integers** are range-constrained fields with additional overhead
- **Private values** are known only to the Prover (also called witnesses)
- **Public values** are revealed in the proof and known to the Verifier
- Type visibility (`pub Field`) differs from function visibility (`pub fn`)
- Choose types based on your privacy requirements and efficiency needs

## Additional Resources

- [Noir Documentation](https://noir-lang.org)
- [Noir GitHub Repository](https://github.com/noir-lang/noir)

---

*Last updated: 2025*