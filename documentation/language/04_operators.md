---
id: operators
title: Operators & Expressions
sidebar_label: Operators and Expressions
---
[general tags]: # (operators, cryptographic_operators, assert, hash, commit, random, address, block)

## Operators

Operators in Leo compute a value based off of one or more expressions. Leo defaults to checked arithmetic, which means that it will throw an error if an overflow or division by zero is detected.

For instance, addition adds `first` with `second`, storing the outcome in `destination`. For integer types, a constraint is added to check for overflow.
For cases where wrapping semantics are needed for integer types, see the wrapped variants of the operators.

```leo
let a: u8 = 1u8 + 1u8;
// a is equal to 2

a += 1u8;
// a is now equal to 3

a = a.add(1u8);
// a is now equal to 4
```

:::note
The Leo operators compile down to [`Aleo Instructions`](https://developer.aleo.org/guides/aleo/opcodes) opcodes executable by the Aleo Virtual Machine (AVM). 
:::

### Operator Precedence

Operators will prioritize evaluation according to:

|                                   Operator                                    | Associativity |
| :---------------------------------------------------------------------------: | :-----------: |
|                                `!` `-`(unary)                                 |               |
|                                     `**`                                      | right to left |
|                                    `*` `/`                                    | left to right |
|                                `+` `-`(binary)                                | left to right |
|                                   `<<` `>>`                                   | left to right |
|                                      `&`                                      | left to right |
|                              <code>&#124;</code>                              | left to right |
|                                      `^`                                      | left to right |
|                               `<` `>` `<=` `>=`                               |               |
|                                   `==` `!=`                                   | left to right |
|                                     `&&`                                      | left to right |
|                           <code>&#124;&#124;</code>                           | left to right |
| `=` `+=` `-=` `*=` `/=` `%=` `**=` `<<=` `>>=` `&=` <code>&#124;=</code> `^=` |               |

### Parentheses

To prioritize a different evaluation, use parentheses `()` around the expression.

```leo
let result = (a + 1u8) * 2u8;
```

`(a + 1u8)` will be evaluated before multiplying by two `* 2u8`.

## Hashing vs. Committing

Many of the cryptographic operators have both `hash` and `commit` variants.  

The `hash` variant is a one-way function that takes an input produces a fixed-size output called a "hash" or "digest." It has a unique property that if even one bit of the input changes, the output hash will change completely, making it easy to see if data has been tampered with. 

The `commit` variant is a wrapper around the `hash` variant that takes an additional parameter called a blinding factor, otherwise known as a **salt**.  The **salt** is appended to the input value before hashing it, ensuring the output will be unique from just the simple hash of the raw input.  So long as a different salt is used each time, this allows a user to commit to the same value multiple times without revealing that they've done so.
