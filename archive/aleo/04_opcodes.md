---
id: opcodes
title: Aleo Opcodes Reference
sidebar_label: Opcodes
---

The following lists show the standard and cryptographic opcodes supported by Aleo instructions.

## Table of Standard Opcodes
| Name                         | Description                                           |
|------------------------------|:------------------------------------------------------|
| [abs](#abs)                  | Absolute value operation                              |
| [abs.w](#absw)               | Wrapping absolute value operation                     |
| [add](#add)                  | Addition operation                                    |
| [add.w](#addw)               | Wrapping addition operation                           |
| [and](#and)                  | AND operation                                         |
| [assert.eq](#asserteq)       | Assert equality                                       |
| [assert.neq](#assertneq)     | Assert non-equality                                   |
| [block.height](#blockheight) | Returns height of the block within the finalize scope |
| [branch.eq](#brancheq)       | Branches to a position if the arguments are equal     |
| [branch.neq](#branchneq)     | Branches to a position if the arguments are not equal |
| [cast](#cast)                | Cast between literals                                 |
| [cast.lossy](#castlossy)     | Cast between literals with lossy truncation           |
| [div](#div)                  | Division operation                                    |
| [div.w](#divw)               | Wrapping division operation                           |
| [double](#double)            | Double operation                                      |
| [gt](#gt)                    | Greater than comparison                               |
| [gte](#gte)                  | Greater than or equal to comparison                   |
| [inv](#inv)                  | Multiplicative inverse operation                      |
| [is.eq](#iseq)               | Equality comparison                                   |
| [is.neq](#isneq)             | Not equal comparison                                  |
| [lt](#lt)                    | Less than comparison                                  |
| [lte](#lte)                  | Less than or equal to comparison                      |
| [mod](#mod)                  | Arithmetic modulo operation                           |
| [mul](#mul)                  | Multiplication operation                              |
| [mul.w](#mulw)               | Wrapping multiplication operation                     |
| [nand](#nand)                | `Boolean` NAND operation                              |
| [neg](#neg)                  | Additive inverse operation                            |
| [nor](#nor)                  | `Boolean` NOR operation                               |
| [not](#not)                  | NOT operation                                         |
| [or](#or)                    | OR Operation                                          |
| [position](#position)        | The position command                                  |
| [pow](#pow)                  | Exponentiation operation                              |
| [pow.w](#poww)               | Wrapping exponentiation operation                     |
| [rand.chacha](#randchacha)   | Generates a random value within the `finalize` scope. |
| [rem](#rem)                  | Remainder operation                                   |
| [rem.w](#remw)               | Wrapping remainder operation                          |
| [shl](#shl)                  | Shift left operation                                  |
| [shl.w](#shlw)               | Wrapping shift left operation                         |
| [shr](#shr)                  | Shift right operation                                 |
| [shr.w](#shrw)               | Wrapping shift right operation                        |
| [sqrt](#sqrt)                | Square root operation                                 |
| [square](#square)            | Square operation                                      |
| [sub](#sub)                  | Subtraction operation                                 |
| [sub.w](#subw)               | Wrapping subtraction operation                        |
| [ternary](#ternary)          | Ternary select operation                              |
| [xor](#xor)                  | XOR operation                                         |

## Table of Cryptographic Opcodes
| Name                             | Description                       |
|----------------------------------|:----------------------------------|
| [commit.bhp256](#commitbhp256)   | 256-bit input BHP commitment      |
| [commit.bhp512](#commitbhp512)   | 512-bit input BHP commitment      |
| [commit.bhp768](#commitbhp768)   | 768-bit input BHP commitment      |
| [commit.bhp1024](#commitbhp1024) | 1024-bit input BHP commitment     |
| [commit.ped64](#commitped64)     | 64-bit input Pedersen commitment  |
| [commit.ped128](#commitped128)   | 128-bit input Pedersen commitment |
| [hash.bhp256](#hashbhp256)       | 256-bit input BHP hash            |
| [hash.bhp512](#hashbhp512)       | 512-bit input BHP hash            |
| [hash.bhp768](#hashbhp768)       | 768-bit input BHP hash            |
| [hash.bhp1024](#hashbhp1024)     | 1024-bit input BHP hash           |
| [hash.keccak256](#hashkeccak256) | 256-bit input Keccak hash         |
| [hash.keccak384](#hashkeccak384) | 384-bit input Keccak hash         |
| [hash.keccak512](#hashkeccak512) | 512-bit input Keccak hash         |
| [hash.ped64](#hashped64)         | 64-bit input Pedersen hash        |
| [hash.ped128](#hashped128)       | 128-bit input Pedersen hash       |
| [hash.psd2](#hashpsd2)           | Poseidon hash with input rate 2   |
| [hash.psd4](#hashpsd4)           | Poseidon hash with input rate 4   |
| [hash.psd8](#hashpsd8)           | Poseidon hash with input rate 8   |
| [hash.sha3_256](#hashsha3_256)   | 256-bit input SHA3 hash           |
| [hash.sha3_384](#hashsha3_384)   | 384-bit input SHA3 hash           |
| [hash.sha3_512](#hashsha3_512)   | 512-bit input SHA3 hash           |
| [sign.verify](#signverify)       | Verify a Schnorr signature        |

## Specification

The following is the specification for each opcode in the Aleo Virtual Machine (AVM).

### `abs`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes the absolute value of the input, checking for overflow, storing the result in the destination register.

For integer types, a constraint is added to check for underflow. For cases where wrapping semantics are needed, see the [abs.w](#abs.w) instruction. This underflow happens when the input is the minimum value of a signed integer type. For example, `abs -128i8` would result in underflow, since `128` cannot be represented as an `i8`.

#### Supported Types

| Input  | Destination |
|--------|:------------|
| `I8`   | `I8`        |
| `I16`  | `I16`       |
| `I32`  | `I32`       |
| `I64`  | `I64`       |
| `I128` | `I128`      |

***

### `abs.w`

[Back to Top](#table-of-standard-opcodes)

#### Description

Compute the absolute value of the input, wrapping around at the boundary of the type, and storing the result in the destination register.

#### Supported Types

| Input  | Destination |
|--------|:------------|
| `I8`   | `I8`        |
| `I16`  | `I16`       |
| `I32`  | `I32`       |
| `I64`  | `I64`       |
| `I128` | `I128`      |

***

### `add`

[Back to Top](#table-of-standard-opcodes)

#### Description

Adds `first` with `second`, storing the outcome in `destination`.

For integer types, a constraint is added to check for overflow. For cases where wrapping semantics are needed for integer types, see the [add.w](#add.w) instruction.

#### Supported Types

| First    | Second   | Destination |
|----------|----------|-------------|
| `Field`  | `Field`  | `Field`     |
| `Group`  | `Group`  | `Group`     |
| `I8`     | `I8`     | `I8`        |
| `I16`    | `I16`    | `I16`       |
| `I32`    | `I32`    | `I32`       |
| `I64`    | `I64`    | `I64`       |
| `I128`   | `I128`   | `I128`      |
| `U8`     | `U8`     | `U8`        |
| `U16`    | `U16`    | `U16`       |
| `U32`    | `U32`    | `U32`       |
| `U64`    | `U64`    | `U64`       |
| `U128`   | `U128`   | `U128`      |
| `Scalar` | `Scalar` | `Scalar`    |

***

### `add.w`

[Back to Top](#table-of-standard-opcodes)

#### Description

Adds `first` with `second`, wrapping around at the boundary of the type, and storing the outcome in `destination`.

#### Supported Types

| First  | Second | Destination |
|--------|--------|:------------|
| `I8`   | `I8`   | `I8`        |
| `I16`  | `I16`  | `I16`       |
| `I32`  | `I32`  | `I32`       |
| `I64`  | `I64`  | `I64`       |
| `I128` | `I128` | `I128`      |
| `U8`   | `U8`   | `U8`        |
| `U16`  | `U16`  | `U16`       |
| `U32`  | `U32`  | `U32`       |
| `U64`  | `U64`  | `U64`       |
| `U128` | `U128` | `U128`      |

***

### `and`

[Back to Top](#table-of-standard-opcodes)

#### Description

Performs an AND operation on integer (bitwise) or boolean `first` and `second`,
storing the outcome in `destination`.

#### Supported Types

| First     | Second    | Destination |
|-----------|-----------|:------------|
| `Boolean` | `Boolean` | `Boolean`   |
| `I8`      | `I8`      | `I8`        |
| `I16`     | `I16`     | `I16`       |
| `I32`     | `I32`     | `I32`       |
| `I64`     | `I64`     | `I64`       |
| `I128`    | `I128`    | `I128`      |
| `U8`      | `U8`      | `U8`        |
| `U16`     | `U16`     | `U16`       |
| `U32`     | `U32`     | `U32`       |
| `U64`     | `U64`     | `U64`       |
| `U128`    | `U128`    | `U128`      |

***

### `assert.eq`

[Back to Top](#table-of-standard-opcodes)

#### Description

Checks whether `first` and `second` are equal, halting if they are not equal.

#### Supported Types

| First       | Second      |
|-------------|-------------|
| `Address`   | `Address`   |
| `Boolean`   | `Boolean`   |
| `Field`     | `Field`     |
| `Group`     | `Group`     |
| `I8`        | `I8`        |
| `I16`       | `I16`       |
| `I32`       | `I32`       |
| `I64`       | `I64`       |
| `I128`      | `I128`      |
| `U8`        | `U8`        |
| `U16`       | `U16`       |
| `U32`       | `U32`       |
| `U64`       | `U64`       |
| `U128`      | `U128`      |
| `Scalar`    | `Scalar`    |
| `Signature` | `Signature` |
| `Struct`    | `Struct`    |
| `Record`    | `Record`    |

***

### `assert.neq`

[Back to Top](#table-of-standard-opcodes)

#### Description

Checks whether `first` and `second` are not equal, halting if they are equal.

#### Supported Types

| First       | Second      |
|-------------|-------------|
| `Address`   | `Address`   |
| `Boolean`   | `Boolean`   |
| `Field`     | `Field`     |
| `Group`     | `Group`     |
| `I8`        | `I8`        |
| `I16`       | `I16`       |
| `I32`       | `I32`       |
| `I64`       | `I64`       |
| `I128`      | `I128`      |
| `U8`        | `U8`        |
| `U16`       | `U16`       |
| `U32`       | `U32`       |
| `U64`       | `U64`       |
| `U128`      | `U128`      |
| `Scalar`    | `Scalar`    |
| `Signature` | `Signature` |
| `Struct`    | `Struct`    |
| `Record`    | `Record`    |

***

### `block.height`

[Back to Top](#table-of-standard-opcodes)

#### Description

Returns the height of the block within the finalize scope.
Currently, block height is the only supported property.

#### Example Usage

```aleo
assert.eq block.height 1337;
```

***

### `branch.eq`

[Back to Top](#table-of-standard-opcodes)

#### Description

The command `branch.eq <first> <second> to <destination>` branches execution to the [position](#position) indicated by `destination` if `first` and `second` are equal.  This command is restricted to the finalize scope, and the destination must follow the command.  Backward branches are not currently supported.

| First     | Second    | Destination |
|-----------|-----------|-------------|
| `Address` | `Address` | `Position`  |
| `Boolean` | `Boolean` | `Position`  |
| `Field`   | `Field`   | `Position`  |
| `Group`   | `Group`   | `Position`  |
| `I8`      | `I8`      | `Position`  |
| `I16`     | `I16`     | `Position`  |
| `I32`     | `I32`     | `Position`  |
| `I64`     | `I64`     | `Position`  |
| `I128`    | `I128`    | `Position`  |
| `U8`      | `U8`      | `Position`  |
| `U16`     | `U16`     | `Position`  |
| `U32`     | `U32`     | `Position`  |
| `U64`     | `U64`     | `Position`  |
| `U128`    | `U128`    | `Position`  |
| `Scalar`  | `Scalar`  | `Position`  |
| `Struct`  | `Struct`  | `Position`  |
| `Record`  | `Record`  | `Position`  |

***

### `branch.neq`

[Back to Top](#table-of-standard-opcodes)

#### Description

The command `branch.neq <first> <second> to <destination>` branches execution to the [position](#position) indicated by `destination` if `first` and `second` are not equal.  This command is restricted to the finalize scope, and the destination must follow the command.  Backward branches are not currently supported.


| First     | Second    | Destination |
|-----------|-----------|-------------|
| `Address` | `Address` | `Position`  |
| `Boolean` | `Boolean` | `Position`  |
| `Field`   | `Field`   | `Position`  |
| `Group`   | `Group`   | `Position`  |
| `I8`      | `I8`      | `Position`  |
| `I16`     | `I16`     | `Position`  |
| `I32`     | `I32`     | `Position`  |
| `I64`     | `I64`     | `Position`  |
| `I128`    | `I128`    | `Position`  |
| `U8`      | `U8`      | `Position`  |
| `U16`     | `U16`     | `Position`  |
| `U32`     | `U32`     | `Position`  |
| `U64`     | `U64`     | `Position`  |
| `U128`    | `U128`    | `Position`  |
| `Scalar`  | `Scalar`  | `Position`  |
| `Struct`  | `Struct`  | `Position`  |
| `Record`  | `Record`  | `Position`  |

***

### `cast`

[Back to Top](#table-of-standard-opcodes)

#### Description

Enables casting between different literals and can be used to construct structs and records.
In the case of literals, `cast` checks that the source data type can fit into target data type.

#### Example Usage

```aleo
struct foo:
    a as address;
    b as boolean;

function hello:
    input r0 as address.private;
    input r1 as boolean.private;
    input r2 as group.private;
    cast r0 r1 into r3 as foo;
    cast r0 into r4 as group;
    cast r2 into r5 as group.x;
    cast r2 into r6 as group.y;
    cast r2 into r7 as field;
    cast r1 r1 r1 r1 into r7 as [boolean; 4u32];
    cast r7 into r8 as [[boolean; 4u32]; 1u32];
```

### `cast.lossy`

[Back to Top](#table-of-standard-opcodes)

#### Description

Enables casting between different literals.
`cast.lossy` performs a lossy operation when the source data type cannot fit into the target data type.

#### Example Usage

```aleo
input r0 as field.private;
cast r0 into r1 as group;
cast r0 into r2 as u8;
cast.lossy r0 into r3 as u8; // The bottom 8 bits of the r0 are extracted into a u8 and placed into r3
```

### `commit.bhp256`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes a Bowe-Hopwood-Pedersen commitment on inputs of 256-bit chunks in `first`, and some randomness in `second`, storing the commitment in `destination`. Randomness should always be a `Scalar` value, and the produced commitment will always be an `Address`, `Field`, or `Group` value, as specified via `as` at the end of the instruction.

The instruction will halt if the given input is smaller than 129 bits.

#### Supported Types

| First     | Second   | Destination                 |
|-----------|----------|:----------------------------|
| `Address` | `Scalar` | `Address`, `Field`, `Group` |
| `Boolean` | `Scalar` | `Address`, `Field`, `Group` |
| `Field`   | `Scalar` | `Address`, `Field`, `Group` |
| `Group`   | `Scalar` | `Address`, `Field`, `Group` |
| `I8`      | `Scalar` | `Address`, `Field`, `Group` |
| `I16`     | `Scalar` | `Address`, `Field`, `Group` |
| `I32`     | `Scalar` | `Address`, `Field`, `Group` |
| `I64`     | `Scalar` | `Address`, `Field`, `Group` |
| `I128`    | `Scalar` | `Address`, `Field`, `Group` |
| `U8`      | `Scalar` | `Address`, `Field`, `Group` |
| `U16`     | `Scalar` | `Address`, `Field`, `Group` |
| `U32`     | `Scalar` | `Address`, `Field`, `Group` |
| `U64`     | `Scalar` | `Address`, `Field`, `Group` |
| `U128`    | `Scalar` | `Address`, `Field`, `Group` |
| `Scalar`  | `Scalar` | `Address`, `Field`, `Group` |
| `Struct`  | `Scalar` | `Address`, `Field`, `Group` |

***

### `commit.bhp512`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes a Bowe-Hopwood-Pedersen commitment on inputs of 512-bit chunks in `first`, and some randomness in `second`, storing the commitment in `destination`. Randomness should always be a `Scalar` value, and the produced commitment will always be an `Address`, `Field`, or `Group` value, as specified via `as` at the end of the instruction.

The instruction will halt if the given input is smaller than 171 bits.

#### Supported Types

| First     | Second   | Destination                 |
|-----------|----------|:----------------------------|
| `Address` | `Scalar` | `Address`, `Field`, `Group` |
| `Boolean` | `Scalar` | `Address`, `Field`, `Group` |
| `Field`   | `Scalar` | `Address`, `Field`, `Group` |
| `Group`   | `Scalar` | `Address`, `Field`, `Group` |
| `I8`      | `Scalar` | `Address`, `Field`, `Group` |
| `I16`     | `Scalar` | `Address`, `Field`, `Group` |
| `I32`     | `Scalar` | `Address`, `Field`, `Group` |
| `I64`     | `Scalar` | `Address`, `Field`, `Group` |
| `I128`    | `Scalar` | `Address`, `Field`, `Group` |
| `U8`      | `Scalar` | `Address`, `Field`, `Group` |
| `U16`     | `Scalar` | `Address`, `Field`, `Group` |
| `U32`     | `Scalar` | `Address`, `Field`, `Group` |
| `U64`     | `Scalar` | `Address`, `Field`, `Group` |
| `U128`    | `Scalar` | `Address`, `Field`, `Group` |
| `Scalar`  | `Scalar` | `Address`, `Field`, `Group` |
| `Struct`  | `Scalar` | `Address`, `Field`, `Group` |

***

### `commit.bhp768`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes a Bowe-Hopwood-Pedersen commitment on inputs of 768-bit chunks in `first`, and some randomness in `second`, storing the commitment in `destination`. Randomness should always be a `Scalar` value, and the produced commitment will always be an `Address`, `Field`, or `Group` value, as specified via `as` at the end of the instruction.

The instruction will halt if the given input is smaller than 129 bits.

#### Supported Types

| First     | Second   | Destination                 |
|-----------|----------|:----------------------------|
| `Address` | `Scalar` | `Address`, `Field`, `Group` |
| `Boolean` | `Scalar` | `Address`, `Field`, `Group` |
| `Field`   | `Scalar` | `Address`, `Field`, `Group` |
| `Group`   | `Scalar` | `Address`, `Field`, `Group` |
| `I8`      | `Scalar` | `Address`, `Field`, `Group` |
| `I16`     | `Scalar` | `Address`, `Field`, `Group` |
| `I32`     | `Scalar` | `Address`, `Field`, `Group` |
| `I64`     | `Scalar` | `Address`, `Field`, `Group` |
| `I128`    | `Scalar` | `Address`, `Field`, `Group` |
| `U8`      | `Scalar` | `Address`, `Field`, `Group` |
| `U16`     | `Scalar` | `Address`, `Field`, `Group` |
| `U32`     | `Scalar` | `Address`, `Field`, `Group` |
| `U64`     | `Scalar` | `Address`, `Field`, `Group` |
| `U128`    | `Scalar` | `Address`, `Field`, `Group` |
| `Scalar`  | `Scalar` | `Address`, `Field`, `Group` |
| `Struct`  | `Scalar` | `Address`, `Field`, `Group` |

***

### `commit.bhp1024`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes a Bowe-Hopwood-Pedersen commitment on inputs of 1024-bit chunks in `first`, and some randomness in `second`, storing the commitment in `destination`. Randomness should always be a `Scalar` value, and the produced commitment will always be an `Address`, `Field`, or `Group` value, as specified via `as` at the end of the instruction.

The instruction will halt if the given input is smaller than 171 bits.

#### Supported Types

| First     | Second   | Destination                 |
|-----------|----------|:----------------------------|
| `Address` | `Scalar` | `Address`, `Field`, `Group` |
| `Boolean` | `Scalar` | `Address`, `Field`, `Group` |
| `Field`   | `Scalar` | `Address`, `Field`, `Group` |
| `Group`   | `Scalar` | `Address`, `Field`, `Group` |
| `I8`      | `Scalar` | `Address`, `Field`, `Group` |
| `I16`     | `Scalar` | `Address`, `Field`, `Group` |
| `I32`     | `Scalar` | `Address`, `Field`, `Group` |
| `I64`     | `Scalar` | `Address`, `Field`, `Group` |
| `I128`    | `Scalar` | `Address`, `Field`, `Group` |
| `U8`      | `Scalar` | `Address`, `Field`, `Group` |
| `U16`     | `Scalar` | `Address`, `Field`, `Group` |
| `U32`     | `Scalar` | `Address`, `Field`, `Group` |
| `U64`     | `Scalar` | `Address`, `Field`, `Group` |
| `U128`    | `Scalar` | `Address`, `Field`, `Group` |
| `Scalar`  | `Scalar` | `Address`, `Field`, `Group` |
| `Struct`  | `Scalar` | `Address`, `Field`, `Group` |

***

### `commit.ped64`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes a Pedersen commitment up to a 64-bit input in `first`, and some randomness in `second`, storing the commitment in `destination`. Randomness should always be a `Scalar` value, and the produced commitment is an `Address`, `Field`, or `Group` value, as specified via `as` at the end of the instruction.

The instruction will halt if the given `Struct` value exceeds the 64-bit limit.

#### Supported Types

| First     | Second   | Destination                 |
|-----------|----------|:----------------------------|
| `Boolean` | `Scalar` | `Address`, `Field`, `Group` |
| `I8`      | `Scalar` | `Address`, `Field`, `Group` |
| `I16`     | `Scalar` | `Address`, `Field`, `Group` |
| `I32`     | `Scalar` | `Address`, `Field`, `Group` |
| `U8`      | `Scalar` | `Address`, `Field`, `Group` |
| `U16`     | `Scalar` | `Address`, `Field`, `Group` |
| `U32`     | `Scalar` | `Address`, `Field`, `Group` |
| `Struct`  | `Scalar` | `Address`, `Field`, `Group` |

***

### `commit.ped128`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes a Pedersen commitment up to a 128-bit input in `first`, and some randomness in `second`, storing the commitment in `destination`. Randomness should always be a `Scalar` value, and the produced commitment is an `Address`, `Field`, or `Group` value, as specified via `as` at the end of the instruction.

The instruction will halt if the given `Struct` value exceeds the 128-bit limit.

#### Supported Types

| First     | Second   | Destination                 |
|-----------|----------|:----------------------------|
| `Boolean` | `Scalar` | `Address`, `Field`, `Group` |
| `I8`      | `Scalar` | `Address`, `Field`, `Group` |
| `I16`     | `Scalar` | `Address`, `Field`, `Group` |
| `I32`     | `Scalar` | `Address`, `Field`, `Group` |
| `I64`     | `Scalar` | `Address`, `Field`, `Group` |
| `U8`      | `Scalar` | `Address`, `Field`, `Group` |
| `U16`     | `Scalar` | `Address`, `Field`, `Group` |
| `U32`     | `Scalar` | `Address`, `Field`, `Group` |
| `U64`     | `Scalar` | `Address`, `Field`, `Group` |
| `Struct`  | `Scalar` | `Address`, `Field`, `Group` |

***

### `div`

[Back to Top](#table-of-standard-opcodes)

#### Description

Divides `first` by `second`, storing the outcome in `destination`. Halts on division by zero and underflow.

For integer types, this operation performs truncated division. Furthermore, a constraint is added to check for underflow. This underflow happens when dividing the minimum value of a signed integer type by `-1`. For example, `div -128i8 -1i8` would result in underflow, since `128` cannot be represented as an `i8`. 

For cases where wrapping semantics are needed for integer types, see the [div.w](#div.w) instruction.

#### Supported Types

| First   | Second  | Destination |
|---------|---------|:------------|
| `Field` | `Field` | `Field`     |
| `I8`    | `I8`    | `I8`        |
| `I16`   | `I16`   | `I16`       |
| `I32`   | `I32`   | `I32`       |
| `I64`   | `I64`   | `I64`       |
| `I128`  | `I128`  | `I128`      |
| `U8`    | `U8`    | `U8`        |
| `U16`   | `U16`   | `U16`       |
| `U32`   | `U32`   | `U32`       |
| `U64`   | `U64`   | `U64`       |
| `U128`  | `U128`  | `U128`      |

***

### `div.w`

[Back to Top](#table-of-standard-opcodes)

#### Description

Divides `first` by `second`, wrapping around at the boundary of the type, and storing the outcome in `destination`. Halts on division by zero.

#### Supported Types

| First  | Second | Destination |
|--------|--------|:------------|
| `I8`   | `I8`   | `I8`        |
| `I16`  | `I16`  | `I16`       |
| `I32`  | `I32`  | `I32`       |
| `I64`  | `I64`  | `I64`       |
| `I128` | `I128` | `I128`      |
| `U8`   | `U8`   | `U8`        |
| `U16`  | `U16`  | `U16`       |
| `U32`  | `U32`  | `U32`       |
| `U64`  | `U64`  | `U64`       |
| `U128` | `U128` | `U128`      |

***

### `double`

[Back to Top](#table-of-standard-opcodes)

#### Description

Doubles the input, storing the outcome in `destination`.

#### Supported Types

| Input   | Destination |
|---------|-------------|
| `Field` | `Field`     |
| `Group` | `Group`     |

***

### `gt`

[Back to Top](#table-of-standard-opcodes)

#### Description

Checks if `first` is greater than `second`, storing the result in `destination`.

#### Supported Types

| First     | Second    | Destination |
|-----------|-----------|-------------|
| `Field`   | `Field`   | `Boolean`   |
| `I8`      | `I8`      | `Boolean`   |
| `I16`     | `I16`     | `Boolean`   |
| `I32`     | `I32`     | `Boolean`   |
| `I64`     | `I64`     | `Boolean`   |
| `I128`    | `I128`    | `Boolean`   |
| `U8`      | `U8`      | `Boolean`   |
| `U16`     | `U16`     | `Boolean`   |
| `U32`     | `U32`     | `Boolean`   |
| `U64`     | `U64`     | `Boolean`   |
| `U128`    | `U128`    | `Boolean`   |
| `Scalar`  | `Scalar`  | `Boolean`   |

***

### `gte`

[Back to Top](#table-of-standard-opcodes)

#### Description

Checks if `first` is greater than or equal to `second`, storing the result in `destination`.

#### Supported Types

| First    | Second   | Destination |
|----------|----------|-------------|
| `Field`  | `Field`  | `Boolean`   |
| `I8`     | `I8`     | `Boolean`   |
| `I16`    | `I16`    | `Boolean`   |
| `I32`    | `I32`    | `Boolean`   |
| `I64`    | `I64`    | `Boolean`   |
| `I128`   | `I128`   | `Boolean`   |
| `U8`     | `U8`     | `Boolean`   |
| `U16`    | `U16`    | `Boolean`   |
| `U32`    | `U32`    | `Boolean`   |
| `U64`    | `U64`    | `Boolean`   |
| `U128`   | `U128`   | `Boolean`   |
| `Scalar` | `Scalar` | `Boolean`   |

***

### `hash.bhp256`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes a Bowe-Hopwood-Pedersen hash on inputs of 256-bit chunks in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

The instruction will halt if the given input is smaller than 129 bits.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Address` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Field`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Group`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Scalar`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `hash.bhp512`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes a Bowe-Hopwood-Pedersen hash on inputs of 512-bit chunks in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

The instruction will halt if the given input is smaller than 171 bits.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Address` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Field`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Group`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Scalar`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `hash.bhp768`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes a Bowe-Hopwood-Pedersen hash on inputs of 768-bit chunks in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

The instruction will halt if the given input is smaller than 129 bits.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Address` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Field`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Group`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Scalar`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `hash.bhp1024`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes a Bowe-Hopwood-Pedersen hash on inputs of 1024-bit chunks in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

The instruction will halt if the given input is smaller than 171 bits.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Address` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Field`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Group`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Scalar`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `hash.keccak256`

[Back to Top](#table-of-standard-opcodes)

#### Description

Performs a Keccak hash on `first`, storing a 256-bit digest in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Address` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Field`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Group`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Scalar`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `hash.keccak384`

[Back to Top](#table-of-standard-opcodes)

#### Description

Performs a Keccak hash on `first`, storing a 384-bit digest in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Address` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Field`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Group`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Scalar`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `hash.keccak512`

[Back to Top](#table-of-standard-opcodes)

#### Description

Performs a Keccak hash on `first`, storing a 512-bit digest in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Address` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Field`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Group`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Scalar`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `hash.ped64`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes a Pedersen hash up to a 64-bit input in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

The instruction will halt if the given `Struct` value exceeds the 64-bit limit.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `hash.ped128`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes a Pedersen hash up to a 128-bit input in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

The instruction will halt if the given `Struct` value exceeds the 128-bit limit.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `hash.psd2`

[Back to Top](#table-of-standard-opcodes)

#### Description

Calculates a Poseidon hash with an input rate of 2, from an input in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Address` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Field`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Group`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Scalar`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `hash.psd4`

[Back to Top](#table-of-standard-opcodes)

#### Description

Calculates a Poseidon hash with an input rate of 4, from an input in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Address` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Field`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Group`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Scalar`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `hash.psd8`

[Back to Top](#table-of-standard-opcodes)

#### Description

Calculates a Poseidon hash with an input rate of 8, from an input in `first`, storing the hash in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Address` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Field`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Group`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Scalar`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `hash.sha3_256`

[Back to Top](#table-of-standard-opcodes)

#### Description

Calculates a SHA3-256 hash, from an input in `first`, storing the 256-bit digest in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Address` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Field`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Group`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Scalar`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `hash.sha3_384`

[Back to Top](#table-of-standard-opcodes)

#### Description

Calculates a SHA3-384 hash, from an input in `first`, storing the 384-bit digest in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

#### Supported Types
| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Address` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Field`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Group`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Scalar`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `hash.sha3_512`

[Back to Top](#table-of-standard-opcodes)

#### Description

Calculates a SHA3-512 hash, from an input in `first`, storing the 512-bit digest in `destination`. The produced hash will always be an arithmetic (`U8`, `U16`, `U32`, `U64`, `U128`, `I8`, `I16`, `I32`,`I64`,`I128`, `Field`, `Group`, or `Scalar`) or `Address` value, as specified via `as` at the end of the instruction.

#### Supported Types

| First     | Destination                                                                                               |
|-----------|:----------------------------------------------------------------------------------------------------------|
| `Address` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Boolean` | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Field`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Group`   | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `I128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U8`      | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U16`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U32`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U64`     | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `U128`    | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Scalar`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |
| `Struct`  | `Address`, `Field`, `Group`, `Scalar`, `I8`, `I16`, `I32`,`I64`,`I128`, `U8`, `U16`, `U32`, `U64`, `U128` |

***

### `inv`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes the multiplicative inverse of the input, storing the outcome in `destination`.

#### Supported Types

| Input   | Destination |
|---------|-------------|
| `Field` | `Field`     |

***

### `is.eq`

[Back to Top](#table-of-standard-opcodes)

#### Description

Compares `first` and `second`, storing the result in `destination`.

#### Supported Types

| First       | Second      | Destination |
|-------------|-------------|-------------|
| `Address`   | `Address`   | `Boolean`   |
| `Boolean`   | `Boolean`   | `Boolean`   |
| `Field`     | `Field`     | `Boolean`   |
| `Group`     | `Group`     | `Boolean`   |
| `I8`        | `I8`        | `Boolean`   |
| `I16`       | `I16`       | `Boolean`   |
| `I32`       | `I32`       | `Boolean`   |
| `I64`       | `I64`       | `Boolean`   |
| `I128`      | `I128`      | `Boolean`   |
| `U8`        | `U8`        | `Boolean`   |
| `U16`       | `U16`       | `Boolean`   |
| `U32`       | `U32`       | `Boolean`   |
| `U64`       | `U64`       | `Boolean`   |
| `U128`      | `U128`      | `Boolean`   |
| `Scalar`    | `Scalar`    | `Boolean`   |
| `Signature` | `Signature` | `Boolean`   |
| `Struct`    | `Struct`    | `Boolean`   |
| `Record`    | `Record`    | `Boolean`   |

***

### `is.neq`

[Back to Top](#table-of-standard-opcodes)

#### Description

Returns true if `first` is not equal to `second`, storing the result in `destination`.

#### Supported Types

| First       | Second      | Destination |
|-------------|-------------|-------------|
| `Address`   | `Address`   | `Boolean`   |
| `Boolean`   | `Boolean`   | `Boolean`   |
| `Field`     | `Field`     | `Boolean`   |
| `Group`     | `Group`     | `Boolean`   |
| `I8`        | `I8`        | `Boolean`   |
| `I16`       | `I16`       | `Boolean`   |
| `I32`       | `I32`       | `Boolean`   |
| `I64`       | `I64`       | `Boolean`   |
| `I128`      | `I128`      | `Boolean`   |
| `U8`        | `U8`        | `Boolean`   |
| `U16`       | `U16`       | `Boolean`   |
| `U32`       | `U32`       | `Boolean`   |
| `U64`       | `U64`       | `Boolean`   |
| `U128`      | `U128`      | `Boolean`   |
| `Scalar`    | `Scalar`    | `Boolean`   |
| `Signature` | `Signature` | `Boolean`   |
| `Struct`    | `Struct`    | `Boolean`   |
| `Record`    | `Record`    | `Boolean`   |

***

### `lt`

[Back to Top](#table-of-standard-opcodes)

#### Description

Checks if `first` is less than `second`, storing the outcome in `destination`.

#### Supported Types

| First    | Second   | Destination |
|----------|----------|-------------|
| `Field`  | `Field`  | `Boolean`   |
| `I8`     | `I8`     | `Boolean`   |
| `I16`    | `I16`    | `Boolean`   |
| `I32`    | `I32`    | `Boolean`   |
| `I64`    | `I64`    | `Boolean`   |
| `I128`   | `I128`   | `Boolean`   |
| `U8`     | `U8`     | `Boolean`   |
| `U16`    | `U16`    | `Boolean`   |
| `U32`    | `U32`    | `Boolean`   |
| `U64`    | `U64`    | `Boolean`   |
| `U128`   | `U128`   | `Boolean`   |
| `Scalar` | `Scalar` | `Boolean`   |

***

### `lte`

[Back to Top](#table-of-standard-opcodes)

#### Description

Checks if `first` is less than or equal to `second`, storing the outcome in `destination`.

#### Supported Types

| First    | Second   | Destination |
|----------|----------|-------------|
| `Field`  | `Field`  | `Boolean`   |
| `I8`     | `I8`     | `Boolean`   |
| `I16`    | `I16`    | `Boolean`   |
| `I32`    | `I32`    | `Boolean`   |
| `I64`    | `I64`    | `Boolean`   |
| `I128`   | `I128`   | `Boolean`   |
| `U8`     | `U8`     | `Boolean`   |
| `U16`    | `U16`    | `Boolean`   |
| `U32`    | `U32`    | `Boolean`   |
| `U64`    | `U64`    | `Boolean`   |
| `U128`   | `U128`   | `Boolean`   |
| `Scalar` | `Scalar` | `Boolean`   |

***

### `mod`

[Back to Top](#table-of-standard-opcodes)

#### Description

Takes the modulus of `first` with respect to `second`, storing the outcome in `destination`. Halts if `second` is zero.

The semantics of this operation are consistent with the mathematical definition of modulo operation.

#### Supported Types

| First  | Second | Destination |
|--------|--------|-------------|
| `U8`   | `U8`   | `U8`        |
| `U16`  | `U16`  | `U16`       |
| `U32`  | `U32`  | `U32`       |
| `U64`  | `U64`  | `U64`       |
| `U128` | `U128` | `U128`      |

***

### `mul`

[Back to Top](#table-of-standard-opcodes)

#### Description

Multiplies `first` with `second`, storing the outcome in `destination`.

For integer types, a constraint is added to check for overflow/underflow. For cases where wrapping semantics are needed for integer types, see the [mul.w](#mul.w) instruction.

#### Supported Types

| First    | Second   | Destination |
|----------|----------|-------------|
| `Field`  | `Field`  | `Field`     |
| `Group`  | `Scalar` | `Group`     |
| `Scalar` | `Group`  | `Group`     |
| `I8`     | `I8`     | `I8`        |
| `I16`    | `I16`    | `I16`       |
| `I32`    | `I32`    | `I32`       |
| `I64`    | `I64`    | `I64`       |
| `I128`   | `I128`   | `I128`      |
| `U8`     | `U8`     | `U8`        |
| `U16`    | `U16`    | `U16`       |
| `U32`    | `U32`    | `U32`       |
| `U64`    | `U64`    | `U64`       |
| `U128`   | `U128`   | `U128`      |

***

### `mul.w`

[Back to Top](#table-of-standard-opcodes)

#### Description

Multiplies `first` with `second`, wrapping around at the boundary of the type, and storing the outcome in `destination`.

#### Supported Types

| First  | Second | Destination |
|--------|--------|-------------|
| `I8`   | `I8`   | `I8`        |
| `I16`  | `I16`  | `I16`       |
| `I32`  | `I32`  | `I32`       |
| `I64`  | `I64`  | `I64`       |
| `I128` | `I128` | `I128`      |
| `U8`   | `U8`   | `U8`        |
| `U16`  | `U16`  | `U16`       |
| `U32`  | `U32`  | `U32`       |
| `U64`  | `U64`  | `U64`       |
| `U128` | `U128` | `U128`      |

***

### `nand`

[Back to Top](#table-of-standard-opcodes)

#### Description

Returns false only if `first` and `second` are true, storing the outcome in `destination`.

#### Supported Types

| First     | Second    | Destination |
|-----------|-----------|-------------|
| `Boolean` | `Boolean` | `Boolean`   |

***

### `neg`

[Back to Top](#table-of-standard-opcodes)

#### Description

Negates `first`, storing the outcome in `destination`.

For signed integer types, calling `neg` on the minimum value is an invalid operation. For example, the input `-128i8` would not be valid since `128` cannot be represented as an `i8`.

#### Supported Types

| Input   | Destination |
|---------|-------------|
| `Field` | `Field`     |
| `Group` | `Group`     |
| `I8`    | `I8`        |
| `I16`   | `I16`       |
| `I32`   | `I32`       |
| `I64`   | `I64`       |
| `I128`  | `I128`      |

***

### `nor`

[Back to Top](#table-of-standard-opcodes)

#### Description

Returns true when neither `first` nor `second` is true, storing the outcome in `destination`.

#### Supported Type

| First     | Second    | Destination |
|-----------|-----------|-------------|
| `Boolean` | `Boolean` | `Boolean`   |

***

### `not`

[Back to Top](#table-of-standard-opcodes)

#### Description

Perform a NOT operation on an integer (bitwise) or boolean input, storing the outcome in `destination`.

#### Supported Types

| Input     | Destination |
|-----------|-------------|
| `Boolean` | `Boolean`   |
| `I8`      | `I8`        |
| `I16`     | `I16`       |
| `I32`     | `I32`       |
| `I64`     | `I64`       |
| `I128`    | `I128`      |
| `U8`      | `U8`        |
| `U16`     | `U16`       |
| `U32`     | `U32`       |
| `U64`     | `U64`       |
| `U128`    | `U128`      |

***

### or

[Back to Top](#table-of-standard-opcodes)

#### Description

Performs an OR operation on integer (bitwise) or boolean `first` and `second`, storing the outcome in `destination`.

#### Supported Types

| First     | Second    | Destination |
|-----------|-----------|-------------|
| `Boolean` | `Boolean` | `Boolean`   |
| `I8`      | `I8`      | `I8`        |
| `I16`     | `I16`     | `I16`       |
| `I32`     | `I32`     | `I32`       |
| `I64`     | `I64`     | `I64`       |
| `I128`    | `I128`    | `I128`      |
| `U8`      | `U8`      | `U8`        |
| `U16`     | `U16`     | `U16`       |
| `U32`     | `U32`     | `U32`       |
| `U64`     | `U64`     | `U64`       |
| `U128`    | `U128`    | `U128`      |

***

### position

[Back to Top](#table-of-standard-opcodes)

#### Description

The position declaration, e.g. `position <name>`, which indicates a location `name` in the program to branch execution to.  
This opcode is only avaialable in an on-chain context (`async function`).
Positions must be a lowercase alphanumeric string.  

***

### `pow`

[Back to Top](#table-of-standard-opcodes)

#### Description

Raises `first` to the power of `second`, storing the outcome in `destination`.

For integer types, a constraint is added to check for overflow/underflow. For cases where wrapping semantics are needed for integer types, see the [pow.w](#pow.w) instruction.

#### Supported Types

`Magnitude` can be a `U8`, `U16`, or `U32`.

| First   | Second      | Destination |
|---------|-------------|-------------|
| `Field` | `Field`     | `Field`     |
| `I8`    | `Magnitude` | `I8`        |
| `I16`   | `Magnitude` | `I16`       |
| `I32`   | `Magnitude` | `I32`       |
| `I64`   | `Magnitude` | `I64`       |
| `I128`  | `Magnitude` | `I128`      |
| `U8`    | `Magnitude` | `U8`        |
| `U16`   | `Magnitude` | `U16`       |
| `U32`   | `Magnitude` | `U32`       |
| `U64`   | `Magnitude` | `U64`       |
| `U128`  | `Magnitude` | `U128`      |

***

### `pow.w`

[Back to Top](#table-of-standard-opcodes)

#### Description

Raises `first` to the power of `second`, wrapping around at the boundary of the type, storing the outcome in `destination`.

#### Supported Types

`Magnitude` can be a `U8`, `U16`, or `U32`.

| First  | Second      | Destination |
|--------|-------------|-------------|
| `I8`   | `Magnitude` | `I8`        |
| `I16`  | `Magnitude` | `I16`       |
| `I32`  | `Magnitude` | `I32`       |
| `I64`  | `Magnitude` | `I64`       |
| `I128` | `Magnitude` | `I128`      |
| `U8`   | `Magnitude` | `U8`        |
| `U16`  | `Magnitude` | `U16`       |
| `U32`  | `Magnitude` | `U32`       |
| `U64`  | `Magnitude` | `U64`       |
| `U128` | `Magnitude` | `U128`      |

***

### `rand.chacha`

#### Description

The `rand.chacha` opcode is used to generate random values within the `finalize` scope. It supports a wide range of types for the random value.

#### Example Usage

```aleo
rand.chacha into r0 as field;
rand.chacha r0 into r1 as field;
rand.chacha r0 r1 into r2 as field;
rand.chacha 1u8 2i16 into r27 as u32;
```

#### Supported Types

Single can be any of the following types `Address`, `Boolean`, `Field`, `Group`, `I8`, `I16`, `I32`, `I64`, `I128`, `U8`, `U16`, `U32`, `U64`, `U128`, or `Scalar`. Composite data types such as structs and mappings are not allowed.

| First       | Second      | Destination |
|-------------|-------------|-------------|
| `Single` | `Single` | `Single` |

***

### `rem`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes the truncated remainder of `first` divided by `second`, storing the outcome in `destination`. Halts on division by zero and underflow.


A constraint is added to check for underflow.  This underflow happens when the associated division operation, [div](#div), underflows.

For cases where wrapping semantics are needed for integer types, see the [rem.w](#rem.w) instruction.

#### Supported Types

| First  | Second | Destination |
|--------|--------|-------------|
| `I8`   | `I8`   | `I8`        |
| `I16`  | `I16`  | `I16`       |
| `I32`  | `I32`  | `I32`       |
| `I64`  | `I64`  | `I64`       |
| `I128` | `I128` | `I128`      |
| `U8`   | `U8`   | `U8`        |
| `U16`  | `U16`  | `U16`       |
| `U32`  | `U32`  | `U32`       |
| `U64`  | `U64`  | `U64`       |
| `U128` | `U128` | `U128`      |

***

### `rem.w`

[Back to Top](#table-of-standard-opcodes)

#### Description
Computes the truncated remainder of `first` divided by `second`, wrapping around at the boundary of the type, and storing the outcome in destination. Halts on division by zero.

#### Supported Types

| First  | Second | Destination |
|--------|--------|-------------|
| `I8`   | `I8`   | `I8`        |
| `I16`  | `I16`  | `I16`       |
| `I32`  | `I32`  | `I32`       |
| `I64`  | `I64`  | `I64`       |
| `I128` | `I128` | `I128`      |
| `U8`   | `U8`   | `U8`        |
| `U16`  | `U16`  | `U16`       |
| `U32`  | `U32`  | `U32`       |
| `U64`  | `U64`  | `U64`       |
| `U128` | `U128` | `U128`      |

***

### `sign.verify`

[Back to Top](#table-of-standard-opcodes)

#### Description

Verifies the signature `first` against the address public key `second` and the message `third`, storing the outcome in `destination`.

#### Example Usage

```aleo
sign.verify r0 r1 r2 into r3;
```

#### Supported Types

| First       | Second    | Third     | Destination |
|-------------|-----------|-----------|-------------|
| `Signature` | `Address` | `Message` | `Boolean`   |

***

### `shl`

[Back to Top](#table-of-standard-opcodes)

#### Description

Shifts `first` left by `second` bits, storing the outcome in `destination`. Halts if the multiplication or exponentiation represented by the shift overflows.

#### Supported Types

`Magnitude` can be a `U8`, `U16`, or `U32`.

| First  | Second      | Destination |
|--------|-------------|-------------|
| `I8`   | `Magnitude` | `I8`        |
| `I16`  | `Magnitude` | `I16`       |
| `I32`  | `Magnitude` | `I32`       |
| `I64`  | `Magnitude` | `I64`       |
| `I128` | `Magnitude` | `I128`      |
| `U8`   | `Magnitude` | `U8`        |
| `U16`  | `Magnitude` | `U16`       |
| `U32`  | `Magnitude` | `U32`       |
| `U64`  | `Magnitude` | `U64`       |
| `U128` | `Magnitude` | `U128`      |

***

### `shl.w`

[Back to Top](#table-of-standard-opcodes)

#### Description

Shifts `first` left by `second` bits, wrapping around at the boundary of the type, storing the outcome in `destination`.

#### Supported Types

`Magnitude` can be a `U8`, `U16`, or `U32`.

| First  | Second      | Destination |
|--------|-------------|-------------|
| `I8`   | `Magnitude` | `I8`        |
| `I16`  | `Magnitude` | `I16`       |
| `I32`  | `Magnitude` | `I32`       |
| `I64`  | `Magnitude` | `I64`       |
| `I128` | `Magnitude` | `I128`      |
| `U8`   | `Magnitude` | `U8`        |
| `U16`  | `Magnitude` | `U16`       |
| `U32`  | `Magnitude` | `U32`       |
| `U64`  | `Magnitude` | `U64`       |
| `U128` | `Magnitude` | `U128`      |

***

### `shr`

[Back to Top](#table-of-standard-opcodes)

#### Description

Shifts `first` right by `second` bits, storing the outcome in `destination`. Halts when the value of `second` exceeds the number of bits in `first`

#### Supported Types

`Magnitude` can be a `U8`, `U16`, or `U32`.

| First  | Second      | Destination |
|--------|-------------|-------------|
| `I8`   | `Magnitude` | `I8`        |
| `I16`  | `Magnitude` | `I16`       |
| `I32`  | `Magnitude` | `I32`       |
| `I64`  | `Magnitude` | `I64`       |
| `I128` | `Magnitude` | `I128`      |
| `U8`   | `Magnitude` | `U8`        |
| `U16`  | `Magnitude` | `U16`       |
| `U32`  | `Magnitude` | `U32`       |
| `U64`  | `Magnitude` | `U64`       |
| `U128` | `Magnitude` | `U128`      |

***

### `shr.w`

[Back to Top](#table-of-standard-opcodes)

#### Description

Shifts `first` right by `second` bits, wrapping around at the boundary of the type, storing the outcome in `destination`.

#### Supported Types

`Magnitude` can be a `U8`, `U16`, or `U32`.

| First  | Second      | Destination |
|--------|-------------|-------------|
| `I8`   | `Magnitude` | `I8`        |
| `I16`  | `Magnitude` | `I16`       |
| `I32`  | `Magnitude` | `I32`       |
| `I64`  | `Magnitude` | `I64`       |
| `I128` | `Magnitude` | `I128`      |
| `U8`   | `Magnitude` | `U8`        |
| `U16`  | `Magnitude` | `U16`       |
| `U32`  | `Magnitude` | `U32`       |
| `U64`  | `Magnitude` | `U64`       |
| `U128` | `Magnitude` | `U128`      |

***

### `square`

[Back to Top](#table-of-standard-opcodes)

#### Description

Squares the input, storing the outcome in `destination`.

#### Supported Types

| Input   | Destination |
|---------|-------------|
| `Field` | `Field`     |

***

### `sqrt`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes the square root of the input, storing the outcome in `destination`. Halts if no square root exists (the element is a quadratic non-residue).

#### Supported Types

| Input   | Destination |
|---------|-------------|
| `Field` | `Field`     |

***


### `sub`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes `first - second`, storing the outcome in `destination`.

#### Supported Types

| First   | Second  | Destination |
|---------|---------|-------------|
| `Field` | `Field` | `Field`     |
| `Group` | `Group` | `Group`     |
| `I8`    | `I8`    | `I8`        |
| `I16`   | `I16`   | `I16`       |
| `I32`   | `I32`   | `I32`       |
| `I64`   | `I64`   | `I64`       |
| `I128`  | `I128`  | `I128`      |
| `U8`    | `U8`    | `U8`        |
| `U16`   | `U16`   | `U16`       |
| `U32`   | `U32`   | `U32`       |
| `U64`   | `U64`   | `U64`       |
| `U128`  | `U128`  | `U128`      |

***

### `sub.w`

[Back to Top](#table-of-standard-opcodes)

#### Description

Computes `first - second`, wrapping around at the boundary of the type, and storing the outcome in `destination`.

#### Supported Types

| First  | Second | Destination |
|--------|--------|-------------|
| `I8`   | `I8`   | `I8`        |
| `I16`  | `I16`  | `I16`       |
| `I32`  | `I32`  | `I32`       |
| `I64`  | `I64`  | `I64`       |
| `I128` | `I128` | `I128`      |
| `U8`   | `U8`   | `U8`        |
| `U16`  | `U16`  | `U16`       |
| `U32`  | `U32`  | `U32`       |
| `U64`  | `U64`  | `U64`       |
| `U128` | `U128` | `U128`      |

***

### `ternary`

[Back to Top](#table-of-standard-opcodes)

#### Description

Selects `first`, if `condition` is true, otherwise selects `second`, storing the result in `destination`.

Example: `ternary r0 r1 r2 into r3`, where `r0` is the condition, `r1` is first, `r2` is second, and `r3` is the destination.

#### Supported Types

| Condition | First       | Second      | Destination |
|-----------|-------------|-------------|-------------|
| `Boolean` | `Boolean`   | `Boolean`   | `Boolean`   |
| `Boolean` | `Field`     | `Field`     | `Field`     |
| `Boolean` | `Group`     | `Group`     | `Group`     |
| `Boolean` | `I8`        | `I8`        | `I8`        |
| `Boolean` | `I16`       | `I16`       | `I16`       |
| `Boolean` | `I32`       | `I32`       | `I32`       |
| `Boolean` | `I64`       | `I64`       | `I64`       |
| `Boolean` | `I128`      | `I128`      | `I128`      |
| `Boolean` | `U8`        | `U8`        | `U8`        |
| `Boolean` | `U16`       | `U16`       | `U16`       |
| `Boolean` | `U32`       | `U32`       | `U32`       |
| `Boolean` | `U64`       | `U64`       | `U64`       |
| `Boolean` | `U128`      | `U128`      | `U128`      |
| `Boolean` | `Scalar`    | `Scalar`    | `Scalar`    |
| `Boolean` | `Signature` | `Signature` | `Signature` |

***

### `xor`

[Back to Top](#table-of-standard-opcodes)

#### Description

Performs a XOR operation on integer (bitwise) or boolean `first` and `second`, storing the outcome in `destination`.

#### Supported Types

| First     | Second    | Destination |
|-----------|-----------|-------------|
| `Boolean` | `Boolean` | `Boolean`   |
| `I8`      | `I8`      | `I8`        |
| `I16`     | `I16`     | `I16`       |
| `I32`     | `I32`     | `I32`       |
| `I64`     | `I64`     | `I64`       |
| `I128`    | `I128`    | `I128`      |
| `U8`      | `U8`      | `U8`        |
| `U16`     | `U16`     | `U16`       |
| `U32`     | `U32`     | `U32`       |
| `U64`     | `U64`     | `U64`       |
| `U128`    | `U128`    | `U128`      |

***
