---
id: overview 
title: The Leo Command Line Interface
sidebar_label: Overview 
---

The Leo CLI is a command line interface tool that comes equipped with the Leo compiler.

:::tip
You can print the list of commands by running `leo --help`
:::

## Commands

* [`account`](#leo-account) - Create a new Aleo account, sign and verify messages.
* [`add`](#leo-add) - Add a new on-chain or local dependency to the current project.
* [`build`](#leo-build) - Compile the current project.
* [`clean`](#leo-clean) - Clean the build and output artifacts.
* [`debug`](#leo-debug) - Run the interactive debugger in the current package.
* [`deploy`](#leo-deploy) - Generate proving and verifying keys and produce a transaction containing the deployment.
* [`devnet`](#leo-devnet) - Initialize a local devnet
* [`execute`](#leo-execute) - Execute a program and produce a transaction containing a proof.
* [`new`](#leo-new) - Create a new Leo project in a new directory.
* [`query`](#leo-query) - Retrieve metadata and state from the network.
* [`remove`](#leo-remove) - Remove a dependency from the current project.
* [`run`](#leo-run) - Run a program without producing a proof.
* [`update`](#leo-update) - Update to the latest version of Leo.

---


## `leo account`

[Back to Top](#commands)

To create a new Aleo account, run:

```bash
leo account new
```


```bash title="console output:"
  Private Key  APrivateKey1zkp...
     View Key  AViewKey1...
      Address  aleo1...
```

:::warning
We urge you to exercise caution when managing your private keys. `leo account` cannot be used to recover lost keys.
:::

To import an existing Aleo account, run:

```bash
leo account import <PRIVATE_KEY>
```

To create a new account and save it to your `.env` file, run:

```bash
leo account new --write
````

`leo account` has a number of additional subcommands. To list all options, run:

```bash
leo account --help
```

```bash title="console output:"
Create a new Aleo account, sign and verify messages

Usage: leo account [OPTIONS] <COMMAND>

Commands:
  new     Generates a new Aleo account
  import  Derive an Aleo account from a private key
  sign    Sign a message using your Aleo private key
  verify  Verify a message from an Aleo address
  help    Print this message or the help of the given subcommand(s)

Options:
  -d                 Print additional information for debugging
  -q                 Suppress CLI output
      --path <PATH>  Optional path to Leo program root folder
      --home <HOME>  Optional path to Aleo program registry.
  -h, --help         Print help
```

To learn more about how to use `leo account` to sign and verify messages, check out [**Sign and Verify**](03_signing.md).

&nbsp;

## `leo add`
[Back to Top](#commands)
:::tip
Use this command to add a dependency to your program. This is a precursor to being able to import a program inside the leo source code file. 
:::

The command can be used to add an already deployed program as a project dependency.
`<PROGRAM>` should be the name of the program to add as a dependency.
```bash
leo add <PROGRAM> // <NETWORK> defaults to `testnet`.
leo add -n <NETWORK> <PROGRAM> // To pull from a custom network. 
```

The command can be used to add a local Leo program as a project dependency.
`<PATH>` should be the relative path to the project directory.
```bash
leo add -l <PATH> <PROGRAM> 
```

See [Dependency Management](./04_dependencies.md) for more details.

&nbsp;

## `leo build`

[Back to Top](#commands)

To compile your program into Aleo Instructions and verify that it builds properly, run:
```bash
leo build
```

[//]: # (The results of compiling `main.leo` or `lib.leo` and it's imported dependencies will be printed:)

[//]: # (```bash title="console output:")

[//]: # ( Compiling Starting...)

[//]: # ( Compiling Compiling main program... &#40;"${NAME>/src/main.leo"&#41;)

[//]: # ( Compiling Complete)

[//]: # (      Done Finished in 10 milliseconds)

[//]: # (```)
This will populate the directory `build/` (creating it if it doesn't exist) with an Aleo instructions file `.aleo`.
```bash title="console output:"
     Leo ✅ Compiled 'main.leo' into Aleo instructions
```

&nbsp;

## `leo clean`
[Back to Top](#commands)

To clean the build directory, run:
```bash
leo clean
```
```bash title="console output:"
  Leo 🧹 Cleaned the outputs directory (in "...")
  Leo 🧹 Cleaned the build directory (in "...")
```

&nbsp;

## `leo debug`
[Back to Top](#commands)

To start the interactive debugger, run `leo debug` in a Leo project.

```bash
> leo debug
       Leo ✅ Compiled sources for 'workshop'
This is the Leo Interpreter. Try the command `#help`.

? Command? › 
```

See [Debugging](./../testing/02_debugger.md) for more details.

&nbsp;

## `leo deploy`
[Back to Top](#commands)
:::tip
Use this command to deploy a program to the Aleo network. This requires having a funded account. 
:::

To deploy the project in the current working directory.
```bash
leo deploy // Defaults to using key information in `.env`, and deploys to `testnet3` via endoint `http://api.explorer.provable.com/v1`.
leo deploy --endpoint "<ENDPOINT>" --private-key "<PRIVATE_KEY>" // To deploy using custom private key, to a custom endpoint (e.g. local devnet `http://0.0.0.0:3030`).
```

:::info
The `-b` or `--broadcast` flag is required in order to broadcast a transaction to a network.
:::

:::info
The `-devnet` flag is required if broadcasting to a local devnet.
:::

See [Deploy](01_deploying.md) for more details.

&nbsp;

## `leo devnet`
[Back to Top](#commands)
:::tip
Use this command to initialize a local devnet
:::

:::info
The default ENDPOINT for a local devnet is `http://localhost:3030`
:::

To intialize a local devnet, run the following command:
```bash
leo devnet --snarkos <SNARKOS>
```
where `<SNARKOS>` should be the path to an installed binary of snarkOS.

If you don't have snarkOS installed, you can do so by passing the `--install` flag, which will install the binary at the path specified above.

### Optional Flags:
#### `--snarkos-features <FEATURES>`
Specifies which of snarkOS to use (e.g. `test_network`)

#### `--install`
Installs (or reinstalls) snarkOS at the provided `--snarkos` path with the given `--snarkos-features`.

#### `--snarkos-version <SNARKOS_VERSION>`
Specifies which version of snarkOS to use or install.  Defaults to latest version on [crates.io](https://crates.io/crates/snarkos)

#### `--consensus-heights <CONSENSUS_HEIGHTS> `
Optional blocks heights to use for each successive consensus upgrade.  Must have `--snarkos-features test_network` enabled as well.

The following will enable Consensus_V0 at block 0, Consensus_V1 at block 1, etc.:
```bash
--consensus-heights 0,1,2,3....
```

#### `--storage <STORAGE>`
Root directory path for snarkOS ledgers and logs.  Defaults to `./`

#### `--clear-storage`
Clear existing snarkOS ledgers before starting the devnet


#### `--network <NETWORK>`
Specifies what the network ID of the devnet will be.

| ID |  Network  |
|:---------:|:------:|
| 0  | Mainnet | 
| 1  | Testnet (default)| 
| 2  | Canary | 

#### `--tmux`
Run devnet nodes in tmux (only available on Unix-based systems)

#### `--num-validators <NUM_VALIDATORS>`
Number of validators to use in snarkOS.  Defaults to 4.

#### `--num-clients <NUM_CLIENTS>`
Number of clients to use in snarkOS. Defaults to 2.

#### `--verbosity <VERBOSITY>`
Specifies the verbository of snarkOS (0-4).  Defaults to 1.


&nbsp;

## `leo execute`
[Back to Top](#commands)
:::tip
Use this command to execute your program and generate a transaction object.
:::

To execute a Leo transition function with inputs from the command line.
`<INPUTS>` should be a list of inputs to the program separated by spaces.
```bash
leo execute <TRANSITION> <INPUTS>
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

:::info
The `-b` or `--broadcast` flag is required in order to broadcast a transaction to a network.
:::

:::info
The `-devnet` flag is required if broadcasting to a local devnet.
:::

See [Execute](./02_executing.md) for more details.

&nbsp;

## `leo new`

[Back to Top](#commands)

To create a new project, run:
```bash
leo new <NAME>
```

Valid project names are snake_case: lowercase letters and numbers separated by underscores.
This command will create a new directory with the given project name.

See [Project Layout](../language/01_layout.md) for more details .

&nbsp;

## `leo query`
[Back to Top](#commands)

Use `leo query` to get data from a network supporting the canonical `snarkOS` endpoints.

```bash
Query live data from the Aleo network

Usage: leo query [OPTIONS] <COMMAND>

Commands:
  block        Query block information
  transaction  Query transaction information
  program      Query program source code and live mapping values
  stateroot    Query the latest stateroot
  committee    Query the current committee
  mempool      Query transactions and transmissions from the memory pool
  peers        Query peer information
  help         Print this message or the help of the given subcommand(s)

Options:
  -d                         Print additional information for debugging
  -e, --endpoint <ENDPOINT>  Endpoint to retrieve network state from. Defaults to entry in `.env`.
  -n, --network <NETWORK>    Network to use. Defaults to entry in `.env`.
  -q                         Suppress CLI output
      --path <PATH>          Path to Leo program root folder
      --home <HOME>          Path to Aleo program registry
  -h, --help                 Print help
```



[Back to Top](#commands)
:::tip
Use this command to add a dependency to your program. This is a precursor to being able to import a program inside the leo source code file. 
:::

The command can be used to add an already deployed program as a project dependency.
`<PROGRAM>` should be the name of the program to add as a dependency.
```bash
leo add <PROGRAM> // <NETWORK> defaults to `testnet`.
leo add -n <NETWORK> <PROGRAM> // To pull from a custom network. 
```

The command can be used to add a local Leo program as a project dependency.
`<PATH>` should be the relative path to the project directory.
```bash
leo add -l <PATH> <PROGRAM> 
```

See [Dependency Management](./04_dependencies.md) for more details.

&nbsp;

## `leo remove`
[Back to Top](#commands)

Removes the dependency, local or remote, from the project.

```bash
leo remove credits.aleo
```

See [Dependency Management](./04_dependencies.md) for more details.

&nbsp;

## `leo run`
[Back to Top](#commands)
:::tip
Use this command to run your program before [**executing**](#leo-execute) it.
:::

To run a Leo transition function with inputs from the command line.
`<INPUTS>` should be a list of inputs to the program separated by spaces.
```bash
leo run <TRANSITION> <INPUTS>
```
This command does not synthesize the program circuit or generate proving and verifying keys.

For the default program created by `leo new`, this command
```bash
leo run main 1u32 2u32
```
will generate this output

```bash title="console output:"
 Leo ✅ Compiled 'main.leo' into Aleo instructions

⛓  Constraints
 • 'hello.aleo/main' - 33 constraints (called 1 time)

➡️  Output
 • 3u32
 
 Leo ✅ Finished 'hello.aleo/main' (in "/hello/build")
```

If one or more of your inputs are negatives, and consequently begin with a `-`,
you may separate the inputs with a `--` so that the command line parser
won't attempt to parse them as options:
```bash
leo run <TRANSITION_NAME> -- <INPUT_1> -- <INPUT_2> ...
```

&nbsp;

## `leo update`
[Back to Top](#commands)

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

&nbsp;

## `leo upgrade`
[Back to Top](#commands)

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

<!-- 
### `leo example`
[Back to Top](#commands)

:::warning
`leo example` has been deprecated and will no longer be supported.
::: -->
