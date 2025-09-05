---
id: test 
title: Testing, Testing, 123
sidebar_label: Testing
---

Once deployed, an application lives on the ledger forever. Consequently, it's important to consider every edge case and rigorously test your code. There are number of tools and techniques you can use. 

- [**Unit and Integration Testing**](#test-framework) - Validate Leo program logic through test cases.

- [**Running a Devnet**](#running-a-devnet) - Deploy and execute on a local devnet.

- [**Deploying/Executing on Testnet**](#deployingexecuting-on-testnet) - Deploy and execute on the Aleo Testnet.

- [**Other Tools**](#other-tools) - Tools and methodologies developed by the open-source Aleo community.


## Unit and Integration Testing
The Leo testing framework enables developers to validate their Leo program logic by writing unit and integration tests. Tests are written in Leo and are located in a `tests/` subdirectory of the main Leo project directory.

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

This tutorial will use an example program which can be found in the [example's repository](https://github.com/ProvableHQ/leo-examples/tree/main/example_with_test).  

:::info
Developers can add multiple `leo` files to the test directory but must ensure that the name of the test file matches the program name within that test file.  For example, if the name of the test file is `test_example_program.leo`, the program name in that file must be `test_example_program.aleo`.
:::


### Testing `transition` Functions

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

### Testing Leo Types

Developers can test that record and struct fields match their expected values.  In `example_program.leo`, a record is minted transition function shown here:

```Leo
record Example {
    owner: address,
    x: field,
}

transition mint_record(x: field) -> Example {
    return Example {
        owner: self.signer,
        x,
    };
}
```

The corresponding test in `test_example_program.leo` checks that the Record field contains the correct value:

```Leo
@test
transition test_record_maker() {
    let r: example_program.aleo/Example = example_program.aleo/mint_record(0field);
    assert_eq(r.x, 0field);
}
```

:::info
Each test file is required to have at least one `transition` function.
:::


### Modeling Onchain State
While the testing framework cannot access on-chain state from either `testnet` or `mainnet`, developers can simulate on-chain state in `script`s. A script is interpreted Leo code in which developers are able to await `Future`s and update mappings using interpreted tests. When using interpreted tests, the `transition` or `function` keyword is replaced with the `script` keyword.

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
External `transition`s -- `async` or not -- may be called from test `transition`s or scripts, but external `async function`s may only be called directly from scripts.
:::


### Running Tests
Invoking the `leo test` command will run all of the compiled and interpreted tests. Developer may optionally select an individual tests by supplying a a test function name or a string that is contained within a test function name.  For instance, to run the test for `test_async`, developers would use the following command:
```bash
leo test test_async
```
Either of the following commands will run both of the addition function tests:
```bash
leo test simple
```
or
```bash
leo test addition
```

See the `leo test` CLI documentation [here](./../cli/13_test.md)


## Running a Devnet

A local devnet can be a heavyweight but reliable way to test your application on Aleo.

For more information, refer the to [Devnet](./07_devnet.md) guide

## Deploying/Executing on Testnet
To deploy and execute on Testnet, you'll need to set your endpoint back to one of the public facing options. Additionally, you'll need to obtain Testnet credits from one of the faucets below:

### Faucets

<!--TODO: Update this information once the new faucet becomes public.-->

At some point you'll need testnet credits. There are a few community-supported faucets to choose from:
- [**Puzzle**](https://dev.puzzle.online/faucet) - 15 credits / 4 hours

- [**Demox**](https://discord.com/channels/913160862670397510/1202322326230937640/1203135682873266207) - 10 credits / 12 hours

The faucets are periodically refreshed.


## Other Tools

The Aleo community has developed some neat tools to aid in testing.

- [**doko.js**](https://github.com/venture23-aleo/doko-js)

