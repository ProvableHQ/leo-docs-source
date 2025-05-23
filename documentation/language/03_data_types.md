---
id: data_types
title: Data Types
sidebar_label: Data Types
---

### Booleans

Leo supports the traditional `true` or `false` boolean values. The explicit `bool` type for booleans in statements is
required.

```leo
let b: bool = false;
```

### Integers

Leo supports signed integer types `i8`, `i16`, `i32`, `i64`, `i128`
and unsigned integer types `u8`, `u16`, `u32`, `u64`, `u128`.

```leo
let b: u8 = 1u8;
```

Underscores `_` can be used to separate digits in integer literals.

```leo
let n: u64 = 1_000_000u64;
```

:::info
Higher bit length integers generate more constraints in the circuit, which can slow down computation time.
:::

#### A Note on Leo Integers

Leo will not default to an integer type. The definition of an integer **must** include an explicit type.  
**Type casting is supported as of Leo v1.8.2**

```leo
let a: u8 = 2u8; // explicit type
let b: u16 = a as u16; // type casting
let c: u8 = 2; // implicit type -- not supported
```

### Field Elements

Leo supports the `field` type for elements of the base field of the elliptic curve.
These are unsigned integers less than the modulus of the base field. The following are the
smallest and largest field elements.

```leo
let a: field = 0field;
let b: field = 8444461749428370424248824938781546531375899335154063827935233455917409239040field;
```

### Group Elements

The set of affine points on the elliptic curve forms a group.
The curve is a Twisted Edwards curve with `a = -1` and `d = 3021`.
Leo supports a subgroup of the group, generated by a generator point, as a primitive data type.
A group element is denoted by the x-coordinate of its point; for example,
`2group` means the point `(2, 5553594316923449299484601589326170487897520766531075014687114064346375156608)`.

```leo
let a: group = 0group; // the point with 0 x-coordinate, (0, 1)
let b: group = 1540945439182663264862696551825005342995406165131907382295858612069623286213group;  // the generator point
```

The aforementioned generator point can be obtained via a constant associated to the `group` type.

```leo
let g: group = group::GEN; // the group generator
```

### Scalar Elements

Leo supports the `scalar` type for elements of the scalar field defined by the elliptic curve subgroup.
These are unsigned integers less than the modulus of the scalar field. The following are the smallest and largest scalars.

```leo
let a: scalar = 0scalar;
let b: scalar = 2111115437357092606062206234695386632838870926408408195193685246394721360382scalar;
```

### Addresses

Addresses are defined to enable compiler-optimized routines for parsing and operating over addresses.
These semantics will be accompanied by a standard library in a future sprint.

```leo
let receiver: address = aleo1ezamst4pjgj9zfxqq0fwfj8a4cjuqndmasgata3hggzqygggnyfq6kmyd4;
```

### Signatures

Aleo uses the Schnorr signature scheme to sign messages with an Aleo private key.
Signatures are a native type in Leo, and can be declared with the keyword `signature`.
Signatures can be verified in Leo using the [`signature::verify`](./04_operators.md#signatureverify) or [`s.verify`](./04_operators.md#signatureverify) operators.

```leo
program test.aleo {

    struct foo {
        a: u8,
        b: scalar
    }

    transition verify_field(s: signature, a: address, v: field) {
        let first: bool = signature::verify(s, a, v);
        let second: bool = s.verify(a, v);
        assert_eq(first, second);
    }

    transition verify_foo(s: signature, a: address, v: foo) {
        let first: bool = signature::verify(s, a, v);
        let second: bool = s.verify(a, v);
        assert_eq(first, second);
    }
}
```


### Array

Leo supports static arrays. Array types are declared as `[type; length]` and can be nested. Arrays cannot be empty nor modified.

Arrays only support constant accesses. The accessor expression must be a constant expression.

Arrays can contain primitive data types, structs, or arrays. Structs and records can also contain arrays.

Arrays can be iterated over using a for loop.

```leo
// Initalize a boolean array of length 4
let arr: [bool; 4] = [true, false, true, false];

// Nested array
let nested: [[bool; 2]; 2] = [[true, false], [true, false]];

struct Bar {
    data: u8,
}

// Array of structs
let arr_of_structs: [Bar; 2] = [Bar { data: 1u8 }, Bar { data: 2u8 }];

// Access the field of a struct within an array
transition foo(a: [Bar; 8]) -> u8 {
    return a[0u8].data;
}

// Struct that contains an array
struct Bat {
    data: [u8; 8],
}

// Record that contains an array
record Floo {
    owner: address,
    data: [u8; 8],
}

// Declare a mapping that contains array values
mapping data: address => [bool; 8];

// Iterate over an array using a for loop and sum the values within
transition sum_with_loop(a: [u64; 4]) -> u64 {
    let sum: u64 = 0u64;
    for i: u8 in 0u8..4u8 {
        sum += a[i];
    }
    return sum;
}
```

### Tuple

Leo supports tuples. Tuple types are declared as `(type1, type2, ...)` and can be nested. Tuples cannot be empty or modified.

Tuples only support constant access with a dot `.` and a constant integer.

Tuples can contain primitive data types, structs, or arrays. Structs and records can also contain tuples.

```leo
program test.aleo {
    transition baz(foo: u8, bar: u8) -> u8 {
        let a: (u8, u8) = (foo, bar);
        let result: u8 = a.0 + a.1;
        return result;
    }
}
```


### Structs

Struct types are declared and constructed with a familiar syntax. Note that there is a global namespace for struct
types across your program and its dependencies. If a dependency declares a struct type `T`, you may access that type
without any qualifier.

```leo
program test.aleo {
    struct S {
        x: field,
        y: u32,
    }

    transition foo(y: u32) -> S {
        let s: S = S {
            x: 172field,
            y,
        };
        return s;
    }
}
```
