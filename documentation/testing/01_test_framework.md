---
id: test_framework
title: Leo's Native Testing Framework
sidebar_label: Testing
---

## Introduction
The Leo testing frameworks enables developers to validate their Leo program logic by writing unit and integration tests in Leo. Tests are written in a `/tests` subdirectory of the main Leo project directory.

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

This tutorial will use an example program which can be found in the [example's repository]().  

:::info
The name of the test file must match the program name.  For example, if the name of the test file is `test_example_program.leo`, the program name should be `test_example_program.aleo`.
:::


## Testing transition function logic.

The `example_program.leo` program contains a transition function which adds two `u32` values and returns the sum.

```Leo
transition simple_addition(public a: u32, b: u32) -> u32 {
        let c: u32 = a + b;
        return c;
    }
```





On-chain state is not accessible.
Test file name should match the program name of the test.

