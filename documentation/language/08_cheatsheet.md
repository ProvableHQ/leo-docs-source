---
id: cheatsheet
title: Leo Syntax Cheatsheet
sidebar: Cheatsheet
---
[general tags]: # (program, import, boolean, integer, field, group, scalar, address, signature, array, tuple, struct, operators, cryptographic_operators, assert, hash, commit, random, address, block, transition, async_transition, function, async_function, inline, mapping, conditionals, loops)

## 1. File Import
```leo
import foo.aleo;
```

## 2. Programs
```leo
program hello.aleo {
    // code
}
```

## 3. Primitive Data Types
```leo
// Boolean value (true or false)
let b: bool = false; 

// Signed 32-bit integer (also available: i8, i16, i64, i128)
let i: i32 = -10i32; 

// Unsigned 32-bit integer (also available: u8, u16, u64, u128)
let ui: u32 = 10u32; 

// Field element (used in cryptographic computations)
let a: field = 1field; 

// Group element (used in elliptic curve operations)
let g: group = 0group; 

// Scalar element (used in elliptic curve arithmetic)
let s: scalar = 1scalar; 

// Aleo blockchain address
let receiver: address = aleo1ezamst4pjgj9zfxqq0fwfj8a4cjuqndmasgata3hggzqygggnyfq6kmyd4; 

// Digital signature (used for authentication and verification)
let s: signature = sign1ftal5ngunk4lv9hfygl45z35vqu9cufqlecumke9jety3w2s6vqtjj4hmjulh899zqsxfxk9wm8q40w9zd9v63sqevkz8zaddugwwq35q8nghcp83tgntvyuqgk8yh0temt6gdqpleee0nwnccxfzes6pawcdwyk4f70n9ecmz6675kvrfsruehe27ppdsxrp2jnvcmy2wws6sw0egv;
```

### Type Casting
```leo
// Casting between integer types
let a: u8 = 255u8;
let b: u16 = a as u16; // 255u8 to 255u16
let c: u32 = b as u32; // 255u16 to 255u32
let d: i32 = c as i32; // 255u32 to 255i32

// Casting between field and integers
let f: field = 10field;
let i: i32 = f as i32; // Convert field to i32
let u: u64 = f as u64; // Convert field to u64

// Casting between scalar and field
let s: scalar = 5scalar;
let f_from_scalar: field = s as field; // Convert scalar to field

// Casting between group and field
let g: group = 1group;
let f_from_group: field = g as field; // Convert group to field

// Address casting (only valid conversions)
let addr: address = aleo1ezamst4pjgj9zfxqq0fwfj8a4cjuqndmasgata3hggzqygggnyfq6kmyd4;
let addr_field: field = addr as field; // Convert address to field
```
The primitive types are: `address`, `bool`, `field`, `group`, `i8`, `i16`, `i32`, `i64`, `i128`, `u8`, `u16`, `u32`, `u64`, `u128`, `scalar`.

We can cast between all of these types except `signature`.

You can cast an `address` to a `field` but not vice versa.

### Option Types

```leo
// Boolean option value (true or false or none)
let b_some: bool? = true; 
let b_none: bool? = none; 
// Unwrapping option values
let b_true = b_some.unwrap();
let b_false = b_none.unwrap_or(false);

// Signed 32-bit integer option (also available: i8, i16, i64, i128)
let i_some: i32? = -10i32; 
let i_none: i32? = none; 

// Unsigned 32-bit integer option (also available: u8, u16, u64, u128)
let ui_some: u32? = 10u32; 
let ui_none: u32? = none; 

// Field element option (used in cryptographic computations)
let a_some: field? = 1field; 
let a_none: field? = none; 

// Group element option (used in elliptic curve operations)
let g_some: group? = 0group; 
let g_none: group? = none; 

// Scalar element option (used in elliptic curve arithmetic)
let s_some: scalar? = 1scalar; 
let s_none: scalar? = none; 
```
Both `address` and `signature` types do not have option variants.


## 4. Records
Defining a `record`
```leo
record Token {
    owner: address,
    amount: u64,
}
```

Creating a `record`
```leo
let user: User = User {
    owner: aleo1ezamst4pjgj9zfxqq0fwfj8a4cjuqndmasgata3hggzqygggnyfq6kmyd4,
    balance: 1000u64,
};
```

Accessing `record` fields
```leo
let user_address: address = user.owner;
let user_balance: u64 = user.balance;
```

## 5. Structs
Defining a `struct`
```leo
struct Message {
    sender: address,
    object: u64,
}
```

Creating an instance of a `struct`
```leo
let msg: Message = Message {
    sender: aleo1ezamst4pjgj9zfxqq0fwfj8a4cjuqndmasgata3hggzqygggnyfq6kmyd4,
    object: 42u64,
};
```

Accessing `struct` fields
```leo
let sender_address: address = msg.sender;
let object_value: u64 = msg.object;
```

### Const Generics
```leo
struct Matrix::[N: u32, M: u32] {
    data: [field; N * M],
}

// Usage
let m = Matrix::[2, 2] { data: [0, 1, 2, 3] };
```
Note that generic structs cannot currently be imported outside a program, but can be declared and used in submodules. Acceptable types for const generic parameters include integer types, `bool`, `scalar`, `group`, `field`, and `address`.

### Option Types
Creating an option type instance of a `struct`
```leo
struct Point {
    x : u32,
    y : u32
}

let point1: Point? = Point {
    x: 8u32,
    y: 41u32,
};
let point2: Point? = none;

let point1_val = point1.unwrap();
let point2_val = point2.unwrap_or(Point {x: 0u32, y: 0u32,});
```
Note that because the `address` and `signature` types do not have option variants, a `struct` containing elements of these types also cannot have an option variant.



## 6. Arrays
Declaring `arrays`
```leo
let arrb: [bool; 2] = [true, false];
let arr: [u8; 4] = [1u8, 2u8, 3u8, 4u8]; 
let empty: [u8; 0] = []; 
```

Accessing elements
```leo
let first: u8 = arr[0]; // Get the first element
let second: u8 = arr[1]; // Get the second element
```

Modifying elements

Leo does not support mutable variables, so arrays are immutable after declaration.

Looping Over Arrays
```leo
let numbers: [u32; 3] = [5u32, 10u32, 15u32];

let sum: u32 = 0u32;

for i: u8 in 0u8..3u8 {
    sum += numbers[i];
}
```

## 7. Tuples
Declaring tuples
```leo
let t: (u8, bool, field) = (42u8, true, 100field);
```
**Tuples cannot be empty.**

Accessing tuple elements

Use destructuring
```leo
let (a, b, c) = t; 
```

Index-based access
```leo
let first: u8 = t.0;
let second: bool = t.1;
let third: field = t.2;

```
## 8. Transitions
```leo
transition mint_public(
    public receiver: address,
    public amount: u64,
) -> Token { /* Your code here */ }
```

## 9. Functions
The rules for functions (in the traditional sense) are as follows:

There are three variants of functions:
1. **transition**: Can only call functions and inlines.
2. **functions**: Can only call inlines.
3. **inlines**: Can only call inlines. 

**Direct/indirect recursive calls are not allowed.**

### Inline
An `inline` function is used for **small operations**. It gets **inlined at compile time**, meaning it does not create a separate function call
```leo
inline foo(
    a: field,
    b: field,
) -> field {
    return a + b;
}
```
#### Const Generics
```leo
inline sum_first_n_ints::[N: u32]() -> u32 {
    let sum = 0u32;
    for i in 0u32..N {
        sum += i
    }
    return sum;
}
 
transition main() -> u32 {
    return sum_first_n_ints::[5u32]();
}
```

Acceptable types for const generic parameters include integer types, `bool`, `scalar`, `group`, `field`, and `address`.

✅ Can call: `inline`

❌ Cannot call: `function` or `transition`





### Function
A `function` is used for **computations**. It **cannot** modify state and can only call `inline` functions.
```leo
function compute(a: u64, b: u64) -> u64 {
    return a + b;
}
```
✅ Can call: `inline`

❌ Cannot call: `function` or `transition`


### Transition
A `transition` function **modifies state** (e.g., transfers, updates records). It can call `function` and `inline` functions, but **cannot be called by a function or inline**.
```leo
transition transfer(receiver: address, amount: u64) {
    let balance: u64 = 1000u64; // Example balance
    let new_balance: u64 = subtract(balance, amount);
    // Logic to send `amount` to `receiver` would go here
}

function subtract(a: u64, b: u64) -> u64 {
    return a - b;
}
```
✅ Can call: `function`, `inline`

❌ Cannot call: another `transition` (unless from another program)

### Async Transition
An `async transition` function  **modifies private state** exactly like a regular `transition`, but it also includes an onchain portion that can **modify onchain/public state**. It can call `function` and `inline` functions, but **cannot be called by a function or inline**.  An `async transition` **must** return a `Future`.
```leo
mapping balances: address => u64

async transition mint(receiver: address, amount: u64) -> Future {
    return async {
        let current_balance: u64 = balances.get_or_use(receiver, 0u64);
        let new_balance: u64 = current_balance + amount;
        balances.set(receiver, new_balance);
    };
}

```
✅ Can call: `function`, `inline`

❌ Cannot call: another `transition` (unless from another program)

## 10. Loops
```leo
let count: u32 = 0u32;

for i: u32 in 0u32..5u32 {
    count += 1u32;
}
```

## 11. Conditionals
```leo
let a: u8 = 1u8;

if a == 1u8 {
    a += 1u8;
} else if a == 2u8 {
    a += 2u8;
} else {
    a += 3u8;
}
 
a = (a == 1u8) ? a + 1u8 : ((a == 2u8) ? a + 2u8 : a + 3u8); // Ternary format
```

## 12. Onchain Storage
### Mappings
```leo
mapping balances: address => u64;

let contains_bal: bool = Mapping::contains(balances, receiver);
let get_bal: u64 = Mapping::get(balances, receiver);
let get_or_use_bal: u64 = Mapping::get_or_use(balances, receiver, 0u64);
let set_bal: () = Mapping::set(balances, receiver, 100u64);
let remove_bal: () = Mapping::remove(balances, receiver);
```
### Storage Variables
```leo
storage var: u8

let unwrap_var: u8 = var.unwrap();
let unwrap_or_var: u8 = var.unwrap_or(0u8);
var = 8u8;
var = none;
```
### Storage Vectors
```leo
storage vec: [u8]

let len_vec: u32 = vec.len();
let val: u8 = vec.get(idx)
vec.set(idx, value);
vec.push(value);
vec.pop();
vec.swap_remove(idx);
vec.clear();
```
## 13. Commands
```leo
async transition matches(height: u32) -> Future {
    return async {
        assert_eq(height, block.height); // block.height returns latest block height
    };
}
let this: address = self.address; // address of this program
let caller: address = self.caller; // address of the program function caller (may be another program)
let signer: address = self.signer; // address of the transaction signer (end user)

let a: bool = true;
let b: u8 = 1u8;
let c: u8 = 2u8;
assert(a); // assert the value of a is true
assert_eq(b, b); // assert a and b are equal
assert_neq(b, c); // assert a and b are not equal
```


## 14. Operators
### Standard
```leo
// Arithmetic Operators
let sum: u64 = a + b; // addition (also has wrapped variant)
let diff: u64 = a - b; // subtraction (also has wrapped variant)
let prod: u64 = a * b; // multiplication (also has wrapped variant)
let quot: u64 = a / b; // division (also has wrapped variant)
let power: u64 = a ** b; // exponentiation (also has wrapped variant)
let remainder: u64 = a % b; // remainder (also has wrapped variant)
let neg: i64 = -(a as i64); // negation
let abs : i64 = neg.abs(); // absolute value (also has wrapped variant)

// Bitwise/Boolean Operators
let logical_and: bool = a && b; // logical AND
let logical_or: bool = a || b; // logical OR
let bitwise_and: u64 = a & b; // bitwise AND
let bitwise_or: u64 = a | b; // bitwise OR
let bitwise_xor: u64 = a ^ b; // bitwise XOR
let bitwise_not: u64 = !a; // bitwise NOT
let bitwise_shl: u64 = a << b // bitwise shift left (also has wrapped variant)
let bitwise_shr: u64 = a >> b // bitwise shift left (also has wrapped variant)

// Comparators
let eq: bool = a == b; // equality
let neq: bool = a != b; // non-equality
let lt: bool = a < b; // less than
let lte: bool = a <= b; // less than or equal
let gt: bool = a > b; // greater than
let gte: bool = a >= b; // greater than or equal

// Group & Field Operators
let g: group = group::GEN; // the group generator
let x: field = 0group.to_x_coordinate(); // x-coordinate of a group element 
let y: field = 0group.to_y_coordinate(); // y-coordinate of a group element 
let doubled: group = 1field.double();  // Doubles the field/group element
let inverse: field = 1field.inv(); // Multiplicative inverse of the field/group element
let squared: field = 1field.square(); // Square of the field/group element
let root: field = 1field.square_root(); // Square root of the field/group element

// Context-dependent Expressions
let this: address = self.address;  // Address of program
let caller: address = self.caller; // Address of function caller
let signer: address = self.signer; // Address of tx signer (origin)

// Bit Serialization/Deserialization
let bits: [bool; 58] = Serialize::to_bits(value);  // Standard serialization (includes type metadata)
let raw_bits: [bool; 32] = Serialize::to_bits_raw(value); // Raw serialization (no metadata, just raw bits)
```

### Cryptographic
```leo
// Randomization 
let rand: u32 = ChaCha::rand_u32(); // generate a random value `ChaCha::rand_<type>()`

// Hash Functions (BHP, Pedersen, Poseidon, Keccak, SHA3)
let hash: field = BHP256::hash_to_field(1u32); // hash any type to any type
let hash_raw: address = Poseidon2::hash_to_address_raw(1u8); // hash any raw type to any type
let hash_native: [bool; 256] = Keccak256::hash_native(0field); // hash any type to an array of bits (only available for Keccak and SHA3)
let hash_native_raw: [bool; 256] = Keccak256::hash_native_raw(0field); // hash any raw type to an array of bits (only available for Keccak and SHA3)

// Commitment Algorithms (BHP, Pedersen)
let commit: group = Pedersen64::commit_to_group(1u64, 1scalar); // commit any type to a field, group, or address, using a scalar as blinding factor (salt)

// Schnorr Signatures
let schnorr: bool = signature::verify(sig, addr, 0field) // Schnorr Signature Verification 

// ECDSA Signatures (Keccak, SHA3)
let ecdsa: bool = ECDSA::verify_keccak256(sig, addr, msg); // Verify an ECDSA signature against an ECDSA public key and the Keccak256 hash of a message
let ecdsa_raw: bool = ECDSA::verify_keccak256_raw(sig, addr, msg); // Verify an ECDSA signature against an ECDSA public key and the Keccak256 hash of a raw message
let ecdsa_eth: bool = ECDSA::verify_keccak256_eth(sig, eth_addr, msg); // Verify an ECDSA signature against an Ethereum address and the Keccak256 hash of a raw message

let ecdsa_digest: bool = ECDSA::verify_digest(sig, addr, digest); // Verify an ECDSA signature against an ECDSA public key and a prehashed message
let ecdsa_digest_eth: bool = ECDSA::verify_digest_eth(sig, eth_addr, digest); Verify an ECDSA signature against an Ethereum address and a prehashed message
```