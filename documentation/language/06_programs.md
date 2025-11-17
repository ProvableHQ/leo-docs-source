---
id: programs
title: Programs in Practice
sidebar_label: Programs in Practice
---
[general tags]: # (program, mapping, transition, function, inline, async_transition, async_function)


## Mappings

There are several functions available to query and modify mappings.  The examples below will reference the following mapping:
```leo
mapping balance: address => u64;
```

### Querying
To simply check if a value has been set for a particular `address` in `balance`:
```leo
balance.contains(addr)
Mapping::contains(balance, addr); // Alternate syntax
```
To query a value for a particular `address` in `balance`:
```leo
balance.get(addr)
Mapping::get(balance, addr); // Alternate syntax
```
Note that if value at `addr` does not exist above, then the program will fail to execute.  To query a value with a fallback for this case:
```leo
balance.get_or_use(addr,fallback_value)
Mapping::get_or_use(balance, addr, fallback_value); // Alternate syntax
```

### Modifying
To set a value for a particular `address` in `balance`:
```leo
balance.set(addr,value)
Mapping::set(balance, addr, value); // Alternate syntax
```
To remove the value set at particular `address` in `balance`:
```leo
balance.remove(addr)
Mapping::remove(balance, addr); // Alternate syntax
```

### Usage
```leo showLineNumbers
program mapping.aleo {
    mapping balance: address => u64;

    async transition dubble() -> Future {
        return dubble_onchain(self.caller);
    }

    async function dubble_onchain(addr: address) {
        let current_value: u64 = balance.get_or_use(addr, 0u64);
        balance.set(addr, current_value + 1u64);

        let next_current_value = balance.get(addr);
        balance.set(addr, current_value + 1u64);
    }

}
```

:::info
Mapping operations are only allowed in an [async function](#async-function) or async block.
:::

## Storage Variables

Storage variables behave similar to option types.  There are several functions available to query and modify singleton storage variables.  The examples below will reference the following:
```leo
storage counter: u64;
```

### Querying
To query the value currrently stored at `counter`:
```leo
counter.unwrap();
```
Note that if `counter` has not been initialized, then the program will fail to execute.  To query the value with a fallback for this case:
```leo
counter.unwrap_or(fallback_value);
```
### Modifying
To set a value for `counter`:
```leo
counter = 5u64;
```
To unset the value at `counter`:
```leo
counter = none;
```

### Usage
```leo showLineNumbers
program storage_variable.aleo {
    storage counter: u64;

    async transition increment() -> Future {
        return increment_onchain();
    }

    async function increment_onchain() {
        let current_value: u64 = counter.unwrap_or(0u64);
        counter = current_value + 1u64;
    }

}
```
:::info
Storage variable operations are only allowed in an [async function](#async-function) or async block.
:::


## Storage Vectors

Storage vectors behave like dynamic arrays of values of a given types.  There are several functions available to query and modify storage vectors.  The examples below will reference the following:
```leo
storage id_numbers: [u64];
```

### Querying
To query the element currrently stored in `id_numbers` at index `idx`:
```leo
id_numbers.get(idx);
```
To get the current length of `id_numbers`:
```leo
id_numbers.len();
```
### Modifying
To set an element at index `idx` in `id_numbers`:
```leo
id_numbers.set(idx, value);
```
To push an element onto the end of `id_numbers`:
```leo
id_numbers.push(value);
```
To pop and return the last element of `id_numbers`:
```leo
id_numbers.pop();
```
To remove the element at index `idx`, return it, and replace it with the final element of `id_numbers`:
```leo
id_numbers.swap_remove(idx);
```
To clear the every element in `id_numbers`:
```leo
id_numbers.clear()
```
:::note
- `clear()` does not actually remove any values from the vector. It just sets the length to 0.
- Similarly `swap_remove()` and `pop()` do not actually remove values either. They just reduce the length by 1 to make sure the last element is no longer accessible.
:::

### Usage

```leo showLineNumbers
program storage_vector.aleo {
    storage id_numbers: [u64];

    async transition add_id(new_id: u64) -> Future {
        return add_id_onchain(new_id);
    }

    async function add_id_onchain(new_id: u64) {
        id_numbers.push(new_id);
    }


    async transition remove_id(idx: u32) -> Future {
        return remove_id_onchain(idx);
    }

    async function remove_id_onchain(idx: u32) {
        id_numbers.swap_remove(idx);
    }
}
```
:::info
Storage vector operations are only allowed in an [async function](#async-function) or async block.
:::


## Functions

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

As of Leo v3.0.0, inline functions also support **const generics**:
```leo showLineNumbers
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


### Async Function

An async function is used to define computation run on-chain. It is declared as `async function`, and calls to it return a [`Future`](#future) object.  The term asynchronous is used because the code gets executed at a later point in time.  The most common use case is to initiate or change public on chain state within mappings. An async function can only be called by an [`async transition`](#transition-function) and is executed on chain only after the zero-knowledge proof of the execution of the associated transition is verified.

Async functions are atomic; they either succeed or fail, and the state is reverted if they fail.

An example of using an async function to perform on-chain state mutation is in the `transfer_public_to_private` transition below, which updates the public `account` mapping (and thus a user's balance) when called.

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

Alternatively, you may write async code inside of an `async` block within an `async transition` function.  Below is what that would look like for the same `transfer_public_to_private` transition:
```leo showLineNumbers
program transfer.aleo {
    async transition transfer_public_to_private(
        receiver: address,
        public amount: u64
    ) -> (token, Future) {
        let new: token = token {
            owner: receiver,
            amount,
        };

        let f : Future = async {
            let current_amount: u64 = Mapping::get_or_use(account, self.caller, 0u64);
            Mapping::set(account, self.caller, current_amount - amount);
        }

        return (new, f);
    }
}
```

If there is no need to create or alter the public on-chain state, async functions are not required.

### Function Call Rules

- There are three function variants: `transition`, `function`, and `inline`.
- A `transition` can call: `function`, `inline`, and external `transition`s.
- A `function` can only call: `inline`s.
- An `inline` can only call: other `inline`s.
- Recursive calls (direct or indirect) are not allowed.

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
