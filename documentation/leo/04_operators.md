---
id: operators
title: Leo Operators Reference
sidebar_label: Operators
---

The following lists show the standard and cryptographic operators supported by Leo.
The Leo operators compile down to [Aleo instructions opcodes](../aleo/04_opcodes.md) executable by the Aleo Virtual Machine (AVM).

## Table of Standard Operators
| Name                        | Description                         |
|-----------------------------|:------------------------------------|
| [abs](#abs)                 | Absolute value                      |
| [abs_wrapped](#abs_wrapped) | Wrapping absolute value             |
| [add](#add)                 | Addition                            |
| [add_wrapped](#add_wrapped) | Wrapping addition                   |
| [and](#and)                 | Conjunction                         |
| [assert](#assert)           | Assert boolean true                 |
| [assert_eq](#assert_eq)     | Assert equality                     |
| [assert_neq](#assert_neq)   | Assert non-equality                 |
| [block.height](#block.height)| Fetch the latest block height      |
| [div](#div)                 | Division                            |
| [div_wrapped](#div_wrapped) | Wrapping division operation         |
| [double](#double)           | Double                              |
| [group::GEN](#groupgen)     | group generator                     |
| [gt](#gt)                   | Greater than comparison             |
| [gte](#gte)                 | Greater than or equal to comparison |
| [inv](#inv)                 | Multiplicative inverse              |
| [eq](#eq)                   | Equality comparison                 |
| [neq](#neq)                 | Non-equality comparison             |
| [lt](#lt)                   | Less than comparison                |
| [lte](#lte)                 | Less than or equal to comparison    |
| [mod](#mod)                 | Modulo                              |
| [mul](#mul)                 | Multiplication                      |
| [mul_wrapped](#mul_wrapped) | Wrapping multiplication             |
| [nand](#nand)               | Negated conjunction                 |
| [neg](#neg)                 | Additive inverse                    |
| [nor](#nor)                 | Negated disjunction                 |
| [not](#not)                 | Logical negation                    |
| [or](#or)                   | (Inclusive) disjunction             |
| [pow](#pow)                 | Exponentiation                      |
| [pow_wrapped](#pow_wrapped) | Wrapping exponentiation             |
| [rem](#rem)                 | Remainder                           |
| [rem_wrapped](#rem_wrapped) | Wrapping remainder                  |
| [shl](#shl)                 | Shift left                          |
| [shl_wrapped](#shl_wrapped) | Wrapping shift left                 |
| [shr](#shr)                 | Shift right                         |
| [shr_wrapped](#shr_wrapped) | Wrapping shift right                |
| [square_root](#square_root) | Square root                         |
| [square](#square)           | Square                              |
| [sub](#sub)                 | Subtraction                         |
| [sub_wrapped](#sub_wrapped) | Wrapping subtraction                |
| [ternary](#ternary)         | Ternary select                      |
| [xor](#xor)                 | Exclusive conjunction               |

## Table of Cryptographic Operators
| Name                                                                    | Description                       |
|-------------------------------------------------------------------------|:----------------------------------|
| [ChaCha::rand_destination](#chacharand_destination)                     | ChaCha RNG                        |
| [BHP256::commit_to_destination](#bhp256commit_to_destination)           | 256-bit input BHP commitment      |
| [BHP512::commit_to_destination](#bhp512commit_to_destination)           | 512-bit input BHP commitment      |
| [BHP768::commit_to_destination](#bhp768commit_to_destination)           | 768-bit input BHP commitment      |
| [BHP1024::commit_to_destination](#bhp1024commit_to_destination)         | 1024-bit input BHP commitment     |
| [Pedersen64::commit_to_destination](#pedersen64commit_to_destination)   | 64-bit input Pedersen commitment  |
| [Pedersen128::commit_to_destination](#pedersen128commit_to_destination) | 128-bit input Pedersen commitment |
| [BHP256::hash_to_destination](#bhp256hash_to_destination)               | 256-bit input BHP hash            |
| [BHP512::hash_to_destination](#bhp512hash_to_destination)               | 512-bit input BHP hash            |
| [BHP768::hash_to_destination](#bhp768hash_to_destination)               | 768-bit input BHP hash            |
| [BHP1024::hash_to_destination](#bhp1024hash_to_destination)             | 1024-bit input BHP hash           |
| [Keccak256::hash_to_destination](#keccak256hash_to_destination)         | 256-bit input Keccak hash         |
| [Keccak384::hash_to_destination](#keccak384hash_to_destination)         | 384-bit input Keccak hash         |
| [Keccak512::Hash_to_destination](#keccak512hash_to_destination)         | 512-bit input Keccak hash         |
| [Pedersen64::hash_to_destination](#pedersen64hash_to_destination)       | 64-bit input Pedersen hash        |
| [Pedersen128::hash_to_destination](#pedersen128hash_to_destination)     | 128-bit input Pedersen hash       |
| [Poseidon2::hash_to_destination](#poseidon2hash_to_destination)         | Poseidon hash with input rate 2   |
| [Poseidon4::hash_to_destination](#poseidon4hash_to_destination)         | Poseidon hash with input rate 4   |
| [Poseidon8::hash_to_destination](#poseidon8hash_to_destination)         | Poseidon hash with input rate 8   |
| [SHA3_256::hash_to_destination](#sha3_256hash_to_destination)           | 256-bit input SHA3 hash           |
| [SHA3_384::hash_to_destination](#sha3_384hash_to_destination)           | 384-bit input SHA3 hash           |
| [SHA3_512::hash_to_destination](#sha3_512hash_to_destination)           | 512-bit input SHA3 hash           |
| [signature::verify](#signatureverify)                                   | Verify a signature                |

## Specification

The following is the specification for each operator in the Leo compiler.

### `abs`

```leo
let a: i8 = -1i8;
let b: i8 = a.abs(); // 1i8
```

#### Description

Computes the absolute value of the input, checking for overflow, storing the result in the destination.

Note that execution will halt if the operation overflows/underflows. For cases where wrapping semantics are needed, see the [abs_wrapped](#abs_wrapped) instruction. This underflow happens when the input is the minimum value of a signed integer type. For example, `abs -128i8` would result in underflow, since `128` cannot be represented as an `i8`.

#### Supported Types

| Input  | Destination |
|--------|:------------|
| `i8`   | `i8`        |
| `i16`  | `i16`       |
| `i32`  | `i32`       |
| `i64`  | `i64`       |
| `i128` | `i128`      |

[Back to Top](#table-of-standard-operators)
***

### `abs_wrapped`

```leo
let a: i8 = -128i8;
let b: i8 = a.abs_wrapped(); // -128i8
```

#### Description

Compute the absolute value of the input, wrapping around at the boundary of the type, and storing the result in the destination.

#### Supported Types

| Input  | Destination |
|--------|:------------|
| `i8`   | `i8`        |
| `i16`  | `i16`       |
| `i32`  | `i32`       |
| `i64`  | `i64`       |
| `i128` | `i128`      |

[Back to Top](#table-of-standard-operators)
***

### `add`

```leo
let a: u8 = 1u8;
let b: u8 = a + 1u8; // 2u8
let c: u8 = b.add(1u8); // 3u8
```

#### Description

Adds `first` with `second`, storing the result in `destination`.

Note that execution will halt if the operation overflows. For cases where wrapping semantics are needed for integer types, see the [add_wrapped](#add_wrapped) instruction.

#### Supported Types

| First    | Second   | Destination |
|----------|----------|-------------|
| `field`  | `field`  | `field`     |
| `group`  | `group`  | `group`     |
| `i8`     | `i8`     | `i8`        |
| `i16`    | `i16`    | `i16`       |
| `i32`    | `i32`    | `i32`       |
| `i64`    | `i64`    | `i64`       |
| `i128`   | `i128`   | `i128`      |
| `u8`     | `u8`     | `u8`        |
| `u16`    | `u16`    | `u16`       |
| `u32`    | `u32`    | `u32`       |
| `u64`    | `u64`    | `u64`       |
| `u128`   | `u128`   | `u128`      |
| `scalar` | `scalar` | `scalar`    |

[Back to Top](#table-of-standard-operators)
***

### `add_wrapped`

```leo
let a: u8 = 255u8;
let b: u8 = a.add_wrapped(1u8); // 0u8
```

#### Description

Adds `first` with `second`, wrapping around at the boundary of the type, and storing the result in `destination`.

#### Supported Types

| First  | Second | Destination |
|--------|--------|:------------|
| `i8`   | `i8`   | `i8`        |
| `i16`  | `i16`  | `i16`       |
| `i32`  | `i32`  | `i32`       |
| `i64`  | `i64`  | `i64`       |
| `i128` | `i128` | `i128`      |
| `u8`   | `u8`   | `u8`        |
| `u16`  | `u16`  | `u16`       |
| `u32`  | `u32`  | `u32`       |
| `u64`  | `u64`  | `u64`       |
| `u128` | `u128` | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `and`

```leo
let a: i8 = 1i8 & 1i8; // 1i8
let b: i8 = 1i8.and(2i8); // 0i8
```


#### Description

Performs an AND operation on integer (bitwise) or boolean `first` and `second`,
storing the result in `destination`.

#### Supported Types

| First     | Second   | Destination |
|-----------|----------|:------------|
| `bool`    | `bool`   | `bool`      |
| `i8`      | `i8`     | `i8`        |
| `i16`     | `i16`    | `i16`       |
| `i32`     | `i32`    | `i32`       |
| `i64`     | `i64`    | `i64`       |
| `i128`    | `i128`   | `i128`      |
| `u8`      | `u8`     | `u8`        |
| `u16`     | `u16`    | `u16`       |
| `u32`     | `u32`    | `u32`       |
| `u64`     | `u64`    | `u64`       |
| `u128`    | `u128`   | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `assert`

```leo
let a: bool = true;
let b: bool = false;

assert(a); // will not halt
assert(b); // program halts
```

#### Description

Checks whether the expression evaluates to a `true` boolean value, halting if evaluates to `false`.

#### Supported Types

| Expression |
|------------|
| `bool`  |

[Back to Top](#table-of-standard-operators)
***

### `assert_eq`

```leo
let a: u8 = 1u8;
let b: u8 = 2u8;

assert_eq(a, a); // will not halt
assert_eq(a, b); // program halts
```

#### Description

Checks whether `first` and `second` are equal, halting if they are not equal.

#### Supported Types

| First       | Second      |
|-------------|-------------|
| `address`   | `address`   |
| `bool`   | `bool`   |
| `field`     | `field`     |
| `group`     | `group`     |
| `i8`        | `i8`        |
| `i16`       | `i16`       |
| `i32`       | `i32`       |
| `i64`       | `i64`       |
| `i128`      | `i128`      |
| `u8`        | `u8`        |
| `u16`       | `u16`       |
| `u32`       | `u32`       |
| `u64`       | `u64`       |
| `u128`      | `u128`      |
| `scalar`    | `scalar`    |
| `Signature` | `Signature` |
| `struct`    | `struct`    |
| `Record`    | `Record`    |

[Back to Top](#table-of-standard-operators)
***

### `assert_neq`

```leo
let a: u8 = 1u8;
let b: u8 = 2u8;

assert_neq(a, b); // will not halt
assert_neq(a, a); // program halts
```

#### Description

Checks whether `first` and `second` are not equal, halting if they are equal.

#### Supported Types

| First       | Second      |
|-------------|-------------|
| `address`   | `address`   |
| `bool`   | `bool`   |
| `field`     | `field`     |
| `group`     | `group`     |
| `i8`        | `i8`        |
| `i16`       | `i16`       |
| `i32`       | `i32`       |
| `i64`       | `i64`       |
| `i128`      | `i128`      |
| `u8`        | `u8`        |
| `u16`       | `u16`       |
| `u32`       | `u32`       |
| `u64`       | `u64`       |
| `u128`      | `u128`      |
| `scalar`    | `scalar`    |
| `Signature` | `Signature` |
| `struct`    | `struct`    |
| `Record`    | `Record`    |

[Back to Top](#table-of-standard-operators)
***

### `block.height`

```leo
async transition matches(height: u32) -> Future {
    return check_block_height(height);
} 

async function check_block_height(height: u32) {
    assert_eq(height, block.height);
}
```

#### Description

The `block.height` operator is used to fetch the latest block height in a Leo program. It represents the number of 
blocks in the chain. In the above example, `block.height` is used in an async function to fetch the latest block 
height in a program.

#### Note:
* The `block.height` operator can only be used in an async function. Using it outside an async function will result in a compilation error.
* The `block.height` operator doesn't take any parameters.

[Back to Top](#table-of-standard-operators)
***

### `div`

```leo
let a: u8 = 4u8;
let b: u8 = a / 2u8; // 2u8
let c: u8 = b.div(2u8); // 1u8
```
#### Description

Performs division of the first operand by the second, storing the result in the destination. The operation halts if division by zero is attempted.

For integer types, this operation performs truncated division. Truncated division rounds towards zero, regardless of the sign of the operands. This means it cuts off any digits after the decimal, leaving the whole number whose absolute value is less than or equal to the result.

For example:

- `7 / 3` yields `2`, not `2.3333`.
- `-7 / 3` yields `-2`, not `-2.3333`.

The operation halts if there is an underflow. Underflow occurs when dividing the minimum value of a signed integer type by -1. For example, `-128i8 / -1i8` would result in underflow, since 128 cannot be represented as an `i8`.

For field types, division `a / b` is well-defined for any field values `a` and `b` except when `b = 0field`.

For cases where wrapping semantics are needed for integer types, see the [div_wrapped](#div_wrapped) instruction.

#### Supported Types

| First   | Second  | Destination |
|---------|---------|:------------|
| `field` | `field` | `field`     |
| `i8`    | `i8`    | `i8`        |
| `i16`   | `i16`   | `i16`       |
| `i32`   | `i32`   | `i32`       |
| `i64`   | `i64`   | `i64`       |
| `i128`  | `i128`  | `i128`      |
| `u8`    | `u8`    | `u8`        |
| `u16`   | `u16`   | `u16`       |
| `u32`   | `u32`   | `u32`       |
| `u64`   | `u64`   | `u64`       |
| `u128`  | `u128`  | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `div_wrapped`

```leo
let a: i8 = -128i8;
let b: i8 = a.div_wrapped(-1i8); // -128i8
```

#### Description

Divides `first` by `second`, wrapping around at the boundary of the type, and storing the result in `destination`.

#### Supported Types

| First  | Second | Destination |
|--------|--------|:------------|
| `i8`   | `i8`   | `i8`        |
| `i16`  | `i16`  | `i16`       |
| `i32`  | `i32`  | `i32`       |
| `i64`  | `i64`  | `i64`       |
| `i128` | `i128` | `i128`      |
| `u8`   | `u8`   | `u8`        |
| `u16`  | `u16`  | `u16`       |
| `u32`  | `u32`  | `u32`       |
| `u64`  | `u64`  | `u64`       |
| `u128` | `u128` | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `double`

```leo
let a: group = (0, 4)group;
let b: group = a.double();
```

#### Description

Doubles the input, storing the result in `destination`.


#### Supported Types

| Input   | Destination |
|---------|-------------|
| `field` | `field`     |
| `group` | `group`     |

[Back to Top](#table-of-standard-operators)
***

### `gt`

```leo
let a: bool = 2u8 > 1u8; // true
let b: bool = 1u8.gt(1u8); // false
```

#### Description

Checks if `first` is greater than `second`, storing the result in `destination`.

#### Supported Types

| First    | Second   | Destination |
|----------|----------|-------------|
| `field`  | `field`  | `bool`   |
| `i8`     | `i8`     | `bool`   |
| `i16`    | `i16`    | `bool`   |
| `i32`    | `i32`    | `bool`   |
| `i64`    | `i64`    | `bool`   |
| `i128`   | `i128`   | `bool`   |
| `u8`     | `u8`     | `bool`   |
| `u16`    | `u16`    | `bool`   |
| `u32`    | `u32`    | `bool`   |
| `u64`    | `u64`    | `bool`   |
| `u128`   | `u128`   | `bool`   |
| `scalar` | `scalar` | `bool`   |

[Back to Top](#table-of-standard-operators)
***

### `gte`

```leo
let a: bool = 2u8 >= 1u8; // true
let b: bool = 1u8.gte(1u8); // true
```

#### Description

Checks if `first` is greater than or equal to `second`, storing the result in `destination`.

#### Supported Types

| First    | Second   | Destination |
|----------|----------|-------------|
| `field`  | `field`  | `bool`   |
| `i8`     | `i8`     | `bool`   |
| `i16`    | `i16`    | `bool`   |
| `i32`    | `i32`    | `bool`   |
| `i64`    | `i64`    | `bool`   |
| `i128`   | `i128`   | `bool`   |
| `u8`     | `u8`     | `bool`   |
| `u16`    | `u16`    | `bool`   |
| `u32`    | `u32`    | `bool`   |
| `u64`    | `u64`    | `bool`   |
| `u128`   | `u128`   | `bool`   |
| `scalar` | `scalar` | `bool`   |

[Back to Top](#table-of-standard-operators)
***

### `inv`

```leo
let a: field = 1field.inv();
```

#### Description

Computes the multiplicative inverse of the input, storing the result in `destination`.

#### Supported Types

| Input   | Destination |
|---------|-------------|
| `field` | `field`     |

[Back to Top](#table-of-standard-operators)
***

### `eq`

```leo
let a: bool = 1u8 == 1u8; // true
let b: bool = 1u8.eq(2u8); // false
```

#### Description

Compares `first` and `second` for equality, storing the result in `destination`.

#### Supported Types

| First       | Second      | Destination |
|-------------|-------------|-------------|
| `address`   | `address`   | `bool`   |
| `bool`   | `bool`   | `bool`   |
| `field`     | `field`     | `bool`   |
| `group`     | `group`     | `bool`   |
| `i8`        | `i8`        | `bool`   |
| `i16`       | `i16`       | `bool`   |
| `i32`       | `i32`       | `bool`   |
| `i64`       | `i64`       | `bool`   |
| `i128`      | `i128`      | `bool`   |
| `u8`        | `u8`        | `bool`   |
| `u16`       | `u16`       | `bool`   |
| `u32`       | `u32`       | `bool`   |
| `u64`       | `u64`       | `bool`   |
| `u128`      | `u128`      | `bool`   |
| `scalar`    | `scalar`    | `bool`   |
| `Signature` | `Signature` | `bool`   |
| `struct`    | `struct`    | `bool`   |
| `Record`    | `Record`    | `bool`   |

[Back to Top](#table-of-standard-operators)
***

### `neq`

```leo
let a: bool = 1u8 != 1u8; // false
let b: bool = 1u8.neq(2u8); // true
```

#### Description

Compares `first` and `second` for non-equality, storing the result in `destination`.

#### Supported Types

| First       | Second      | Destination |
|-------------|-------------|-------------|
| `address`   | `address`   | `bool`   |
| `bool`   | `bool`   | `bool`   |
| `field`     | `field`     | `bool`   |
| `group`     | `group`     | `bool`   |
| `i8`        | `i8`        | `bool`   |
| `i16`       | `i16`       | `bool`   |
| `i32`       | `i32`       | `bool`   |
| `i64`       | `i64`       | `bool`   |
| `i128`      | `i128`      | `bool`   |
| `u8`        | `u8`        | `bool`   |
| `u16`       | `u16`       | `bool`   |
| `u32`       | `u32`       | `bool`   |
| `u64`       | `u64`       | `bool`   |
| `u128`      | `u128`      | `bool`   |
| `scalar`    | `scalar`    | `bool`   |
| `Signature` | `Signature` | `bool`   |
| `struct`    | `struct`    | `bool`   |
| `Record`    | `Record`    | `bool`   |

[Back to Top](#table-of-standard-operators)
***

### `lt`

```leo
let a: bool = 1u8 < 2u8; // true
let b: bool = 1u8.lt(1u8); // false
```

#### Description

Checks if `first` is less than `second`, storing the result in `destination`.

#### Supported Types

| First    | Second   | Destination |
|----------|----------|-------------|
| `field`  | `field`  | `bool`   |
| `i8`     | `i8`     | `bool`   |
| `i16`    | `i16`    | `bool`   |
| `i32`    | `i32`    | `bool`   |
| `i64`    | `i64`    | `bool`   |
| `i128`   | `i128`   | `bool`   |
| `u8`     | `u8`     | `bool`   |
| `u16`    | `u16`    | `bool`   |
| `u32`    | `u32`    | `bool`   |
| `u64`    | `u64`    | `bool`   |
| `u128`   | `u128`   | `bool`   |
| `scalar` | `scalar` | `bool`   |

[Back to Top](#table-of-standard-operators)
***

### `lte`

```leo
let a: bool = 1u8 <= 2u8; // true
let b: bool = 1u8.lte(1u8); // true
```

#### Description

Checks if `first` is less than or equal to `second`, storing the result in `destination`.

#### Supported Types

| First    | Second   | Destination |
|----------|----------|-------------|
| `field`  | `field`  | `bool`   |
| `i8`     | `i8`     | `bool`   |
| `i16`    | `i16`    | `bool`   |
| `i32`    | `i32`    | `bool`   |
| `i64`    | `i64`    | `bool`   |
| `i128`   | `i128`   | `bool`   |
| `u8`     | `u8`     | `bool`   |
| `u16`    | `u16`    | `bool`   |
| `u32`    | `u32`    | `bool`   |
| `u64`    | `u64`    | `bool`   |
| `u128`   | `u128`   | `bool`   |
| `scalar` | `scalar` | `bool`   |

[Back to Top](#table-of-standard-operators)
***

### `mod`

```leo
let a: u8 = 3u8.mod(2u8); // 1u8
```

#### Description

Takes the modulo of `first` with respect to `second`, storing the result in `destination`. Halts if `second` is zero.

The semantics of this operation are consistent with the mathematical definition of modulo operation.

#### Supported Types

| First  | Second | Destination |
|--------|--------|-------------|
| `u8`   | `u8`   | `u8`        |
| `u16`  | `u16`  | `u16`       |
| `u32`  | `u32`  | `u32`       |
| `u64`  | `u64`  | `u64`       |
| `u128` | `u128` | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `mul`

```leo
let a: u8 = 2u8 * 2u8; // 4u8
let b: u8 = a.mul(2u8); // 8u8
```

#### Description

Multiplies `first` with `second`, storing the result in `destination`.

Note that execution will halt if the operation overflows/underflows. For cases where wrapping semantics are needed for integer types, see the [mul_wrapped](#mul_wrapped) instruction.

#### Supported Types

| First    | Second   | Destination |
|----------|----------|-------------|
| `field`  | `field`  | `field`     |
| `group`  | `scalar` | `group`     |
| `scalar` | `group`  | `group`     |
| `i8`     | `i8`     | `i8`        |
| `i16`    | `i16`    | `i16`       |
| `i32`    | `i32`    | `i32`       |
| `i64`    | `i64`    | `i64`       |
| `i128`   | `i128`   | `i128`      |
| `u8`     | `u8`     | `u8`        |
| `u16`    | `u16`    | `u16`       |
| `u32`    | `u32`    | `u32`       |
| `u64`    | `u64`    | `u64`       |
| `u128`   | `u128`   | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `mul_wrapped`

```leo
let a: u8 = 128u8.mul_wrapped(2u8); // 0u8
```

#### Description

Multiplies `first` with `second`, wrapping around at the boundary of the type, and storing the result in `destination`.

#### Supported Types

| First  | Second | Destination |
|--------|--------|-------------|
| `i8`   | `i8`   | `i8`        |
| `i16`  | `i16`  | `i16`       |
| `i32`  | `i32`  | `i32`       |
| `i64`  | `i64`  | `i64`       |
| `i128` | `i128` | `i128`      |
| `u8`   | `u8`   | `u8`        |
| `u16`  | `u16`  | `u16`       |
| `u32`  | `u32`  | `u32`       |
| `u64`  | `u64`  | `u64`       |
| `u128` | `u128` | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `nand`

```leo
let a: bool = true.nand(false); // true
```

#### Description

Calculates the negated conjunction of `first` and `second`, storing the result in `destination`. 
The result is false if and only if both first and second are true.

#### Supported Types

| First     | Second    | Destination |
|-----------|-----------|-------------|
| `bool` | `bool` | `bool`   |

[Back to Top](#table-of-standard-operators)
***

### `neg`

```leo
let a: i8 = -1i8.neg(); // 1i8
```

#### Description

Negates the first operand, storing the result in the destination.

For signed integer types, the operation halts if the minimum value is negated. For example, `-128i8.neg()` halts since `128` cannot be represented as an `i8`.


#### Supported Types

| Input   | Destination |
|---------|-------------|
| `field` | `field`     |
| `group` | `group`     |
| `i8`    | `i8`        |
| `i16`   | `i16`       |
| `i32`   | `i32`       |
| `i64`   | `i64`       |
| `i128`  | `i128`      |

[Back to Top](#table-of-standard-operators)
***

### `nor`

```leo
let a: bool = false.nor(false); // true
```

#### Description

Calculates the negated (inclusive) disjunction of `first` and `second`, storing the result in `destination`. The result is `true` if and only if both `first` and `second` are `false`.

#### Supported Type

| First     | Second    | Destination |
|-----------|-----------|-------------|
| `bool` | `bool` | `bool`   |

[Back to Top](#table-of-standard-operators)
***

### `not`

```leo
let a: bool = true.not(); // false
```

#### Description

Perform a NOT operation on an integer (bitwise) or boolean input, storing the result in `destination`.

#### Supported Types

| Input     | Destination |
|-----------|-------------|
| `bool` | `bool`   |
| `i8`      | `i8`        |
| `i16`     | `i16`       |
| `i32`     | `i32`       |
| `i64`     | `i64`       |
| `i128`    | `i128`      |
| `u8`      | `u8`        |
| `u16`     | `u16`       |
| `u32`     | `u32`       |
| `u64`     | `u64`       |
| `u128`    | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### or

```leo
let a: bool = true || false; // true
let b: bool = false.or(false); // false
```

#### Description

Performs an inclusive OR operation on integer (bitwise) or boolean `first` and `second`, storing the result in `destination`.

#### Supported Types

| First     | Second    | Destination |
|-----------|-----------|-------------|
| `bool` | `bool` | `bool`   |
| `i8`      | `i8`      | `i8`        |
| `i16`     | `i16`     | `i16`       |
| `i32`     | `i32`     | `i32`       |
| `i64`     | `i64`     | `i64`       |
| `i128`    | `i128`    | `i128`      |
| `u8`      | `u8`      | `u8`        |
| `u16`     | `u16`     | `u16`       |
| `u32`     | `u32`     | `u32`       |
| `u64`     | `u64`     | `u64`       |
| `u128`    | `u128`    | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `pow`

```leo
let a: u8 = 2u8 ** 2u8; // 4u8
let b: u8 = a.pow(2u8); // 16u8
```

#### Description

Raises `first` to the power of `second`, storing the result in `destination`.

Note that execution will halt if the operation overflows/underflows. For cases where wrapping semantics are needed for integer types, see the [pow_wrapped](#pow_wrapped) instruction.

#### Supported Types

`Magnitude` can be a `u8`, `u16`, or `u32`.

| First   | Second      | Destination |
|---------|-------------|-------------|
| `field` | `field`     | `field`     |
| `i8`    | `Magnitude` | `i8`        |
| `i16`   | `Magnitude` | `i16`       |
| `i32`   | `Magnitude` | `i32`       |
| `i64`   | `Magnitude` | `i64`       |
| `i128`  | `Magnitude` | `i128`      |
| `u8`    | `Magnitude` | `u8`        |
| `u16`   | `Magnitude` | `u16`       |
| `u32`   | `Magnitude` | `u32`       |
| `u64`   | `Magnitude` | `u64`       |
| `u128`  | `Magnitude` | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `pow_wrapped`

```leo
let a: u8 = 16u8.pow_wrapped(2u8); // 0u8
```

#### Description

Raises `first` to the power of `second`, wrapping around at the boundary of the type, storing the result in `destination`.

#### Supported Types

`Magnitude` can be a `u8`, `u16`, or `u32`.

| First  | Second      | Destination |
|--------|-------------|-------------|
| `i8`   | `Magnitude` | `i8`        |
| `i16`  | `Magnitude` | `i16`       |
| `i32`  | `Magnitude` | `i32`       |
| `i64`  | `Magnitude` | `i64`       |
| `i128` | `Magnitude` | `i128`      |
| `u8`   | `Magnitude` | `u8`        |
| `u16`  | `Magnitude` | `u16`       |
| `u32`  | `Magnitude` | `u32`       |
| `u64`  | `Magnitude` | `u64`       |
| `u128` | `Magnitude` | `u128`      |

[Back to Top](#table-of-standard-operators)
***


### `rem`

```leo
let a: u8 = 3u8 % 2u8; // 1u8
let b: u8 = 4u8.rem(2u8); // 0u8
```

#### Description

Computes the truncated remainder of `first` divided by `second`, storing the result in `destination`. Halts on division by zero.


Note that execution will halt if the operation underflows.  This underflow happens when the associated division operation, [div](#div), underflows.

For cases where wrapping semantics are needed for integer types, see the [rem_wrapped](#rem_wrapped) instruction.

#### Supported Types

| First  | Second | Destination |
|--------|--------|-------------|
| `i8`   | `i8`   | `i8`        |
| `i16`  | `i16`  | `i16`       |
| `i32`  | `i32`  | `i32`       |
| `i64`  | `i64`  | `i64`       |
| `i128` | `i128` | `i128`      |
| `u8`   | `u8`   | `u8`        |
| `u16`  | `u16`  | `u16`       |
| `u32`  | `u32`  | `u32`       |
| `u64`  | `u64`  | `u64`       |
| `u128` | `u128` | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `rem_wrapped`

```leo
let a: i8 = -128i8;
let b: i8 = a.rem_wrapped(-1i8); // 0i8
```

#### Description
Computes the truncated remainder of `first` divided by `second`, wrapping around at the boundary of the type, and storing the result in destination.

#### Supported Types

| First  | Second | Destination |
|--------|--------|-------------|
| `i8`   | `i8`   | `i8`        |
| `i16`  | `i16`  | `i16`       |
| `i32`  | `i32`  | `i32`       |
| `i64`  | `i64`  | `i64`       |
| `i128` | `i128` | `i128`      |
| `u8`   | `u8`   | `u8`        |
| `u16`  | `u16`  | `u16`       |
| `u32`  | `u32`  | `u32`       |
| `u64`  | `u64`  | `u64`       |
| `u128` | `u128` | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `signature::verify`

```leo
transition verify_field(s: signature, a: address, v: field) {
    let first: bool = signature::verify(s, a, v);
    let second: bool = s.verify(a, v);
    assert_eq(first, second);
}
```

#### Description

Verifies that the signature `first` was signed by the address `second` with respect to the field `third`, storing the result in `destination`. This verification follows the [Schnorr signature scheme](https://en.wikipedia.org/wiki/Schnorr_signature), which is a digital signature algorithm where the signer generates a random nonce, commits to it, computes a challenge using a hash function, and produces a signature by combining the nonce, challenge, and private key. The verifier checks the validity by reconstructing the challenge and ensuring consistency with the public key and message.

#### Supported Types

A `Message` is any literal or `struct` type.

| First       | Second    | Third     | Destination |
|-------------|-----------|-----------|-------------|
| `signature` | `address` | `Message` | `bool`   |

[Back to Top](#table-of-standard-operators)
***

### `shl`

```leo
let a: u8 = 1u8 << 1u8; // 2u8
let b: u8 = a.shl(1u8); // 4u8
```

#### Description

Shifts `first` left by `second` bits, storing the result in `destination`. The operation halts if the shift distance exceeds the bit size of `first`, or if the shifted result does not fit within the type of `first`.

#### Supported Types

`Magnitude` can be a `u8`, `u16`, or `u32`.

| First  | Second      | Destination |
|--------|-------------|-------------|
| `i8`   | `Magnitude` | `i8`        |
| `i16`  | `Magnitude` | `i16`       |
| `i32`  | `Magnitude` | `i32`       |
| `i64`  | `Magnitude` | `i64`       |
| `i128` | `Magnitude` | `i128`      |
| `u8`   | `Magnitude` | `u8`        |
| `u16`  | `Magnitude` | `u16`       |
| `u32`  | `Magnitude` | `u32`       |
| `u64`  | `Magnitude` | `u64`       |
| `u128` | `Magnitude` | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `shl_wrapped`

```leo
let a: u8 = 128u8.shl_wrapped(1u8); // 0u8
let b: i8 = 64i8.shl_wrapped(2u8); // -128i8
```

#### Description

Shifts `first` left by `second` bits, wrapping around at the boundary of the type, storing the result in `destination`. The shift distance is masked to the bit width of `first`, ensuring that shifting by n is equivalent to shifting by `n % bit_size`. 

If bits are shifted beyond the type's range, they are discarded, which may cause sign changes for signed integers.

#### Supported Types

`Magnitude` can be a `u8`, `u16`, or `u32`.

| First  | Second      | Destination |
|--------|-------------|-------------|
| `i8`   | `Magnitude` | `i8`        |
| `i16`  | `Magnitude` | `i16`       |
| `i32`  | `Magnitude` | `i32`       |
| `i64`  | `Magnitude` | `i64`       |
| `i128` | `Magnitude` | `i128`      |
| `u8`   | `Magnitude` | `u8`        |
| `u16`  | `Magnitude` | `u16`       |
| `u32`  | `Magnitude` | `u32`       |
| `u64`  | `Magnitude` | `u64`       |
| `u128` | `Magnitude` | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `shr`

```leo
let a: u8 = 4u8 >> 1u8; // 2u8
let b: u8 = a.shr(1u8); // 1u8
```

#### Description

Shifts `first` right by `second` bits, storing the result in `destination`. The operation halts if the shift distance exceeds the bit size of `first`.

#### Supported Types

`Magnitude` can be a `u8`, `u16`, or `u32`.

| First  | Second      | Destination |
|--------|-------------|-------------|
| `i8`   | `Magnitude` | `i8`        |
| `i16`  | `Magnitude` | `i16`       |
| `i32`  | `Magnitude` | `i32`       |
| `i64`  | `Magnitude` | `i64`       |
| `i128` | `Magnitude` | `i128`      |
| `u8`   | `Magnitude` | `u8`        |
| `u16`  | `Magnitude` | `u16`       |
| `u32`  | `Magnitude` | `u32`       |
| `u64`  | `Magnitude` | `u64`       |
| `u128` | `Magnitude` | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `shr_wrapped`

```leo
let a: u8 = 128u8.shr_wrapped(7u8); // 1u8
```

#### Description

Shifts `first` right by `second` bits, wrapping around at the boundary of the type, storing the result in `destination`.  The shift distance is masked to the bit width of `first`, ensuring that shifting by `n` is equivalent to shifting by `n % bit_size`.

#### Supported Types

`Magnitude` can be a `u8`, `u16`, or `u32`.

| First  | Second      | Destination |
|--------|-------------|-------------|
| `i8`   | `Magnitude` | `i8`        |
| `i16`  | `Magnitude` | `i16`       |
| `i32`  | `Magnitude` | `i32`       |
| `i64`  | `Magnitude` | `i64`       |
| `i128` | `Magnitude` | `i128`      |
| `u8`   | `Magnitude` | `u8`        |
| `u16`  | `Magnitude` | `u16`       |
| `u32`  | `Magnitude` | `u32`       |
| `u64`  | `Magnitude` | `u64`       |
| `u128` | `Magnitude` | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `square`

```leo
let a: field = 1field.square(); // 1field
```

#### Description

Squares the input, storing the result in `destination`.

#### Supported Types

| Input   | Destination |
|---------|-------------|
| `field` | `field`     |

[Back to Top](#table-of-standard-operators)
***

### `square_root`

```leo
let a: field = 1field.square_root(); // 1field
```

#### Description

Computes the square root of the input, storing the result in `destination`. If the input is a quadratic residue, the function returns the `smaller` of the two possible roots based on modular ordering. If the input is not a quadratic residue, execution halts.

#### Supported Types

| Input   | Destination |
|---------|-------------|
| `field` | `field`     |

[Back to Top](#table-of-standard-operators)
***


### `sub`

```leo
let a: u8 = 2u8 - 1u8; // 1u8
let b: u8 = a.sub(1u8); // 0u8
```

#### Description

Computes `first - second`, storing the result in `destination`. The operation halts if the result is negative in an unsigned type or if it exceeds the minimum representable value in a signed type.

#### Supported Types

| First   | Second  | Destination |
|---------|---------|-------------|
| `field` | `field` | `field`     |
| `group` | `group` | `group`     |
| `i8`    | `i8`    | `i8`        |
| `i16`   | `i16`   | `i16`       |
| `i32`   | `i32`   | `i32`       |
| `i64`   | `i64`   | `i64`       |
| `i128`  | `i128`  | `i128`      |
| `u8`    | `u8`    | `u8`        |
| `u16`   | `u16`   | `u16`       |
| `u32`   | `u32`   | `u32`       |
| `u64`   | `u64`   | `u64`       |
| `u128`  | `u128`  | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `sub_wrapped`

```leo
let a: u8 = 0u8.sub_wrapped(1u8); // 255u8
```

#### Description

Computes `first - second`, wrapping around at the boundary of the type, and storing the result in `destination`.

#### Supported Types

| First  | Second | Destination |
|--------|--------|-------------|
| `i8`   | `i8`   | `i8`        |
| `i16`  | `i16`  | `i16`       |
| `i32`  | `i32`  | `i32`       |
| `i64`  | `i64`  | `i64`       |
| `i128` | `i128` | `i128`      |
| `u8`   | `u8`   | `u8`        |
| `u16`  | `u16`  | `u16`       |
| `u32`  | `u32`  | `u32`       |
| `u64`  | `u64`  | `u64`       |
| `u128` | `u128` | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `ternary`

```leo
let a: u8 = true ? 1u8 : 2u8; // 1u8
```

#### Description

Selects `first`, if `condition` is true, otherwise selects `second`, storing the result in `destination`.

#### Supported Types

| Condition | First       | Second      | Destination |
|-----------|-------------|-------------|-------------|
| `bool` | `bool`   | `bool`   | `bool`   |
| `bool` | `field`     | `field`     | `field`     |
| `bool` | `group`     | `group`     | `group`     |
| `bool` | `i8`        | `i8`        | `i8`        |
| `bool` | `i16`       | `i16`       | `i16`       |
| `bool` | `i32`       | `i32`       | `i32`       |
| `bool` | `i64`       | `i64`       | `i64`       |
| `bool` | `i128`      | `i128`      | `i128`      |
| `bool` | `u8`        | `u8`        | `u8`        |
| `bool` | `u16`       | `u16`       | `u16`       |
| `bool` | `u32`       | `u32`       | `u32`       |
| `bool` | `u64`       | `u64`       | `u64`       |
| `bool` | `u128`      | `u128`      | `u128`      |
| `bool` | `scalar`    | `scalar`    | `scalar`    |
| `bool` | `Signature` | `Signature` | `Signature` |

[Back to Top](#table-of-standard-operators)
***

### `xor`

```leo
let a: bool = true.xor(false); // true
```

#### Description

Performs a XOR operation on integer (bitwise) or boolean `first` and `second`, storing the result in `destination`.

#### Supported Types

| First     | Second    | Destination |
|-----------|-----------|-------------|
| `bool` | `bool` | `bool`   |
| `i8`      | `i8`      | `i8`        |
| `i16`     | `i16`     | `i16`       |
| `i32`     | `i32`     | `i32`       |
| `i64`     | `i64`     | `i64`       |
| `i128`    | `i128`    | `i128`      |
| `u8`      | `u8`      | `u8`        |
| `u16`     | `u16`     | `u16`       |
| `u32`     | `u32`     | `u32`       |
| `u64`     | `u64`     | `u64`       |
| `u128`    | `u128`    | `u128`      |

[Back to Top](#table-of-standard-operators)
***

### `group::GEN`

```leo
let g: group = group::GEN; // the group generator
```

#### Description

Returns the generator of the algebraic group that the `group` type consists of.

The compilation of Leo is based on an elliptic curve, whose points form a group,
and on a specified point on that curve, which generates a subgroup, whose elements form the type `group`.

This is a constant, not a function. Thus, it takes no inputs, and just returns an output.

It is an associated constant, whose name is `GEN` and whose associated type is `group`.

#### Supported Types

| Destination |
|-------------|
| `group`     |

[Back to Top](#table-of-standard-operators)
***

### `ChaCha::rand_DESTINATION`

```leo
let result: address = ChaCha::rand_address();
let result: bool = ChaCha::rand_bool();
let result: field = ChaCha::rand_field();
let result: group = ChaCha::rand_group();
let result: i8 = ChaCha::rand_i8();
let result: i16 = ChaCha::rand_i16();
let result: i32 = ChaCha::rand_i32();
let result: i64 = ChaCha::rand_i64();
let result: i128 = ChaCha::rand_i128();
let result: u8 = ChaCha::rand_u8();
let result: u16 = ChaCha::rand_u16();
let result: u32 = ChaCha::rand_u32();
let result: u64 = ChaCha::rand_u64();
let result: u128 = ChaCha::rand_u128();
let result: scalar = ChaCha::rand_scalar();
```

#### Description

Returns a random value with the destination type.
**This operation can only be used in an async function.**

#### Supported Types

| Destination |
|-------------|
| `address`   |
| `bool`   |
| `field`     |
| `group`     |
| `i8`        |
| `i16`       |
| `i32`       |
| `i64`       |
| `i128`      |
| `u8`        |
| `u16`       |
| `u32`       |
| `u64`       |
| `u128`      |
| `scalar`    |


### `BHP256::commit_to_DESTINATION`

```leo
let salt: scalar = ChaCha::rand_scalar();
let a: address = BHP256::commit_to_address(1u8, salt);
let b: field = BHP256::commit_to_field(2i64, salt);
let c: group = BHP256::commit_to_group(1field, salt);
```

#### Description

Computes a Bowe-Hopwood-Pedersen commitment on inputs of 256-bit chunks in `first`, and some randomness in `second`, storing the commitment in `destination`. Randomness should always be a `scalar` value, and the produced commitment can be an `address`, `field` or, `group` value.

The instruction will halt if the given input is smaller than 129 bits.

#### Supported Types

| First     | Second   | Destination                 |
|-----------|----------|:----------------------------|
| `address` | `scalar` | `address`, `field`, `group` |
| `bool` | `scalar` | `address`, `field`, `group` |
| `field`   | `scalar` | `address`, `field`, `group` |
| `group`   | `scalar` | `address`, `field`, `group` |
| `i8`      | `scalar` | `address`, `field`, `group` |
| `i16`     | `scalar` | `address`, `field`, `group` |
| `i32`     | `scalar` | `address`, `field`, `group` |
| `i64`     | `scalar` | `address`, `field`, `group` |
| `i128`    | `scalar` | `address`, `field`, `group` |
| `u8`      | `scalar` | `address`, `field`, `group` |
| `u16`     | `scalar` | `address`, `field`, `group` |
| `u32`     | `scalar` | `address`, `field`, `group` |
| `u64`     | `scalar` | `address`, `field`, `group` |
| `u128`    | `scalar` | `address`, `field`, `group` |
| `scalar`  | `scalar` | `address`, `field`, `group` |
| `struct`  | `scalar` | `address`, `field`, `group` |

[Back to Top](#table-of-standard-operators)
***

### `BHP512::commit_to_DESTINATION`

```leo
let salt: scalar = ChaCha::rand_scalar();
let a: address = BHP512::commit_to_address(1u8, salt);
let b: field = BHP512::commit_to_field(2i64, salt);
let c: group = BHP512::commit_to_group(1field, salt);
```

#### Description

Computes a Bowe-Hopwood-Pedersen commitment on inputs of 512-bit chunks in `first`, and some randomness in `second`, storing the commitment in `destination`. Randomness should always be a `scalar` value, and the produced commitment will always be a `group` value.

The instruction will halt if the given input is smaller than 171 bits.

#### Supported Types

| First     | Second   | Destination                 |
|-----------|----------|:----------------------------|
| `address` | `scalar` | `address`, `field`, `group` |
| `bool` | `scalar` | `address`, `field`, `group` |
| `field`   | `scalar` | `address`, `field`, `group` |
| `group`   | `scalar` | `address`, `field`, `group` |
| `i8`      | `scalar` | `address`, `field`, `group` |
| `i16`     | `scalar` | `address`, `field`, `group` |
| `i32`     | `scalar` | `address`, `field`, `group` |
| `i64`     | `scalar` | `address`, `field`, `group` |
| `i128`    | `scalar` | `address`, `field`, `group` |
| `u8`      | `scalar` | `address`, `field`, `group` |
| `u16`     | `scalar` | `address`, `field`, `group` |
| `u32`     | `scalar` | `address`, `field`, `group` |
| `u64`     | `scalar` | `address`, `field`, `group` |
| `u128`    | `scalar` | `address`, `field`, `group` |
| `scalar`  | `scalar` | `address`, `field`, `group` |
| `struct`  | `scalar` | `address`, `field`, `group` |

[Back to Top](#table-of-standard-operators)
***

### `BHP768::commit_to_DESTINATION`

```leo
let salt: scalar = ChaCha::rand_scalar();
let a: address = BHP768::commit_to_address(1u8, salt);
let b: field = BHP768::commit_to_field(2i64, salt);
let c: group = BHP768::commit_to_group(1field, salt);
```

#### Description

Computes a Bowe-Hopwood-Pedersen commitment on inputs of 768-bit chunks in `first`, and some randomness in `second`, storing the commitment in `destination`. Randomness should always be a `scalar` value, and the produced commitment will always be a `group` value.

The instruction will halt if the given input is smaller than 129 bits.

#### Supported Types

| First     | Second   | Destination                 |
|-----------|----------|:----------------------------|
| `address` | `scalar` | `address`, `field`, `group` |
| `bool` | `scalar` | `address`, `field`, `group` |
| `field`   | `scalar` | `address`, `field`, `group` |
| `group`   | `scalar` | `address`, `field`, `group` |
| `i8`      | `scalar` | `address`, `field`, `group` |
| `i16`     | `scalar` | `address`, `field`, `group` |
| `i32`     | `scalar` | `address`, `field`, `group` |
| `i64`     | `scalar` | `address`, `field`, `group` |
| `i128`    | `scalar` | `address`, `field`, `group` |
| `u8`      | `scalar` | `address`, `field`, `group` |
| `u16`     | `scalar` | `address`, `field`, `group` |
| `u32`     | `scalar` | `address`, `field`, `group` |
| `u64`     | `scalar` | `address`, `field`, `group` |
| `u128`    | `scalar` | `address`, `field`, `group` |
| `scalar`  | `scalar` | `address`, `field`, `group` |
| `struct`  | `scalar` | `address`, `field`, `group` |

[Back to Top](#table-of-standard-operators)
***

### `BHP1024::commit_to_DESTINATION`

```leo
let salt: scalar = ChaCha::rand_scalar();
let a: address = BHP1024::commit_to_address(1u8, salt);
let b: field = BHP1024::commit_to_field(2i64, salt);
let c: group = BHP1024::commit_to_group(1field, salt);
```

#### Description

Computes a Bowe-Hopwood-Pedersen commitment on inputs of 1024-bit chunks in `first`, and some randomness in `second`, storing the commitment in `destination`. Randomness should always be a `scalar` value, and the produced commitment will always be a `group` value.

The instruction will halt if the given input is smaller than 171 bits.

#### Supported Types

| First     | Second   | Destination                 |
|-----------|----------|:----------------------------|
| `address` | `scalar` | `address`, `field`, `group` |
| `bool` | `scalar` | `address`, `field`, `group` |
| `field`   | `scalar` | `address`, `field`, `group` |
| `group`   | `scalar` | `address`, `field`, `group` |
| `i8`      | `scalar` | `address`, `field`, `group` |
| `i16`     | `scalar` | `address`, `field`, `group` |
| `i32`     | `scalar` | `address`, `field`, `group` |
| `i64`     | `scalar` | `address`, `field`, `group` |
| `i128`    | `scalar` | `address`, `field`, `group` |
| `u8`      | `scalar` | `address`, `field`, `group` |
| `u16`     | `scalar` | `address`, `field`, `group` |
| `u32`     | `scalar` | `address`, `field`, `group` |
| `u64`     | `scalar` | `address`, `field`, `group` |
| `u128`    | `scalar` | `address`, `field`, `group` |
| `scalar`  | `scalar` | `address`, `field`, `group` |
| `struct`  | `scalar` | `address`, `field`, `group` |

[Back to Top](#table-of-standard-operators)
***

### `Pedersen64::commit_to_DESTINATION`

```leo
let salt: scalar = ChaCha::rand_scalar();
let a: address = Pedersen64::commit_to_address(1u8, salt);
let b: field = Pedersen64::commit_to_field(2i64, salt);
let c: group = Pedersen64::commit_to_group(1field, salt);
```

#### Description

Computes a Pedersen commitment up to a 64-bit input in `first`, and some randomness in `second`, storing the commitment in `destination`. Randomness should always be a `scalar` value, and the produced commitment will always be a `group` value.

The instruction will halt if the given `struct` value exceeds the 64-bit limit.

#### Supported Types

| First     | Second   | Destination                 |
|-----------|----------|:----------------------------|
| `bool` | `scalar` | `address`, `field`, `group` |
| `i8`      | `scalar` | `address`, `field`, `group` |
| `i16`     | `scalar` | `address`, `field`, `group` |
| `i32`     | `scalar` | `address`, `field`, `group` |
| `u8`      | `scalar` | `address`, `field`, `group` |
| `u16`     | `scalar` | `address`, `field`, `group` |
| `u32`     | `scalar` | `address`, `field`, `group` |
| `struct`  | `scalar` | `address`, `field`, `group` |

[Back to Top](#table-of-standard-operators)
***

### `Pedersen128::commit_to_DESTINATION`

```leo
let salt: scalar = ChaCha::rand_scalar();
let a: address = Pedersen64::commit_to_address(1u8, salt);
let b: field = Pedersen64::commit_to_field(2i64, salt);
let c: group = Pedersen64::commit_to_group(1field, salt);
```

#### Description

Computes a Pedersen commitment up to a 128-bit input in `first`, and some randomness in `second`, storing the commitment in `destination`. Randomness should always be a `scalar` value, and the produced commitment will always be a `group` value.

The instruction will halt if the given `struct` value exceeds the 128-bit limit.


#### Supported Types

| First     | Second   | Destination                 |
|-----------|----------|:----------------------------|
| `bool` | `scalar` | `address`, `field`, `group` |
| `i8`      | `scalar` | `address`, `field`, `group` |
| `i16`     | `scalar` | `address`, `field`, `group` |
| `i32`     | `scalar` | `address`, `field`, `group` |
| `i64`     | `scalar` | `address`, `field`, `group` |
| `u8`      | `scalar` | `address`, `field`, `group` |
| `u16`     | `scalar` | `address`, `field`, `group` |
| `u32`     | `scalar` | `address`, `field`, `group` |
| `u64`     | `scalar` | `address`, `field`, `group` |
| `struct`  | `scalar` | `address`, `field`, `group` |

[Back to Top](#table-of-standard-operators)
***


### `BHP256::hash_to_DESTINATION`

```leo
let result: address = BHP256::hash_to_address(1u8);
let result: field = BHP256::hash_to_field(2i64);
let result: group = BHP256::hash_to_group(1field);
let result: scalar = BHP256::hash_to_scalar(1field);
let result: i8 = BHP256::hash_to_i8(1field);
let result: i16 = BHP256::hash_to_i16(1field);
let result: i32 = BHP256::hash_to_i32(1field);
let result: i64 = BHP256::hash_to_i64(1field);
let result: i128 = BHP256::hash_to_i128(1field);
let result: u8 = BHP256::hash_to_u8(1field);
let result: u16 = BHP256::hash_to_u16(1field);
let result: u32 = BHP256::hash_to_u32(1field);
let result: u64 = BHP256::hash_to_u64(1field);
let result: u128 = BHP256::hash_to_u128(1field);
```

#### Description

Computes a Bowe-Hopwood-Pedersen hash on inputs of 256-bit chunks in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

The instruction will halt if the given input is smaller than 129 bits.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `address` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `field`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `group`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `scalar`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***

### `BHP512::hash_to_DESTINATION`

```leo
let result: address = BHP512::hash_to_address(1u8);
let result: field = BHP512::hash_to_field(2i64);
let result: group = BHP512::hash_to_group(1field);
let result: scalar = BHP512::hash_to_scalar(1field);
let result: i8 = BHP512::hash_to_i8(1field);
let result: i16 = BHP512::hash_to_i16(1field);
let result: i32 = BHP512::hash_to_i32(1field);
let result: i64 = BHP512::hash_to_i64(1field);
let result: i128 = BHP512::hash_to_i128(1field);
let result: u8 = BHP512::hash_to_u8(1field);
let result: u16 = BHP512::hash_to_u16(1field);
let result: u32 = BHP512::hash_to_u32(1field);
let result: u64 = BHP512::hash_to_u64(1field);
let result: u128 = BHP512::hash_to_u128(1field);
```

#### Description

Computes a Bowe-Hopwood-Pedersen hash on inputs of 512-bit chunks in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

The instruction will halt if the given input is smaller than 171 bits.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `address` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `field`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `group`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `scalar`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***

### `BHP768::hash_to_DESTINATION`

```leo
let result: address = BHP768::hash_to_address(1u8);
let result: field = BHP768::hash_to_field(2i64);
let result: group = BHP768::hash_to_group(1field);
let result: scalar = BHP768::hash_to_scalar(1field);
let result: i8 = BHP768::hash_to_i8(1field);
let result: i16 = BHP768::hash_to_i16(1field);
let result: i32 = BHP768::hash_to_i32(1field);
let result: i64 = BHP768::hash_to_i64(1field);
let result: i128 = BHP768::hash_to_i128(1field);
let result: u8 = BHP768::hash_to_u8(1field);
let result: u16 = BHP768::hash_to_u16(1field);
let result: u32 = BHP768::hash_to_u32(1field);
let result: u64 = BHP768::hash_to_u64(1field);
let result: u128 = BHP768::hash_to_u128(1field);
```

#### Description

Computes a Bowe-Hopwood-Pedersen hash on inputs of 768-bit chunks in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

The instruction will halt if the given input is smaller than 129 bits.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `address` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `field`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `group`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `scalar`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***

### `BHP1024::hash_to_DESTINATION`

```leo
let result: address = BHP1024::hash_to_address(1u8);
let result: field = BHP1024::hash_to_field(2i64);
let result: group = BHP1024::hash_to_group(1field);
let result: scalar = BHP1024::hash_to_scalar(1field);
let result: i8 = BHP1024::hash_to_i8(1field);
let result: i16 = BHP1024::hash_to_i16(1field);
let result: i32 = BHP1024::hash_to_i32(1field);
let result: i64 = BHP1024::hash_to_i64(1field);
let result: i128 = BHP1024::hash_to_i128(1field);
let result: u8 = BHP1024::hash_to_u8(1field);
let result: u16 = BHP1024::hash_to_u16(1field);
let result: u32 = BHP1024::hash_to_u32(1field);
let result: u64 = BHP1024::hash_to_u64(1field);
let result: u128 = BHP1024::hash_to_u128(1field);
```

#### Description

Computes a Bowe-Hopwood-Pedersen hash on inputs of 1024-bit chunks in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

The instruction will halt if the given input is smaller than 171 bits.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `address` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `field`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `group`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `scalar`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***

### `Keccak256::hash_to_DESTINATION`

```leo
let result: address = Keccak256::hash_to_address(1u8);
let result: field = Keccak256::hash_to_field(2i64);
let result: group = Keccak256::hash_to_group(1field);
let result: scalar = Keccak256::hash_to_scalar(1field);
let result: i8 = Keccak256::hash_to_i8(1field);
let result: i16 = Keccak256::hash_to_i16(1field);
let result: i32 = Keccak256::hash_to_i32(1field);
let result: i64 = Keccak256::hash_to_i64(1field);
let result: i128 = Keccak256::hash_to_i128(1field);
let result: u8 = Keccak256::hash_to_u8(1field);
let result: u16 = Keccak256::hash_to_u16(1field);
let result: u32 = Keccak256::hash_to_u32(1field);
let result: u64 = Keccak256::hash_to_u64(1field);
let result: u128 = Keccak256::hash_to_u128(1field);
```

#### Description

Computes a Keccak256 hash on inputs of 256-bit chunks in `first`, storing the hash in `destination`.
The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `address` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `field`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `group`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `scalar`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***

### `Keccak384::hash_to_DESTINATION`

```leo
let result: address = Keccak384::hash_to_address(1u8);
let result: field = Keccak384::hash_to_field(2i64);
let result: group = Keccak384::hash_to_group(1field);
let result: scalar = Keccak384::hash_to_scalar(1field);
let result: i8 = Keccak384::hash_to_i8(1field);
let result: i16 = Keccak384::hash_to_i16(1field);
let result: i32 = Keccak384::hash_to_i32(1field);
let result: i64 = Keccak384::hash_to_i64(1field);
let result: i128 = Keccak384::hash_to_i128(1field);
let result: u8 = Keccak384::hash_to_u8(1field);
let result: u16 = Keccak384::hash_to_u16(1field);
let result: u32 = Keccak384::hash_to_u32(1field);
let result: u64 = Keccak384::hash_to_u64(1field);
let result: u128 = Keccak384::hash_to_u128(1field);
```

#### Description

Computes a Keccak384 hash on inputs of 256-bit chunks in `first`, storing the hash in `destination`.
The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `address` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `field`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `group`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `scalar`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***

### `Keccak512::hash_to_DESTINATION`

```leo
let result: address = Keccak512::hash_to_address(1u8);
let result: field = Keccak512::hash_to_field(2i64);
let result: group = Keccak512::hash_to_group(1field);
let result: scalar = Keccak512::hash_to_scalar(1field);
let result: i8 = Keccak512::hash_to_i8(1field);
let result: i16 = Keccak512::hash_to_i16(1field);
let result: i32 = Keccak512::hash_to_i32(1field);
let result: i64 = Keccak512::hash_to_i64(1field);
let result: i128 = Keccak512::hash_to_i128(1field);
let result: u8 = Keccak512::hash_to_u8(1field);
let result: u16 = Keccak512::hash_to_u16(1field);
let result: u32 = Keccak512::hash_to_u32(1field);
let result: u64 = Keccak512::hash_to_u64(1field);
let result: u128 = Keccak512::hash_to_u128(1field);
```

#### Description

Computes a Keccak512 hash on inputs of 256-bit chunks in `first`, storing the hash in `destination`.
The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `address` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `field`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `group`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `scalar`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***

### `Pedersen64::hash_to_DESTINATION`

```leo
let result: address = Pedersen64::hash_to_address(1u8);
let result: field = Pedersen64::hash_to_field(2i64);
let result: group = Pedersen64::hash_to_group(1field);
let result: scalar = Pedersen64::hash_to_scalar(1field);
let result: i8 = Pedersen64::hash_to_i8(1field);
let result: i16 = Pedersen64::hash_to_i16(1field);
let result: i32 = Pedersen64::hash_to_i32(1field);
let result: i64 = Pedersen64::hash_to_i64(1field);
let result: i128 = Pedersen64::hash_to_i128(1field);
let result: u8 = Pedersen64::hash_to_u8(1field);
let result: u16 = Pedersen64::hash_to_u16(1field);
let result: u32 = Pedersen64::hash_to_u32(1field);
let result: u64 = Pedersen64::hash_to_u64(1field);
let result: u128 = Pedersen64::hash_to_u128(1field);
```

#### Description

Computes a Pedersen hash up to a 64-bit input in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

The instruction will halt if the given `struct` value exceeds the 64-bit limit.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***

### `Pedersen128::hash_to_DESTINATION`

```leo
let result: address = Pedersen128::hash_to_address(1u8);
let result: field = Pedersen128::hash_to_field(2i64);
let result: group = Pedersen128::hash_to_group(1field);
let result: scalar = Pedersen128::hash_to_scalar(1field);
let result: i8 = Pedersen128::hash_to_i8(1field);
let result: i16 = Pedersen128::hash_to_i16(1field);
let result: i32 = Pedersen128::hash_to_i32(1field);
let result: i64 = Pedersen128::hash_to_i64(1field);
let result: i128 = Pedersen128::hash_to_i128(1field);
let result: u8 = Pedersen128::hash_to_u8(1field);
let result: u16 = Pedersen128::hash_to_u16(1field);
let result: u32 = Pedersen128::hash_to_u32(1field);
let result: u64 = Pedersen128::hash_to_u64(1field);
let result: u128 = Pedersen128::hash_to_u128(1field);
```

#### Description

Computes a Pedersen hash up to a 128-bit input in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

The instruction will halt if the given `struct` value exceeds the 64-bit limit.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***


### `Poseidon2::hash_to_DESTINATION`

```leo
let result: address = Poseidon2::hash_to_address(1u8);
let result: field = Poseidon2::hash_to_field(2i64);
let result: group = Poseidon2::hash_to_group(1field);
let result: scalar = Poseidon2::hash_to_scalar(1field);
let result: i8 = Poseidon2::hash_to_i8(1field);
let result: i16 = Poseidon2::hash_to_i16(1field);
let result: i32 = Poseidon2::hash_to_i32(1field);
let result: i64 = Poseidon2::hash_to_i64(1field);
let result: i128 = Poseidon2::hash_to_i128(1field);
let result: u8 = Poseidon2::hash_to_u8(1field);
let result: u16 = Poseidon2::hash_to_u16(1field);
let result: u32 = Poseidon2::hash_to_u32(1field);
let result: u64 = Poseidon2::hash_to_u64(1field);
let result: u128 = Poseidon2::hash_to_u128(1field);
```

#### Description

Calculates a Poseidon hash with an input rate of 2, from an input in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `address` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `field`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `group`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `scalar`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***


### `Poseidon4::hash_to_DESTINATION`

```leo
let result: address = Poseidon4::hash_to_address(1u8);
let result: field = Poseidon4::hash_to_field(2i64);
let result: group = Poseidon4::hash_to_group(1field);
let result: scalar = Poseidon4::hash_to_scalar(1field);
let result: i8 = Poseidon4::hash_to_i8(1field);
let result: i16 = Poseidon4::hash_to_i16(1field);
let result: i32 = Poseidon4::hash_to_i32(1field);
let result: i64 = Poseidon4::hash_to_i64(1field);
let result: i128 = Poseidon4::hash_to_i128(1field);
let result: u8 = Poseidon4::hash_to_u8(1field);
let result: u16 = Poseidon4::hash_to_u16(1field);
let result: u32 = Poseidon4::hash_to_u32(1field);
let result: u64 = Poseidon4::hash_to_u64(1field);
let result: u128 = Poseidon4::hash_to_u128(1field);
```

#### Description

Calculates a Poseidon hash with an input rate of 4, from an input in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `address` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `field`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `group`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `scalar`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***


### `Poseidon8::hash_to_DESTINATION`

```leo
let result: address = Poseidon8::hash_to_address(1u8);
let result: field = Poseidon8::hash_to_field(2i64);
let result: group = Poseidon8::hash_to_group(1field);
let result: scalar = Poseidon8::hash_to_scalar(1field);
let result: i8 = Poseidon8::hash_to_i8(1field);
let result: i16 = Poseidon8::hash_to_i16(1field);
let result: i32 = Poseidon8::hash_to_i32(1field);
let result: i64 = Poseidon8::hash_to_i64(1field);
let result: i128 = Poseidon8::hash_to_i128(1field);
let result: u8 = Poseidon8::hash_to_u8(1field);
let result: u16 = Poseidon8::hash_to_u16(1field);
let result: u32 = Poseidon8::hash_to_u32(1field);
let result: u64 = Poseidon8::hash_to_u64(1field);
let result: u128 = Poseidon8::hash_to_u128(1field);
```

#### Description

Calculates a Poseidon hash with an input rate of 8, from an input in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `address` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `field`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `group`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `scalar`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***

### `SHA3_256::hash_to_DESTINATION`

```leo
let result: address = SHA3_256::hash_to_address(1u8);
let result: field = SHA3_256::hash_to_field(2i64);
let result: group = SHA3_256::hash_to_group(1field);
let result: scalar = SHA3_256::hash_to_scalar(1field);
let result: i8 = SHA3_256::hash_to_i8(1field);
let result: i16 = SHA3_256::hash_to_i16(1field);
let result: i32 = SHA3_256::hash_to_i32(1field);
let result: i64 = SHA3_256::hash_to_i64(1field);
let result: i128 = SHA3_256::hash_to_i128(1field);
let result: u8 = SHA3_256::hash_to_u8(1field);
let result: u16 = SHA3_256::hash_to_u16(1field);
let result: u32 = SHA3_256::hash_to_u32(1field);
let result: u64 = SHA3_256::hash_to_u64(1field);
let result: u128 = SHA3_256::hash_to_u128(1field);
```

#### Description

Calculates a SHA3_256 hash from an input in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `address` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `field`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `group`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `scalar`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***

### `SHA3_384::hash_to_DESTINATION`

```leo
let result: address = SHA3_384::hash_to_address(1u8);
let result: field = SHA3_384::hash_to_field(2i64);
let result: group = SHA3_384::hash_to_group(1field);
let result: scalar = SHA3_384::hash_to_scalar(1field);
let result: i8 = SHA3_384::hash_to_i8(1field);
let result: i16 = SHA3_384::hash_to_i16(1field);
let result: i32 = SHA3_384::hash_to_i32(1field);
let result: i64 = SHA3_384::hash_to_i64(1field);
let result: i128 = SHA3_384::hash_to_i128(1field);
let result: u8 = SHA3_384::hash_to_u8(1field);
let result: u16 = SHA3_384::hash_to_u16(1field);
let result: u32 = SHA3_384::hash_to_u32(1field);
let result: u64 = SHA3_384::hash_to_u64(1field);
let result: u128 = SHA3_384::hash_to_u128(1field);
```

#### Description

Calculates a SHA3_384 hash from an input in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `address` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `field`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `group`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `scalar`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***

### `SHA3_512::hash_to_DESTINATION`

```leo
let result: address = SHA3_512::hash_to_address(1u8);
let result: field = SHA3_512::hash_to_field(2i64);
let result: group = SHA3_512::hash_to_group(1field);
let result: scalar = SHA3_512::hash_to_scalar(1field);
let result: i8 = SHA3_512::hash_to_i8(1field);
let result: i16 = SHA3_512::hash_to_i16(1field);
let result: i32 = SHA3_512::hash_to_i32(1field);
let result: i64 = SHA3_512::hash_to_i64(1field);
let result: i128 = SHA3_512::hash_to_i128(1field);
let result: u8 = SHA3_512::hash_to_u8(1field);
let result: u16 = SHA3_512::hash_to_u16(1field);
let result: u32 = SHA3_512::hash_to_u32(1field);
let result: u64 = SHA3_512::hash_to_u64(1field);
let result: u128 = SHA3_512::hash_to_u128(1field);
```

#### Description

Calculates a SHA3_512 hash from an input in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`u8`, `u16`, `u32`, `u64`, `u128`, `i8`, `i16`, `i32`,`i64`,`i128`, `field`, `group`, or `scalar`) or `address` value, as specified via `hash_to_DESTINATION` at the end of the function.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `address` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `bool` | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `field`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `group`   | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `i128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u8`      | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u16`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u32`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u64`     | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `u128`    | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `scalar`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |
| `struct`  | `address`, `field`, `group`, `scalar`, `i8`, `i16`, `i32`,`i64`,`i128`, `u8`, `u16`, `u32`, `u64`, `u128` |

[Back to Top](#table-of-standard-operators)
***
