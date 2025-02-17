---
id: commands
title: The snarkVM Command Line Interface
sidebar_label: Commands
---

# List of Commands
:::tip
You can print the list of commands by running `snarkvm --help`
:::

* [snarkvm new](#snarkvm-new)
* [snarkvm build](#snarkvm-build)
* [snarkvm run](#snarkvm-run)
* [snarkvm execute](#snarkvm-execute)
* [snarkvm clean](#snarkvm-clean)
* [snarkvm update](#snarkvm-update)

[//]: # (5. [aleo node]&#40;#5-aleo-node&#41;)
[//]: # (5. [aleo deploy]&#40;#6-aleo-deploy&#41;)

The Aleo private key, view key, and address will be printed to console.
See [`concepts/accounts`](../concepts/00_accounts.md) for more information.

## `snarkvm new`

To create a new package, run:
```bash
snarkvm new {$NAME}
```

Valid package names are snake_case: lowercase letters and numbers separated by underscore.
This command will create a new directory with the given package name.
The new package will have a directory structure as follows:

```bash
package-name/
├── program.json # Your program manifest
├── README.md # Your program description
└── main.leo # Your program file
```

## `snarkvm build`
:::info
This command is deprecated as of snarkVM `v0.14.5`. It will be removed in a future release.
:::
To compile your program and verify that it builds properly, run:
```bash
snarkvm build
```

To compile your program in offline mode run:
```bash
snarkvm build --offline
```

## `snarkvm run`

To run your Aleo program function run:
```bash
snarkvm run {$FUNCTION} {$INPUTS}

// Example
snarkvm run hello 2u32 3u32
```

To run your Aleo program function in offline mode run:
```bash
snarkvm run {$FUNCTION} {$INPUTS} --offline
```

To run your Aleo program function to a specified endpoint run:
```bash
snarkvm run {$FUNCTION} {$INPUTS} --endpoint {$ENDPOINT}
```

## `snarkvm execute`

To execute your Aleo program function run:
```bash
snarkvm execute {$FUNCTION} {$INPUTS}

// Example
snarkvm run hello 2u32 3u32
```

To execute your Aleo program function in offline mode run:
```bash
snarkvm execute {$FUNCTION} {$INPUTS} --offline
```

To execute your Aleo program function to a specified endpoint run:
```bash
snarkvm execute {$FUNCTION} {$INPUTS} --endpoint {$ENDPOINT}
```

## `snarkvm clean`

To clean the Aleo package build directory run:
```bash
snarkvm clean
```

## `snarkvm update`

To update snarkVM to the latest version run:
```
snarkvm update
```

To list the available versions of Aleo run:
```
snarkvm update --list
```

To update snarkVM and suppress outputs to terminal run:
```
snarkvm update --quiet
```
