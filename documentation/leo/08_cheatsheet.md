---
id: cheatsheet
title: Leo Syntax Cheatsheet
---

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
## 4. Records
Defining a `record`
```leo
record token {
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
struct message {
    sender: address,
    object: u64,
};
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

## 6. Arrays
Declaring `arrays`
```leo
let arrb: [bool; 2] = [true, false];
let arr: [u8; 4] = [1u8, 2u8, 3u8, 4u8]; 
```
**Arrays cannot be empty.**

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
**Tuples cannot be empty or modified. Same with arrays.**

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
) -> token { /* Your code here */ }
```

## 9. Functions
The rules for functions (in the traditional sense) are as follows:

There are three variants of functions:
1. **transition**: Can only call functions and inlines.
2. **functions**: Can only call inlines.
3. **inlines**: Can only call inlines. 

**Direct/indirect recursive calls are not allowed**

### (Internal) Functions
A `function` is used for **computations**. It **cannot** modify state and can only call `inline` functions.
```leo
function compute(a: u64, b: u64) -> u64 {
    return a + b;
}
```
✅ Can call: `inline`

❌ Cannot call: `function` or `transition`

### Inline Functions
An `inline` function is used for **small operations**. It gets **inlined at compile time**, meaning it does not create a separate function call
```leo
inline foo(
    a: field,
    b: field,
) -> field {
    return a + b;
}
```
✅ Can call: `inline`

❌ Cannot call: `function` or `transition`

### Transition Functions
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

❌ Cannot call: another `transition`

## 10. For Loops
```leo
let count: u32 = 0u32;

for i: u32 in 0u32..5u32 {
    count += 1u32;
}
```

## 11. Mappings
```leo
mapping balances: address => u64;

let contains_bal: bool = Mapping::contains(balances, receiver);
let get_bal: u64 = Mapping::get(balances, receiver);
let get_or_use_bal: u64 = Mapping::get_or_use(balances, receiver, 0u64);
let set_bal: () = Mapping::set(balances, receiver, 100u64);
let remove_bal: () = Mapping::remove(balances, receiver);
```

## 12. Commands
```leo
transition matches(height: u32) -> Future {
    return check_height_matches(height);
}
async function check_height_matches(height: u32) {
    assert_eq(height, block.height); // block.height returns latest block height
}

let g: group = group::GEN; // the group generator
let result: u32 = ChaCha::rand_u32(); // generate a random value `ChaCha::rand_<type>()`
let owner: address = self.caller; // address of the program function caller
let hash: field = BHP256::hash_to_field(1u32); // hash any type to any type
let commit: group = Pedersen64::commit_to_group(1u64, 1scalar); // commit any type to a field, group, or address, using a scalar as blinding factor

let a: bool = true;
assert(a); // assert the value of a is true

let a: u8 = 1u8;
let b: u8 = 2u8;
assert_eq(a, a); // assert a and b are equal
assert_neq(a, b); // assert a and b are not equal
```


## 13. Operators
```leo
let sum: u64 = a + b; // arithmetic addition
let diff: u64 = a - b; // arithmetic subtraction
let prod: u64 = a * b; // arithmetic multiplication
let quot: u64 = a / b; // arithmetic division
let remainder: u64 = a % b; // arithmetic remainder
let neg: u64 = -a; // negation
let bitwise_and: u64 = a & b; // bitwise AND
let bitwise_or: u64 = a | b; // bitwise OR
let bitwise_xor: u64 = a ^ b; // bitwise XOR
let bitwise_not: u64 = !a; // bitwise NOT
let logical_and: bool = a && b; // logical AND
let logical_or: bool = a || b; // logical OR
let eq: bool = a == b; // equality
let neq: bool = a != b; // non-Equality
let lt: bool = a < b; // less than
let lte: bool = a <= b; // less than or equal
let gt: bool = a > b; // greater than
let gte: bool = a >= b; // greater than or equal
```

