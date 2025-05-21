---
id: test_framework
title: Leo's Native Testing Framework
sidebar_label: Testing
---

## Introduction
The Leo testing frameworks enables developers to validate their Leo program logic by writing unit and integration tests. Tests are written in Leo and are located in a `/tests` subdirectory of the main Leo project directory.

```bash
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
The test file is a Leo program that imports the program in `main.leo`.  The test functions will all be annotated with `@test` above the the function declaration. 

This tutorial will use an example program which can be found in the [example's repository](https://github.com/ProvableHQ/leo-examples/tree/main/tests).  

:::info
The name of the test file must match the program name.  For example, if the name of the test file is `test_example_program.leo`, the program name should be `test_example_program.aleo`.
:::


## Testing transition function logic.

The `example_program.leo` program contains a transition function which returns the sum of two `u32` inputs.  When testing 

```Leo
transition simple_addition(public a: u32, b: u32) -> u32 {
        let c: u32 = a + b;
        return c;
    }
```

In the `test_example_program.leo`, we can write two tests to ensure that the transition logic returns correct values.
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

## Using interpreted tests for modelling on-chain state
While the testing framework cannot access on-chain state from Testnet or Mainnet, developers can simulate on-chain state using interpreted tests.  Within interpreted tests, developers are able to await Futures and update mappings.  Interpreted tests are annotated with the `script` keyword.

```Leo
script test_async() {
    const VAL: field = 12field;
    let fut: Future = example_program.aleo/set_mapping(VAL);
    // We must await this future for the async code to run.
    fut.await();
    assert_eq(Mapping::get(example_program.aleo/map, 0field), VAL);

    // scripts can also do other things normally only async code can do:
    let rand_val: field = ChaCha::rand_field();
    Mapping::set(example_program.aleo/map, VAL, rand_val);
    let value: field = Mapping::get(example_program.aleo/map, VAL);
    assert_eq(value, rand_val);
}
```






On-chain state is not accessible.
Test file name should match the program name of the test.
Need at least one transition function in the test file.

