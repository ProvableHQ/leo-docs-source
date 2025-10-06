---
id: structure 
title: Structure of a Leo Program 
sidebar_label: Program Structure
---
[general tags]: # (program, constant, import, transition, async_transition, function, async_function, inline, record, struct, mapping)

## Layout of a Leo Program

A Leo program contains declarations of a [Program](#program), [Constants](#constant), [Imports](#import)
, [Transition Functions](#transition-function), [Async Functions](#async-function), [Helper Functions](#helper-function), [Structs](#struct)
, [Records](#record), and [Mappings](#mapping).
Declarations are locally accessible within a program file.
If you need a declaration from another Leo file, you must import it.

### Program 

A program is a collection of code (its functions) and data (its types) that resides at a
[program ID](#program-id) on the Aleo blockchain. A program is declared as `program {name}.{network} { ... }`.
The body of the program is delimited by curly braces `{}`.

```leo
import foo.aleo;

program hello.aleo {
    const FOO: u64 = 1u64;
    mapping account: address => u64;

    record Token {
        owner: address,
        amount: u64,
    }

    struct Message {
        sender: address,
        object: u64,
    }

    async transition mint_public(
        public receiver: address,
        public amount: u64,
    ) -> (Token, Future) {
        return (Token {
            owner: receiver,
            amount,
        }, update_state(receiver, amount));
    }

    async function update_state(
        public receiver: address,
        public amount: u64,
    ) {
        let current_amount: u64 = Mapping::get_or_use(account, receiver, 0u64);
        Mapping::set(account, receiver, current_amount + amount);
   }

    function compute(a: u64, b: u64) -> u64 {
        return a + b + FOO;
    }
}
```

The following must be declared inside the scope of a program in a Leo file:

- constants
- mappings
- record types
- struct types
- transition functions
- helper functions
- async functions

The following must be declared outside the scope of a program in a Leo file:

- imports

#### Program ID

A program ID is declared as `{name}.{network}`.

The first character of a `name` must be a lowercase letter.
`name` can only contain lowercase letters, numbers, and underscores, and must not contain a double underscore (`__`) or the keyword `aleo` in it.

Currently, `aleo` is the only supported `network` domain.

```leo showLineNumbers
program hello.aleo; // valid

program Foo.aleo;   // invalid
program baR.aleo;   // invalid
program 0foo.aleo;  // invalid
program 0_foo.aleo; // invalid
program _foo.aleo;  // invalid
```

### Constant

A constant is declared as `const {name}: {type} = {expression};`.  
Constants are immutable and must be assigned a value when declared.  
Constants can be declared in the global scope or in a local function scope.  

```leo
program foo.aleo {
    const FOO: u8 = 1u8;
    
    function bar() -> u8 {
        const BAR: u8 = 2u8;
        return FOO + BAR;
    }
}
```

### Import

You can import dependencies that are downloaded to the `imports` directory.
An import is declared as `import {filename}.aleo;`
The dependency resolver will pull the imported program from the network or the local filesystem.

```leo showLineNumbers
import foo.aleo; // Import all `foo.aleo` declarations into the `hello.aleo` program.

program hello.aleo { }
```

### Mappings

A mapping is declared as `mapping {name}: {key-type} => {value-type}`.
Mappings contain key-value pairs.
Mappings are stored on chain.

```leo showLineNumbers
// On-chain storage of an `account` mapping,
// with `address` as the type of keys,
// and `u64` as the type of values.
mapping account: address => u64;
```

### Struct

A struct data type is declared as `struct {name} {}`.
Structs contain component declarations `{name}: {type},`.

```leo showLineNumbers
struct Array3 {
    a0: u32,
    a1: u32,
    a2: u32,
}
```

### Record

A [record](https://developer.aleo.org/concepts/fundamentals/records) data type is declared as `record {name} {}`. A record name must not contain the keyword `aleo`, and must not be a prefix of any other record name.

Records contain component declarations `{visibility} {name}: {type},`. Names of record components must not contain the keyword `aleo`. 

A visibility can be either `constant`, `public`, or `private`.
Users may also omit the visibility, in which case, Leo will default to `private`.

Record data structures must contain the `owner` component as shown below. When passing a record as input to a program function, the `_nonce: group` component is also required but it does not need to be declared in the Leo program. It is inserted automatically by the compiler.

```aleo showLineNumbers
record Token {
    // The token owner.
    owner: address,
    // The token amount.
    amount: u64,
}
```