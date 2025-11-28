---
id: style
title: Best Practices
sidebar: Best Practices
---
[general tags]: # ()

This guide is provided to point developers in the right direction when writing Leo code.
There are many conventions that are unique to the Leo language and the circuits it generates.

This guide is a living document.
As new Leo programming conventions arise and old ones become obsolete this guide should reflect the changes.
Feel free to add your comments and recommendations [here](#contributing).

## Content

### Conditional Branches

The Leo compiler rewrites if-else statements inside `transitions` into a sequence of ternary expressions.
This is because the underlying circuit construction does not support branching.
For precise control over the circuit size, it is recommended to use ternary expressions directly.

```leo title="If-Else:"
if (condition) {
    return a;
} else {
    return b;
} 
```

```leo title="Ternary:"
return condition ? a : b;
```

#### Why?
Ternary expressions are the cheapest form of conditional.
We can resolve the *first expression* and *second expression* values before evaluating the *condition*.
This is very easy to convert into a circuit because we know that each expression does not depend on information in later statements.

In the original `Example`,
We cannot resolve the return statements before evaluating the condition.
As a solution, Leo creates branches in the circuit so both paths can be evaluated.

```leo title="branch 1, condition = true"
return a;
```

```leo title="branch 2, condition = false"
return b;
```
When the input value `condition` is fetched at proving time, we select a branch of the circuit to evaluate.
Observe that the statement `return a` is repeated in both branches.
The cost of every computation within the conditional will be doubled.
This greatly increases the constraint numbers and slows down the circuit.


### Async Functions vs. Blocks

For code conciseness and readability, prefer using `async` blocks rather than a separately declared `async function`:


```leo title="Async Function:"
mapping accumulator: u8 => u64;

async transition increment_accumulator() -> Future {
    return increment_state_onchain();
}
async function increment_accumulator_onchain(){
    let current_count: u64 = accumulator.get_or_use(0u8, 0u64);
    let new_count: u64 = current_count + 1u64;
    accumulator.set(0u8, new_count);

}
```

```leo title="Async Block:"
mapping accumulator: u8 => u64;

async transition increment_accumulator() -> Future {
    let f : Future = async {
        let current_count: u64 = accumulator.get_or_use(0u8, 0u64);
        let new_count: u64 = current_count + 1u64;
        accumulator.set(0u8, new_count);
    }
    return f;
}
```


### Modules

For maximal code cleanliness and readability, take full advantage of Leo's module system:
```
src
├── constants.leo
├── utils.leo
├── structs.leo
└── main.leo
```
With the above structure, consider the following:
- Move all `const`s to the `constants.leo` module
- Move all `inline` functions to the `utils.leo` module
- Move some `struct`s to modules (but this may not make sense in the general case)

The goal is to only have the interface of the program in `main.leo`. Every function should correspond to something than can be called from an external context such as another program.  Note that there is no impact on final program size since modules are flattened into a single program eventually anyways.

##  Layout

### Indentation
4 spaces per indentation level.

### Blank lines

A single blank line should separate the top-level declarations in a `program` scope,
namely `transition`, `function`, `struct`, `record`, and `mapping` declarations.
Multiple imports can be optionally separated by a single blank line;
the last import at the top of the file should be followed by a blank line.

```leo title="Yes:"
import std.io.Write;
import std.math.Add;

program prog.aleo {

    struct A {
        // ...
    }

    function foo() {
        // ...
    }

}
```

```leo title="No:"
import std.io.Write;


import std.math.Add;
program prog.aleo {
    struct A {
        // ...
    }
    function foo() {
        // ...
    }
}
```

### Naming Conventions

| Item                      | Convention                          |
|---------------------------|-------------------------------------|
| Packages                  | snake_case (but prefer single word) |
| Structs and Records       | CamelCase                           |
| Struct and Record Members | snake_case                          |
| Functions                 | snake_case                          |
| Function Parameters       | snake_case                          |
| Variables                 | snake_case                          |
| Inputs                    | snake_case                          |

### Layout
Leo file elements should be ordered:
1. Imports
2. Program declaration
3. Mappings
4. Records + Structs
5. Functions + Transitions


### Braces
Opening braces always go on the same line.
```leo
struct A {
    // ...
}

transition main() {
    // ...
}

let a: A = A { };
```

### Semicolons
Every statement including the `return` statement should end in a semicolon.
```leo
let a: u32 = 1u32;
let b: u32 = a + 5u32;
b *= 2u32;

return b;
```

### Commas
Trailing commas should be included whenever the closing delimiter appears on a separate line.
```leo
let a: A = A { x: 0, y: 1 };

let a: A = A {
    x: 0,
    y: 1,
};
```

## Contributing

Thank you for helping make Leo better!

Before contributing, please view the [Contributor Code of Conduct](https://github.com/ProvableHQ/leo/blob/mainnet/CONTRIBUTING.md).
By participating in this project - In the issues, pull requests, or Gitter channels -
you agree to abide by the terms.

### Report an Issue

To report an issue, please use the [GitHub issues tracker](https://github.com/ProvableHQ/leo/issues). When reporting issues, please mention the following details:

- Which version of Leo you are using.
- What was the source code (if applicable).
- Which platform are you running on.
- How to reproduce the issue.
- What was the result of the issue.
- What the expected behavior is.

Reducing the source code that caused the issue to a bare minimum is always very helpful and sometimes clarifies a misunderstanding.

### Make a Pull Request

Start by forking off of the `mainnet` branch to make your changes. Commit messages should clearly explain why and what you changed.

If you need to pull in any changes from the `mainnet` branch after making your fork (for example, to resolve potential merge conflicts),
please avoid using git merge and instead, git rebase your branch. Rebasing will help us review your changes easily.

#### Tools Required

To build Leo from source you will need the following tools:
- The latest Rust stable version and nightly version.
    - Recommend that you install multiple versions using `rustup`.
- Cargo
    - Rusty Hook install via `cargo install rusty-hook`.
- Clippy
    - Via rustup, if you didn't do the default rustup install `rustup component add clippy`.

#### Formatting

Please do the following before opening a PR.
- `cargo +nightly fmt --all` will format all your code.
- `cargo clippy --all-features --examples --all --benches`

#### Tests

If your code adds new functionality, please write tests to confirm the new features function as expected. Refer to existing tests for examples of how tests are expected to be written. Please read refer to the [parser tests section](#parser-tests). To run the tests please use the following command `cargo test --all --features ci_skip --no-fail-fast`.

##### **Parser Tests**

In the root directory of the repository, there is a "tests" directory.
To add a parser test, look at the Example Leo files in the parser sub-directory.
Then when running the test command, make sure you have the environment variable `CLEAR_LEO_TEST_EXPECTATIONS` set to true. For example, on a UNIX environment, you could run the following command `CLEAR_LEO_TEST_EXPECTATIONS=true cargo test --all --features ci_skip --no-fail-fast`.

#### Grammar

[The `grammars` repository](https://github.com/ProvableHQ/grammars) contains a file [`leo.abnf`](https://github.com/ProvableHQ/grammars/blob/master/leo.abnf) that has the Leo grammar rules in the ABNF format.
If your changes affect a grammar rule, we may ask you to modify it in that `.abnf` file.

We appreciate your hard work!
