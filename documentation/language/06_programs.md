---
id: programs
title: Programs in Practice
sidebar_label: Programs in Practice
---

### Mapping Operations

Mappings can be read from and modified by calling one of the following functions.


#### get

A get command, e.g. `current_value = Mapping::get(counter, addr);`
Gets the value stored at `addr` in `counter` and stores the result in `current_value`
If the value at `addr` does not exist, then the program will fail to execute.

#### get_or_use

A get command that uses the provided default if the key is not present in the mapping,  
e.g. `let current_value: u64 = Mapping::get_or_use(counter, addr, 0u64);`  
Gets the value stored at `addr` in `counter` and stores the result in `current_value`.
If the key is not present, `0u64` is stored in `counter` (associated to the key) and in `current_value`.

#### set

A set command, e.g. `Mapping::set(counter, addr, current_value + 1u64);`
Sets the `addr` entry as `current_value + 1u64` in `counter`.

#### contains

A contains command, e.g. `let contains: bool = Mapping::contains(counter, addr);`
Returns `true` if `addr` is present in `counter`, `false` otherwise.

#### remove

A remove command, e.g. `Mapping::remove(counter, addr);`
Removes the entry at `addr` in `counter`.

#### Usage 

:::info
Mapping operations are only allowed in an [async function](#async-function).
:::

```leo showLineNumbers
program test.aleo {
    mapping counter: address => u64;

    async transition dubble() -> Future {
        return update_mappings(self.caller);
    }

    async function update_mappings(addr: address) {
        let current_value: u64 = Mapping::get_or_use(counter, addr, 0u64);
        Mapping::set(counter, addr, current_value + 1u64);
        current_value = Mapping::get(counter, addr);
        Mapping::set(counter, addr, current_value + 1u64);
    }

}
```



### Transition Function

Transition functions in Leo are declared as `transition {name}() {}`.
Transition functions can be called directly when running a Leo program (via `leo run`).
Transition functions contain expressions and statements that can compute values.
Transition functions must be in a program's current scope to be called.
Transition functions that call [async functions](#async-function) to execute code on-chain must be declared as `async transition`.

```leo showLineNumbers
program hello.aleo {
    transition foo(
        public a: field,
        b: field,
    ) -> field {
        return a + b;
    }
}
```

#### Function Inputs

A function input is declared as `{visibility} {name}: {type}`.
Function inputs must be declared just after the function name declaration, in parentheses.

```leo showLineNumbers
// The transition function `foo` takes a single input `a` with type `field` and visibility `public`.
transition foo(public a: field) { }
```

#### Function Outputs

A function output is calculated as `return {expression};`.
Returning an output ends the execution of the function.
The return type of the function declaration must match the type of the returned `{expression}`.

```leo showLineNumbers
transition foo(public a: field) -> field {
    // Returns the addition of the public input a and the value `1field`.
    return a + 1field;
}
```

### Helper Function

A helper function is declared as `function {name}({arguments}) {}`.
Helper functions contain expressions and statements that can compute values,
however helper functions cannot produce `records`.

Helper functions cannot be called directly. Instead, they must be called by other functions.
Inputs of helper functions cannot have `{visibility}` modifiers like transition functions,
since they are used only internally, not as part of a program's external interface.

```leo showLineNumbers
function foo(
    a: field,
    b: field,
) -> field {
    return a + b;
}
```

### Inline Function

An inline function is declared as `inline {name}() {}`.
Inline functions contain expressions and statements that can compute values.
Inline functions cannot be executed directly from the outside,
instead the Leo compiler inlines the body of the function at each call site.

Inputs of inline functions cannot have `{visibility}` modifiers like transition functions,
since they are used only internally, not as part of a program's external interface.

```leo showLineNumbers
inline foo(
    a: field,
    b: field,
) -> field {
    return a + b;
}
```

The rules for functions (in the traditional sense) are as follows:

- There are three variants of functions: `transition`, `function`, `inline`.
- A `transition` can only call a `function`, `inline`, or external `transition`.
- A `function` can only call an `inline`.
- An `inline` can only call another `inline`.
- Direct/indirect recursive calls are not allowed.

### Async Function

An async function is declared as `async function` and is used to define computation run on-chain. 
A call to an async function returns a [`Future`](#future) object.
It is asynchronous because the code gets executed at a later point in time. 
One of its primary uses is to initiate or change public on chain state within mappings.
An async function can only be called by an async [transition function](#transition-function) and is executed on chain, after the zero-knowledge proof of the execution of the associated transition is verified.
Async functions are atomic; they either succeed or fail, and the state is reverted if they fail.


An example of using an async function to perform on-chain state mutation is in the `transfer_public_to_private` transition below, which updates the public account mapping (and thus a user's balance) when called.

```leo showLineNumbers
program transfer.aleo {
    // The function `transfer_public_to_private` turns a specified token amount
    // from `account` into a token record for the specified receiver.
    //
    // This function preserves privacy for the receiver's record, however
    // it publicly reveals the sender and the specified token amount.
    async transition transfer_public_to_private(
        receiver: address,
        public amount: u64
    ) -> (token, Future) {
        // Produce a token record for the token receiver.
        let new: token = token {
            owner: receiver,
            amount,
        };

        // Return the receiver's record, then decrement the token amount of the caller publicly.
        return (new, update_public_state(self.caller, amount));
    }

    async function update_public_state(
        public sender: address,
        public amount: u64
    ) {
        // Decrements `account[sender]` by `amount`.
        // If `account[sender]` does not exist, it will be created.
        // If `account[sender] - amount` underflows, `transfer_public_to_private` is reverted.
        let current_amount: u64 = Mapping::get_or_use(account, sender, 0u64);
        Mapping::set(account, sender, current_amount - amount);
    }
}
```

If there is no need to create or alter the public on-chain state, async functions are not required.


## Limitations
snarkVM imposes the following limits on Aleo programs:
- the maximum size of the program 100 KB, by the number of characters.
- the maximum number of mappings is 31.
- the maximum number of imports is 64.
- the maximum import depth is 64.
- the maximum call depth is 31.
- the maximum number of functions is 31.
- the maximum number of structs is 310.
- the maximum number of records is 310.
- the maximum number of closures is 62.

**If your *compiled* Leo program exceeds these limits, then consider modularizing or rearchitecting your program.** The only way these limits can be increased is through a formal protocol upgrade via the governance process defined by the Aleo Network Foundation.


Some other protocol-level limits to be aware of are:
- **the maximum transaction size is 128 KB.** If your program exceeds this, perhaps by requiring large inputs or producing large outputs, consider optimizing the data types in your Leo code.
- **the maxmimum number of micro-credits your transaction can consume for on-chain execution is `100_000_000`.**. If your program exceeds this, consider optimizing on-chain components of your Leo code.

As with the above restructions. these limits can only be increased via the governance process.

## Compiling Conditional On-Chain Code
Consider the following Leo transition.
```leo showLineNumbers
transition weird_sub(a: u8, b: u8) -> u8 {
    if (a >= b) {
        return a.sub_wrapped(b);
    } else {
        return b.sub_wrapped(a);
    }
}
```
This is compiled into the following Aleo instructions:
```aleo showLineNumbers
function weird_sub:
    input r0 as u8.private;
    input r1 as u8.private;
    gte r0 r1 into r2;
    sub.w r0 r1 into r3;
    sub.w r1 r0 into r4;
    ternary r2 r3 r4 into r5;
    output r5 as u8.private;
```
Observe that both branches of the conditional are executed in the transition. The correct output is then selected using a ternary instruction. This compilation method is only possible because operations in transitions are purely functional. [^1].

On-chain commands are not all purely functional; for example, `get`, `get.or_use`, `contains`, `remove`, and `set`, whose semantics all depend on the state of the program. As a result, the same technique for off-chain code cannot be used. Instead, the on-chain code is compiled using `branch` and `position` commands, which allow the program to define sequences of code that are skipped. However, because destination registers in skipped instructions are not initialized, they cannot be accessed in a following instructions. In other words, depending on the branch taken, some registers are invalid and an attempt to access them will return in an execution error. The only Leo code pattern that produces such an access attempt is code that attempts to assign out to a parent scope from a conditional statement; consequently, they are disallowed.

This restriction can be mitigated by future improvements to `snarkVM`, however we table that discussion for later.

[^1]: There are some operations that are not purely functional, e.g `add` which can fail on overflow.
