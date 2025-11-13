---
id: functions
title: Functions
sidebar_label: Functions
---
[general tags]: # (transition, function, inline, async_transition, async_function)



## Transition Function

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

### Function Inputs

A function input is declared as `{visibility} {name}: {type}`.
Function inputs must be declared just after the function name declaration, in parentheses.

```leo showLineNumbers
// The transition function `foo` takes a single input `a` with type `field` and visibility `public`.
transition foo(public a: field) { }
```

### Function Outputs

A function output is calculated as `return {expression};`.
Returning an output ends the execution of the function.
The return type of the function declaration must match the type of the returned `{expression}`.

```leo showLineNumbers
transition foo(public a: field) -> field {
    // Returns the addition of the public input a and the value `1field`.
    return a + 1field;
}
```

## Helper Function

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

## Inline Function

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


## Async Function

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

## Function Call Rules

- There are three function variants: `transition`, `function`, and `inline`.
- A `transition` can call: `function`, `inline`, and external `transition`s.
- A `function` can only call: `inline`s.
- An `inline` can only call: other `inline`s.
- Recursive calls (direct or indirect) are not allowed.
