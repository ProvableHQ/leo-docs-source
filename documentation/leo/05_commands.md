---
id: commands
title: The Leo Command Line Interface
sidebar_label: Commands
---

The Leo CLI is a command line interface tool that comes equipped with the Leo compiler.

## Installation

[**Install Leo**](01_installation.md)

## Global Flags

* `-d`, `--debug` - Enables debugging mode
* `-h`, `--help` - Prints help information
* `-v`, `--version` - Prints version information

## List of Commands:

:::tip
You can print the list of commands by running `leo --help`
:::

* [`example`](#leo-example) - Create a new Leo package from an example program.
* [`new`](#leo-new) - Create a new Leo package in a new directory.
* [`build`](#leo-build) - Compile the current package as a program.
* [`run`](#leo-run) - Run a program with input variables.
* [`execute`](#leo-execute) - Execute a program with input variables.
* [`add`](#leo-add) - Add a new on-chain or local dependency to the current package.
* [`deploy`](#leo-deploy) - Deploy a program.
* [`clean`](#leo-clean) - Clean the output directory.
* [`update`](#leo-update) - Update to the latest version of Leo.
* [`account`](#leo-account) - Create a new Aleo account.

[//]: # (* [`deploy`]&#40;#leo-deploy&#41; - Deploy a program.)
[//]: # (* [`node`]&#40;#leo-node&#41; - Start a local development server.)

## `leo example`

To list all available example programs, run:
```
leo example

# Output:

Create a new Leo example package in a new directory

Usage: leo example [OPTIONS] <COMMAND>

Commands:
  lottery    A public lottery program
  tictactoe  A standard tic-tac-toe game program
  token      A transparent & shielded custom token program
  help       Print this message or the help of the given subcommand(s)

Options:
  -d                 Print additional information for debugging
  -q                 Suppress CLI output
      --path <PATH>  Optional path to Leo program root folder
  -h, --help         Print help
```

To create a new Leo package from an example program, run:
```bash
leo example {$NAME}
```

## `leo new`

To create a new package, run:
```bash
leo new {$NAME}
```

Valid package names are snake_case: lowercase letters and numbers separated by underscores.
This command will create a new directory with the given package name.
The new package will have a directory structure as follows:

```bash
package/
├── .env # Your program environment
├── program.json # Your program manifest
├── README.md # Your program description
├── build/
└── src/
  └── main.leo # Your program file
```

## `leo build`
:::info
This command is deprecated as of Leo `v1.9.0`. It will be removed in a future release.
:::
To compile your program into Aleo Instructions and verify that it builds properly, run:
```bash
leo build
```

[//]: # (The results of compiling `main.leo` or `lib.leo` and it's imported dependencies will be printed:)

[//]: # (```bash title="console output:")

[//]: # ( Compiling Starting...)

[//]: # ( Compiling Compiling main program... &#40;"${NAME}/src/main.leo"&#41;)

[//]: # ( Compiling Complete)

[//]: # (      Done Finished in 10 milliseconds)

[//]: # (```)
This will populate the directory `build/` (creating it if it doesn't exist) with an Aleo instructions file `.aleo`.
```bash title="console output:"
     Leo ✅ Compiled 'main.leo' into Aleo instructions
```

## `leo run`
:::tip
Use this command to run your program before [**executing**](#leo-execute) it.
:::

To run a Leo transition function with inputs from the command line.
`{$INPUTS}` should be a list of inputs to the program separated by spaces.
```bash
leo run {$TRANSITION} {$INPUTS}
```
This command does not synthesize the program circuit or generate proving and verifying keys.


```bash title="console output:"
 Leo ✅ Compiled 'main.leo' into Aleo instructions

⛓  Constraints
 • 'hello.aleo/main' - 33 constraints (called 1 time)

➡️  Output
 • 3u32
 
 Leo ✅ Finished 'hello.aleo/main' (in "/hello/build")
```

## `leo execute`
:::tip
Use this command to execute your program and generate a transaction object.
:::

To execute a Leo transition function with inputs from the command line.
`{$INPUTS}` should be a list of inputs to the program separated by spaces.
```bash
leo execute {$TRANSITION} {$INPUTS}
```
This command synthesizes the program circuit and generates proving and verifying keys.


```bash title="console output:"
 Leo ✅ Compiled 'main.leo' into Aleo instructions

⛓  Constraints
 • 'hello.aleo/main' - 33 constraints (called 1 time)

➡️  Output
 • 3u32
 
 {"type":"execute","id":"at1 ... (transaction object truncated for brevity)
 
 Leo ✅ Executed 'hello.aleo/main' (in "/hello/build")
```

## `leo add`
:::tip
Use this command to add a dependency to your program. This is a precursor to being able to import a program inside the leo source code file. 
:::

To add an already deployed program as a project dependency.
`{$PROGRAM}` should be the name of the program to add as a dependency.
```bash
leo add {$PROGRAM} // {$NETWORK} defaults to `testnet`.
leo add -n {$NETWORK} {$PROGRAM} // To pull from a custom network. 
```

To add a local Leo program as a project dependency.
`{$PATH}` should be the relative path to the dependency project directory.
```bash
leo add -l {$PATH} {$PROGRAM} 
```

## `leo deploy`
:::tip
Use this command to deploy a program to the Aleo network. This requires having a funded account. 
:::

To deploy the project in the current working directory.
```bash
leo deploy // Defaults to using key information in `.env`, and deploys to `testnet3` via endoint `http://api.explorer.provable.com/v1`.
leo deploy --endpoint "{$ENDPOINT}" --private-key "{$PRIVATE_KEY}" // To deploy using custom private key, to a custom endpoint (e.g. local devnet `http://0.0.0.0:3030`).
```

## `leo clean`

To clean the build directory, run:
```bash
leo clean
```
```bash title="console output:"
  Leo cleaned the build directory (in "/build/")
```

## `leo update`

To download and install the latest Leo version run:

```bash

leo update

```

```bash title="console output:"

Checking target-arch... x86_64-apple-darwin

Checking current version... v1.8.3

Checking latest released version... v1.8.3

  Updating Leo is on the latest version 1.9.0

```

## `leo account`

To create a new Aleo account, run:

```bash
leo account new

# Output:
  Private Key  APrivateKey1zkp...
     View Key  AViewKey1...
      Address  aleo1...
```

To import an existing Aleo account, run:

```bash
leo account import {$PRIVATE_KEY}
```

To create a new account and save it to your .env file, run:

```bash
leo account new --write
````

To list all options

```
leo account --help

# Output:
Create a new Aleo account

Usage: leo account [OPTIONS] <COMMAND>

Commands:
  new     Generates a new Aleo account
  import  Derive an Aleo account from a private key
  help    Print this message or the help of the given subcommand(s)

Options:
  -d                 Print additional information for debugging
  -q                 Suppress CLI output
      --path <PATH>  Optional path to Leo program root folder
  -h, --help         Print help
```
