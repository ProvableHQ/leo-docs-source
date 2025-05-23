---
id: test_framework
title: Leo's Native Testing Framework
sidebar_label: Testing
---

## Introduction
The Leo testing frameworks enables developers to validate their Leo program logic by writing unit and integration tests. Tests are written in Leo and are located in a `tests/` subdirectory of the main Leo project directory.

```bash
example_program
├── build
│   ├── imports
│   │   └── test_example_program.aleo
│   ├── main.aleo
│   └── program.json
├── outputs
├── src
│   └── main.leo
├── tests
│   └── test_example_program.leo
└── program.json
```
The test file is a Leo program that imports the program in `main.leo`.  The test functions will all be annotated with `@test` above the function declaration. 

This tutorial will use an example program which can be found in the [example's repository](https://github.com/ProvableHQ/leo-examples/tree/main/tests).  

:::info
Developers can add multiple `leo` files to the test directory but must ensure that the name of the test file matches the test program name.  For example, if the name of the test file is `test_example_program.leo`, the test program name must be `test_example_program.aleo`.
:::


## Testing transition function logic.

The `example_program.leo` program contains a transition function which returns the sum of two `u32` inputs.

```Leo
transition simple_addition(public a: u32, b: u32) -> u32 {
    let c: u32 = a + b;
    return c;
}
```

The `test_example_program.leo` contains two tests to ensure that the transition logic returns a correct output and fails when the output does not match the sum of the input values.
```Leo
@test
transition test_simple_addition() {
    let result: u32 = example_program.aleo/simple_addition(2u32, 3u32);
    assert_eq(result, 5u32);
}
```

The `@should_fail` annotation should be added after the `@test` annotation for tests that are expected to fail.
```Leo
@test
@should_fail
transition test_simple_addition_fail() {
    let result: u32 = example_program.aleo/simple_addition(2u32, 3u32);
    assert_eq(result, 3u32);
}
```

Developers can test that Record and struct fields match their expected values.

```Leo
@test
transition test_record_maker() {
    let r: example_program.aleo/Example = example_program.aleo/mint_record(0field);
    assert_eq(r.x, 0field);
}
```

:::info
Each test file is required to have at least one transition function.
:::


## Using interpreted tests for modelling on-chain state
While the testing framework cannot access on-chain state from either `testnet` or `mainnet`, developers can simulate on-chain state using interpreted tests. Developers are able to await `Future`s and update mappings using interpreted tests. When using interpreted tests, the `transition` or `function` keyword is replaced with the `script` keyword.

```Leo
@test
script test_async() {
    const VAL: field = 12field;
    let fut: Future = example_program.aleo/set_mapping(VAL);
    fut.await();
    assert_eq(Mapping::get(example_program.aleo/map, 0field), VAL);

    let rand_val: field = ChaCha::rand_field();
    Mapping::set(example_program.aleo/map, VAL, rand_val);
    let value: field = Mapping::get(example_program.aleo/map, VAL);
    assert_eq(value, rand_val);
}
```

:::info
While non-async logic may be tested using an interpreted tests, all async functionality must be tested within interpreted tests.
:::


## Running tests
Invoking the `leo test` command will run all of the compiled and interpreted tests. Developer may optionally select individual tests by supplying the function test name as follows:
```bash
leo test test_async
```
