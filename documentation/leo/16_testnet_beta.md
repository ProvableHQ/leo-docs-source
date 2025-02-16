---
id: testnetbeta
title: Getting Ready for Testnet Beta
sidebar_label: Testnet Beta
---

# Getting Ready for Testnet Beta


We are about to reach a huge milestone in Aleo's developement. Testnet Beta is around the corner and it represents the last major upgrade before `mainnet`. In preparation, Leo developers may have to make changes to your Leo programs. This guide highlights all breaking changes, introduces new features, and expands upon key concepts.


**IMPORTANT:**
- **Leo v1.12.0 will be the last Testnet3 compatible version.**
- **Leo v.2.0.0 is the first Testnet Beta compatible version.** (In the meantime, you can build from source on the `testnet-beta` branch of the Leo repository.)

For additional support, please feel free to:
- File an issue [here](https://github.com/ProvableHQ/leo/issues/new/choose).
- Post in the #leo-language Discord [channel](https://discord.com/invite/aleo).
- Attend the Leo Core Devs Call / Engineering Office Hours.

## Breaking Changes
We've included a check-list of features that are deprecated in Leo v2.0.0. If you rely on any of these, be sure to update your code!

- [ ] [API Endpoints](#api-endpoints)
- [ ] [Finalization](#finalization)
- [ ] [Assignment In Conditional On-chain Code](#assignment-in-conditional-on-chain-code)
- [ ] [Input Files](#input-files)
- [ ] [Naming Programs](#naming-programs)
- [ ] [Defining and Using Imported Structs](#defining-and-using-imported-structs)
- [ ] [Program Limits](#program-limits)


*If you run into breaking changes that were not covered above or addressed insufficiently, we'd appreciate it if you file an issue [here](https://github.com/ProvableHQ/leo/issues/new/choose). The Leo Team will reach out and help you migrate your applications appropriately.*

## Migrations
For each of the breaking changes above, we've provided instructions on how to update your programs accordingly. Some of these will be one-line fixes and others will be more conceptually involved.

### API Endpoints
If you are using API endpoints, you will likely **need to update the URL** to point to the new Testnet Beta endpoint.

**The official endpoint is `https://api.explorer.provable.com/v1/testnet`.**
If you are using a custom endpoint, you will just need to update `testnet3` to `testnet` in the URL.

The [Explorer](https://explorer.provable.com/) is pointed to Testnet Beta.

### Finalization
The `finalize` programming model for on-chain code has been replaced with a new `async/await` model. See [this](#An-Async-Programming-Model) section for a detailed breakdown.

The key difference between the two models is that instead of a `finalize` block, on-chain code is defined in an `async function`. Note that if a program does not declare on-chain code (there are not `finalize` blocks) then it will work without modification under the `async/await` model.

In the rest of this section, we provide two examples for converting `finalize`-style Leo programs to the `async/await` model. **Developers should be able to follow these as a formula for converting their programs.** That said we still encourage developers to learn the concepts in [this](#An-Async-Programming-Model) section.

Consider the following code in the `finalize` model. Note that this code does **not** make any external calls.
```leo showLineNumbers
program foo.aleo {
    record credit { ... }
    transition bar(...) -> credit {
        ...
        let c: credit = ...;
        ...
        return c then finalize(...);
    }
    finalize bar(...) {
        ...
    }
}
```
This can be rewritten in the `async/await` model as:
```leo showLineNumbers
program foo.aleo {
    record credit { ... }
    async transition bar(...) -> (credit, Future) {
        ...
        let c: credit = ...;
        ...
        return (c, finalize_bar(...));
    }
    async function finalize_bar(...) {
        ...
    }
}
```

Now let's consider the following code in the `finalize` model. Note that this code makes an external call to `foo.aleo` in `finalize` model, above.
```leo showLineNumbers
import foo.aleo;

program boo.aleo {
    transition baz(...) -> foo.aleo/credit {
        ...
        let c: foo.aleo/credit = foo.aleo/bar(...);
        ...
        return c then finalize(...);
    }
    finalize baz(...) {
        ...
    }
}
```
This can be rewritten in the `async/await` model as:
```leo showLineNumbers
import foo.aleo;

program boo.aleo {
    async transition baz(...) -> (foo.aleo/credit, Future) {
        ...
        let (c, f): (foo.aleo/credit, Future) = foo.aleo/bar(...);
        ...
        return (c, finalize_baz(f, ...));
    }

    async function finalize_baz(f: Future, ...) {
        f.await();
        ...
    }
}
```



### Assignment In Conditional On-chain Code
Updates to `snarkVM` have introduced `branch` instructions, which allows users to conditionally execute code. For example, users can remove an entry in a mapping if and only if a condition is met. However, this imposes some fundamental restrictions on the certain code structures in Leo. See [this](#Compiling-Conditional-On-Chain-Code) section for a conceptual breakdown.

Specifically, Leo v2.0.0 does not allow programs to re-assign to a variable declared in a scope outside of the existing one, solely in the on-chain portion of code.

For example, the following code will result in a compiler error:
```leo showLineNumbers
let x: u8 = 1u8;
if (condition) {
    x = x + y;
} else {
    x = x + z;
}
data.set(0u8, x);
```

Users have a number of options to get around this restriction. One option is to explicitly expand the conditional paths. For example:
```leo showLineNumbers
let x: u8 = 1u8;
if (condition) {
    x = x + y;
    data.set(0u8, x);
} else {
    x = x + z;
    data.set(0u8, x);
}
```

Another option is to use ternary operators to correctly craft a code block equivalent to the original.
```leo showLineNumbers
let x: u8 = 1u8;
let x_1: u8 = x + y;
let x_2: u8 = x + z;
x = condition ? x_1 : x_2;
data.set(0u8, x);
```

### Input Files

The Leo CLI used to accept inputs via files with a special syntax. **This has been deprecated in favor of a standard CLI format across `snarkOS`, `snarkVM`, and `leo`.**

For example, if you had an input file:
```leo showLineNumbers
// Filename: hello.in
// The program input for hello/src/main.leo

[main]
public a: u32 = 1u32;
b: u32 = 2u32;
```
you would run the `main` transition via `leo run main`. The Leo CLI would find the appropriate input file and run the `main` transition with inputs `1u32` and `2u32`

Instead, you can run the `main` transition via `leo run main 1u32 2u32`. You can alternatively pass in the `--file` argument and provide an input file, containing the inputs in a space-separated form. For example, you can pass in a file `main.in`, whose contents are:
```leo showLineNumbers
1u32 2u32
```
### Naming Programs

If you haven't updated Leo in a while, you're probably used to using `.leo` in your file extensions and in your imports, programs, and external calls. For example, `import foo.leo`, `program foo.leo`, and `foo.aleo/bar(...)`, respectively.


**The updated rules are:**
- all files containing Leo programs must end with a `.leo` file extension.
- all imports must be defined with the `.aleo` prefix.
- all programs must be declared with the `.aleo` prefix.
- all external calls must be made with the `.aleo` prefix.

For example, if you originally had following file:
```leo showLineNumbers
// Filename: main.leo

import foo.leo;

progam baz.leo {
    transition quip {
        return foo.leo/bar();
    }
}
```

The updated program should be:
```leo showLineNumbers
// Filename: main.leo

import foo.aleo;

progam baz.aleo {
    transition quip {
        return foo.aleo/bar();
    }
}
```
### Defining and Using Imported Structs

### Program Limits
For Testnet Beta, snarkVM imposes the following limits on Aleo programs:
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
- **the maximum transaction size is 128 KB.** If your program execeeds this, perhaps by requiring large inputs or producing large outputs, consider optimizing the data types in your Leo code.
- **the maxmimum number of micro-credits your transaction can consume for on-chain execution is `100_000_000`.**. If your program exceeds this, consider optimizing on-chain components of your Leo code.

As with the above restructions. these limits can only be increased via the governance process.

## New Features

Here is a list of some of the new features that are available to you in Leo v2.0.0!

### Reading External Mappings
Leo now allows users to read all mappings defined in programs that have been imported. Just as with reading local mappings, this operation must take place in an async function.

```leo showLineNumbers
let val:u32 = Mapping::get(token.aleo/account, 0u32);
let val_2:u32 = Mapping::get_or_use(token.aleo/account, 0u32, 0u32);
```

### `leo deploy`
Users can now deploy Leo programs directly from the command-line without having to first compile them and then after use the `snarkos developer` CLI too.

Highlighted features:
- Recursive deployments using `--recursive`. No matter how many layers of dependencies there are in a leo project, this CLI flag enables users to deploy all programs at once.
- Paying for program deployment with a private credits record by using `--record`.
- Configurable endpoint and network with `--endpoint` and `--network`.

### `leo execute`
Users can now easily execute programs on-chain, even if they don't match the local directory. Make sure to have a funded private key in `.env` or specify one using `--private-key`.

```
leo execute --program credits.aleo --broadcast transfer_public_to_private <ADDRESS> 500u64
```

### `leo query`
Extensive information on deployed programs, mapping values, blocks, transactions, node peers, validator committees and the memory pool are now accessible using the leo CLI tool.

```
leo query program credits.aleo --mapping-value account aleo1rhgdu77hgyqd3xjj8ucu3jj9r2krwz6mnzyd80gncr5fxcwlh5rsvzp9px
       Leo ✅ Successfully retrieved data from 'http://api.explorer.provable.com/v1/testnet/program/credits.aleo/mapping/account/aleo1rhgdu77hgyqd3xjj8ucu3jj9r2krwz6mnzyd80gncr5fxcwlh5rsvzp9px'.

"10331249u64"
```

### `leo add/remove`
Enables seemless and intuitive dependency management. Users can import already-deployed network programs or programs from local projects as dependencies.

```
// Pull credits.aleo as a dependency
leo add -n credits

// Add a local dependency named foo.aleo whose leo project path is ../foo
leo add -l ../foo foo

// Attach dependencies inside the Leo file
import credits.aleo
import foo.aleo
```


## Concepts

This section dives into new concepts introduced in the latest updates to Leo.

### An Async Programming Model
The `finalize` model was introduced to naturally enable a novel hybrid programming model, where the first part of a program is executed off-chain and the second part is executed on-chain. However, this model had its shortcomings, mainly that it was not possible define the ordering in which code was finalized.

For example, suppose we have a function `A` first calls `B` and then calls `C`. The finalization order would be the order in which the off-chain components of the function finished, e.g `B`, `C`, `A`.

To enable more expressive programs, the `finalize` model was deprecated in favor of the `async/await` model. This model borrows heavily from asynchronous programming models in other programming languages, but has some restrictions that are specific to the Aleo blockchain. Users familiar with asynchronous programming should find its instantiation in Leo familiar.

At a high-level, on-chain code is asynchronous code that does not return a value. Instead it returns a `Future`. Futures can be composed to execute simply one after another or through complex control flow.

The rules in the `async/await` model are:
- Async functions can only be called from an `async transition`.
- Async functions cannot return values.
- Async transitions cannot be called inside a conditional block.
- An async function call, returns a `Future`, which is code to be executed later.
- An async transition call, returns an optional value, and must return a single `Future`, which is code to be executed later.
- A `Future` produced by an async transition call cannot be returned.
- A `Future` can be passed into an async function call and must be `await`ed.
- All `Future`s must either be consumed by an async function call or returned as an output.


### Compiling Conditional On-Chain Code
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

This restriction can be mitigated by future improvements to `snarkVM`, however we table that discusstion for later.



[^1]: There are some operations that are not purely functional, e.g `add` which can fail on overflow.
