---
id: cli
title: The Leo Command Line Interface
sidebar_label: CLI 
---

The Leo CLI is a command line interface tool that comes equipped with the Leo compiler.

:::tip
You can print the list of commands by running `leo --help`
:::

## Commands

* [`account`](#leo-account) - Create a new Aleo account, sign and verify messages.
  * [`new`](#leo-account-new) - Generates a new Aleo account.
  * [`import`](#leo-account-import) - Derive and Aleo account from a private key.
  * [`sign`](#leo-account-sign) - Sign a message using your Aleo private key.
  * [`verify`](#leo-account-verify) - Verify a message and signature from an Aleo address.
* [`add`](#leo-add) - Add a new onchain or local dependency to the current project.
* [`build`](#leo-build) - Compile the current project.
* [`clean`](#leo-clean) - Clean the build and output artifacts.
* [`debug`](#leo-debug) - Run the interactive debugger in the current package.
* [`deploy`](#leo-deploy) - Deploy a program to the Aleo network.
* [`devnet`](#leo-devnet) - Initialize a local devnet.
* [`execute`](#leo-execute) - Execute a program and produce a transaction containing a proof.
* [`new`](#leo-new) - Create a new Leo project in a new directory.
* [`query`](#leo-query) - Query live data and state from the Aleo network.
  * [`block`](#leo-query) - Query block information.
  * [`transaction`](#leo-query) - Query transaction information.
  * [`program`](#leo-query) - Query program source code and live mapping values.
  * [`stateroot`](#leo-query) - Query the latest stateroot. 
  * [`committee`](#leo-query) - Query the current committee.
  * [`mempool`](#leo-query) - Query transactions and transmissions from the memory pool.
  * [`peers`](#leo-query) - Query peer information.
* [`remove`](#leo-remove) - Remove a dependency from the current project.
* [`run`](#leo-run) - Run a program without producing a proof.
* [`test`](#leo-test) -  Run the test cases for a Leo project.
* [`update`](#leo-update) - Update to the latest version of Leo.
* [`upgrade`](#leo-upgrade) - Upgrade a deployed program on the Aleo network.

### Universal Flags:
These flags are available to use alongside all commands in the Leo CLI.

#### `-h`
#### `--help`
Prints available commands and flags.

#### `-V`
#### `--version`
Prints the currently installed version of Leo.

#### `-q`
Suppresses the CLI output.

#### `-d`
Prints out additional information for debugging if possible.

#### `--path <PATH>`
Specifies the path to Leo program root folder.  Defaults to `./`.

#### `--home <HOME>`  
Specifies the path to the `.aleo` program registry.  This is where programs downloaded from the network will be cached.  Defaults to `~/.aleo/registry`.


&nbsp;

---


## `leo account`

[Back to Top](#commands)

The `leo account` command is used to create and manage Aleo accounts, as well as sign and verify messages

:::warning
We urge you to exercise caution when managing your private keys. `leo account` cannot be used to recover lost keys.
:::

---


### `leo account new`

Use this command to generate a private key, view key, and address for a new Aleo account.


The output should look like this:
```bash title="console output:"
  Private Key  APrivateKey1zkp...
     View Key  AViewKey1...
      Address  aleo1...
```


### Flags:
#### `--seed <SEED>`
#### `-s <SEED>`
Specifies a particular numeric value to use as the seed for the random number generator (RNG)



#### `--write `
#### `-w`
Writes the generated private key to a `.env` file in the current working directory (`./`)



#### `--discreet`
Print sensitive information (such as private key) discreetly to an alternate screen


---

### `leo account import`


Use this command to derive the view key and address for an Aleo account from a private key.


To import an existing Aleo account, run the following command:
```bash
leo account import <PRIVATE_KEY>
```
where `<PRIVATE_KEY>` is your private key.

### Flags:
#### `--write `
#### `-w`
Writes the generated private key to a `.env` file in the current working directory (`./`)



#### `--discreet`
Print sensitive information (such as private key) discreetly to an alternate screen

---

### `leo account sign`
Use this command to sign a message using your Aleo private key.

Assuming either the current working directory is a Leo project or the `$PRIVATE_KEY` environment variable has been set, you can sign a message using the following command:
```bash
leo account sign --message <MESSAGE>
```

### Flags:
#### `--message <MESSAGE>`
#### `-m <MESSAGE>`

:::info
This flag is required!
:::

Specifies the message to be signed.

---

#### `--private-key <PRIVATE_KEY>`
Explicity specifies the private key to sign the message with.  Overrides any private key in `.env` file or `$PRIVATE_KEY` environment variable.


#### `--private-key-file <PRIVATE_KEY_FILE>`
Alternative way to explicity specifies the private key by reading from a text file at path `<PRIVATE_KEY_FILE>` Overrides any private key in `.env` file or `$PRIVATE_KEY` environment variable.


#### `--raw `
#### `-r`
Parses the message as bytes instead of Aleo literals.

---

### `leo account verify`

Use this command to verify a message and signature from an Aleo address.

To verify a message, run the following command
```bash
leo account verify --address <ADDRESS> --signature <SIGNATURE> --message <MESSAGE>
```
where `<MESSAGE>` is the message, `<SIGNATURE>` is the signature of that message, and `<ADDRESS>` is the address of the account that generated the signature.

### Flags:
#### `--address <ADDRESS>`
#### `-a <ADDRESS>`

:::info
This flag is required!
:::

Specifies the address of the account that generated the signature.


#### `--signature <SIGNATURE>`
#### `-s <SIGNATURE>`

:::info
This flag is required!
:::

Specifies the signature of the message.


#### `--message <MESSAGE>`
#### `-m <MESSAGE>`

:::info
This flag is required!
:::

Specifies the message that was signed.


#### `--raw `
#### `-r`
Parses the message as bytes instead of Aleo literals.


&nbsp;

---

## `leo add`
[Back to Top](#commands)

The `leo add` command is used to add a new on-chain or local dependency to the current Leo project.


To add a local dependency to your project, run the following command:
```bash
leo add --local <LOCAL> <NAME>
```
where `<NAME>` is the name of the imported program, and `<LOCAL>` is the path to the local project.

&nbsp;

To add a program already deployed onchain as a dependency to your project, run the following command:
```bash
leo add --network <NAME>
```
where `<NAME>` is the name of the imported program.

### Flags:
#### `--local <LOCAL> `
#### `-l <LOCAL>`
Specifies that the dependency to be added is a local program located at path `<LOCAL>`.  This can either be the root directory for a Leo project, or a path directly to an already compiled `.aleo` file.


#### `--network`
#### `-n `
Specifies that the dependency to be added is a remote program currently deployed onchain. The network that it will be pulled from will be the same as the one specified in by the `NETWORK` variable in `.env`



#### `--edition <EDITION> `
#### `-e <EDITION>`
Specifies the expected edition of the program being imported. Only passing this flag will assume that the program is being imported from the network. 

:::warning
Do not use this feature unless you know what you are doing!
:::



#### `--dev`
Specifies that the imported program is a development dependency and should not be used in production



#### `-c`
#### `--clear`
Clears all previous dependencies.

:::warning
This feature is currently bugged and is non-functional.
:::

&nbsp;

---

## `leo build`

[Back to Top](#commands)

To compile your program into Aleo instructions and verify that it builds properly, run:
```bash
leo build
```
 
On invoking the build command, Leo automatically creates a `build/⁠` and `output/`⁠ folder in the project directory. The compiled `.aleo` file is contained in the `build` directory. The `output` directory is used to stored intermediate artifacts from compilation. 

```bash title="console output:"
  Leo     2 statements before dead code elimination.
  Leo     2 statements after dead code elimination.
  Leo     The program checksum is: '[...]'.
  Leo ✅ Compiled '{PROGRAM_NAME}.aleo' into Aleo instructions.
```

### Flags:
```
--offline
    Enables offline mode.
--enable-ast-spans
    Enable spans in AST snapshots.
--enable-dce
    Enables dead code elimination in the compiler.
--conditional-block-max-depth <CONDITIONAL_BLOCK_MAX_DEPTH>
    Max depth to type check nested conditionals. [default: 10]
--disable-conditional-branch-type-checking
    Disable type checking of nested conditional branches in finalize scope.
--enable-initial-ast-snapshot
    Write an AST snapshot immediately after parsing.
--enable-all-ast-snapshots
    Writes all AST snapshots for the different compiler phases.
--ast-snapshots <AST_SNAPSHOTS>...
    Comma separated list of passes whose AST snapshots to capture.
--build-tests
    Build tests along with the main program and dependencies.
--no-cache
    Don't use the dependency cache.
--no-local
    Don't use the local source code.
--private-key <PRIVATE_KEY>
    The private key to use for the deployment. Overrides the `PRIVATE_KEY` environment variable.
--network <NETWORK>
    The network to deploy to. Overrides the `NETWORK` environment variable.
--endpoint <ENDPOINT>
    The endpoint to deploy to. Overrides the `ENDPOINT` environment variable.
--devnet
    Whether the network is a devnet. If not set, defaults to the `DEVNET` environment variable.
--consensus-heights <CONSENSUS_HEIGHTS>
    Optional consensus heights to use. This should only be set if you are using a custom devnet.
```

&nbsp;

---


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

---


## `leo debug`
[Back to Top](#commands)

To start the interactive debugger, run `leo debug` in a Leo project.

```bash
> leo debug
       Leo ✅ Compiled sources for 'workshop'
This is the Leo Interpreter. Try the command `#help`.

? Command? › 
```

### Cheatsheet

<!--TODO: Rewrite this cheatsheet to present the information in a more condensed form.-->

```
You probably want to start by running a function or transition.
For instance
#into program.aleo/main()
Once a function is running, commands include
#into    to evaluate into the next expression or statement;
#step    to take one step towards evaluating the current expression or statement;
#over    to complete evaluating the current expression or statement;
#run     to finish evaluating
#quit    to quit the interpreter.

You can set a breakpoint with
#break program_name line_number

When executing Aleo VM code, you can print the value of a register like this:
#print 2

Some of the commands may be run with one letter abbreviations, such as #i.

Note that this interpreter is not line oriented as in many common debuggers;
rather it is oriented around expressions and statements.
As you step into code, individual expressions or statements will
be evaluated one by one, including arguments of function calls.

You may simply enter Leo expressions or statements on the command line
to evaluate. For instance, if you want to see the value of a variable w:
w
If you want to set w to a new value:
w = z + 2u8;

Note that statements (like the assignment above) must end with a semicolon.

If there are futures available to be executed, they will be listed by
numerical index, and you may run them using `#future` (or `#f`); for instance
#future 0

The interpreter begins in a global context, not in any Leo program. You can set
the current program with

#set_program program_name

This allows you to refer to structs and other items in the indicated program.

The interpreter may enter an invalid state, often due to Leo code entered at the
REPL. In this case, you may use the command

#restore

Which will restore to the last saved state of the interpreter. Any time you
enter Leo code at the prompt, interpreter state is saved.

Input history is available - use the up and down arrow keys.
```

See [Debugging](./guides/08_debugging.md) for more details.

&nbsp;

## `leo deploy`
[Back to Top](#commands)
:::note
This command requires having a funded account. 
:::

To deploy the project in the current working directory, run the following command:

```bash
leo deploy # Defaults to using the private key, network, and endpoint in .env
```


```bash title="sample output:"
       Leo     ... statements before dead code elimination.
       Leo     ... statements after dead code elimination.
       Leo     The program checksum is: '[...]'.
       Leo ✅ Compiled '{PROGRAM_NAME}.aleo' into Aleo instructions.

📢 Using the following consensus heights: 0,2950000,4800000,6625000,6765000,7600000,8365000,9173000,9800000
  To override, pass in `--consensus-heights` or override the environment variable `CONSENSUS_VERSION_HEIGHTS`.

Attempting to determine the consensus version from the latest block height at {ENDPOINT}...

🛠️  Deployment Plan Summary
──────────────────────────────────────────────
🔧 Configuration:
  Private Key:        {PRIVATE_KEY}
  Address:            {ADDRESS}
  Endpoint:           {ENDPOINT}
  Network:            {NETWORK}
  Consensus Version:  {CONSENSUS_VERSION}

📦 Deployment Tasks:
  • {PROGRAM_NAME}.aleo  │ priority fee: 0  │ fee record: no (public fee)

⚙️ Actions:
  • Transaction(s) will NOT be printed to the console.
  • Transaction(s) will NOT be saved to a file.
  • Transaction(s) will be broadcast to {ENDPOINT}

🔧 Your program '{PROGRAM_NAME}.aleo' has the following constructor.
──────────────────────────────────────────────
constructor:
    ...
──────────────────────────────────────────────
Once it is deployed, it CANNOT be changed.

📦 Creating deployment transaction for '{PROGRAM_NAME}.aleo'...


📊 Deployment Summary for '{PROGRAM_NAME}.aleo'
──────────────────────────────────────────────
  Total Variables:      ...
  Total Constraints:    ...
  Max Variables:        2,097,152
  Max Constraints:      2,097,152

💰 Cost Breakdown (credits)
  Transaction Storage:  ...
  Program Synthesis:    ...
  Namespace:            ...
  Constructor:          ...
  Priority Fee:         ...
  Total Fee:            ...
──────────────────────────────────────────────
```

See the **[Deploying](01_deploying.md)** guide for more details.


### Flags:

#### `--private-key <PRIVATE_KEY>`
Specifies the private key to use for the deployment. Overrides any `$PRIVATE_KEY` environment variable set manually or in a `.env` file.

#### `--network <NETWORK>`
Specifies the network to deploy to. Overrides any `NETWORK` environment variable set manually or in a `.env` file.  Valid network names are `testnet`, `mainnet`, and `canary`.


#### `--endpoint <ENDPOINT>`
The endpoint to deploy to. Overrides any `ENDPOINT` environment variable set manually or in a `.env` file.

**Common Endpoints:**

| Network |  Endpoint  |
|:---------:|:------:|
| Devnet (local)  | https://localhost:3030 | 
| Testnet  | https://api.explorer.provable.com/v1| 
| Mainnet  | https://api.explorer.provable.com/v1| 


#### `--devnet`
Specifies whether the network being deployed to is a devnet. If not set, defaults to the `DEVNET` environment variable.

:::info
This flag requires a devnet to be running locally.  See INSERT DEVNET GUIDE HERE for more information
:::


#### `-print`
Prints the transaction to the terminal/stdout in JSON format.

#### `-broadcast`
Broadcasts the transaction to the network upon successful execution.  Without passing this flag, the transaction will just be generated locally.

#### `--save <SAVE>`
Saves the transaction to the directory located at the `<SAVE>` path.

#### `--skip <SUBSTRING_0> <SUBSTRING_1> ...`
Skips deployment of any program that contains one of the given substrings, delimited by a space.

#### `-y`
#### `--yes`
The CLI will ask for manual confirmation on several steps throughout the deployment process.  Setting this flag automatically agrees to all confirmations.

:::warning
Do not use this feature unless you know what you are doing!
:::

#### `--priority-fees <PRIORITY_FEES>`
Specifes the priority fee for the deployment transaction(s) delimited by `|` and used in order. The fees are in microcredits and must either be valid `u64` or `default`. Defaults to 0.

:::tip
1 Credit == 1,000,000 Microcreditss
:::


#### `-f <FEE_RECORDS>`
#### `--fee-records <FEE_RECORDS>`

Specifes the record(s) to pay for fees privately, delimited by `|` and used in order. The fees must either be valid plaintext, ciphertext, or `default`.  If not specified, then transaction fees will be public.


#### `--consensus-heights <CONSENSUS_HEIGHTS>`
Specifies the consensus heights to use, delimited by `,`. This should only be set if you are using a custom devnet.

The following will enable Consensus_V0 at block 0, Consensus_V1 at block 1, etc.:
```bash
--consensus-heights 0,1,2,3....
```


#### `--consensus-version <CONSENSUS_VERSION>`
Specifies the consensus version to use. If one is not provided, the CLI will attempt to determine it from the latest block.

#### `--max-wait <MAX_WAIT>`
Specifies the number of seconds to wait for a block to appear when searching for a transaction. Defaults to 8 seconds.

#### `--blocks-to-check <BLOCKS_TO_CHECK>`
Specifies the number of blocks to look at when searching for a transaction.  Defaults to 12 blocks

```
Options:
--base-fees <BASE_FEES>
  [UNUSED] Base fees in microcredits, delimited by `|`, and used in order. The fees must either be valid `u64` or `default`. Defaults to automatic calculation.
--skip <SKIP>...
  Skips deployment of any program that contains one of the given substrings.
--offline
    Enables offline mode.
--enable-ast-spans
    Enable spans in AST snapshots.
--enable-dce
    Enables dead code elimination in the compiler.
--conditional-block-max-depth <CONDITIONAL_BLOCK_MAX_DEPTH>
    Max depth to type check nested conditionals. [default: 10]
--disable-conditional-branch-type-checking
    Disable type checking of nested conditional branches in finalize scope.
--enable-initial-ast-snapshot
    Write an AST snapshot immediately after parsing.
--enable-all-ast-snapshots
    Writes all AST snapshots for the different compiler phases.
--ast-snapshots <AST_SNAPSHOTS>...
    Comma separated list of passes whose AST snapshots to capture.
--build-tests
    Build tests along with the main program and dependencies.
--no-cache
    Don't use the dependency cache.
--no-local
    Don't use the local source code.
```


&nbsp;

---

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

### Flags:
#### `--snarkos <SNARKOS>`
Specifies the path to the installed snarkOS binary.

:::info
This flag is required!
:::

#### `--snarkos-features <FEATURES>`
Specifies which features of snarkOS to use (e.g. `test_network`)

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


#### `--network <NETWORK_ID>`
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

---

## `leo execute`
[Back to Top](#commands)
:::note
This command requires having a funded account. 
:::

To execute a Leo transition function with inputs from the command line, run the following command:
```bash
leo execute <TRANSITION_NAME> <INPUTS>
```
where `<TRANSITION_NAME>` is the name of the `transition` or `async transition` function to execute and `<INPUTS>`is a list of inputs to the program separated by spaces.

Under the hood, this command synthesizes the program circuit and generates proving and verifying keys.


```bash title="sample output:"
       Leo     ... statements before dead code elimination.
       Leo     ... statements after dead code elimination.
       Leo     The program checksum is: '[...]'.
       Leo ✅ Compiled '{PROGRAM_NAME}.aleo' into Aleo instructions.

📢 Using the following consensus heights: 0,2950000,4800000,6625000,6765000,7600000,8365000,9173000,9800000
  To override, pass in `--consensus-heights` or override the environment variable `CONSENSUS_VERSION_HEIGHTS`.

Attempting to determine the consensus version from the latest block height at {ENDPOINT}...

🚀 Execution Plan Summary
──────────────────────────────────────────────
🔧 Configuration:
  Private Key:        {PRIVATE_KEY}
  Address:            {ADDRESS}
  Endpoint:           {ENDPOINT}
  Network:            {NETWORK}
  Consensus Version:  {CONSENSUS_VERSION}

🎯 Execution Target:
  Program:        {PROGRAM_NAME}.aleo
  Function:       {FUNCTION_NAME}
  Source:         remote

💸 Fee Info:
  Priority Fee:   {PRIORITY_FEE} μcredits
  Fee Record:     no (public fee) | {FEE RECORD}

⚙️ Actions:
  - Program and its dependencies will be downloaded from the network.
  - Transaction will NOT be printed to the console.
  - Transaction will NOT be saved to a file.
  - Transaction will NOT be broadcast to the network.

📊 Execution Summary for {PROGRAM_NAME}.aleo
──────────────────────────────────────────────
💰 Cost Breakdown (credits)
  Transaction Storage:  ...
  On‑chain Execution:   ...
  Priority Fee:         ...
  Total Fee:            ...
──────────────────────────────────────────────

➡️  Outputs

 • {OUTPUT_0}
 • {OUTPUT_1}
 ...
```


See the **[Executing](./02_executing.md)** guide for more details.


### Flags:

#### `--private-key <PRIVATE_KEY>`
Specifies the private key to use for the deployment. Overrides any `$PRIVATE_KEY` environment variable set manually or in a `.env` file.

#### `--network <NETWORK>`
Specifies the network to deploy to. Overrides any `NETWORK` environment variable set manually or in a `.env` file.  Valid network names are `testnet`, `mainnet`, and `canary`.


#### `--endpoint <ENDPOINT>`
The endpoint to deploy to. Overrides any `ENDPOINT` environment variable set manually or in a `.env` file.

**Common Endpoints:**

| Network |  Endpoint  |
|:---------:|:------:|
| Devnet (local)  | https://localhost:3030 | 
| Testnet  | https://api.explorer.provable.com/v1| 
| Mainnet  | https://api.explorer.provable.com/v1| 


#### `--devnet`
Specifies whether the network being deployed to is a devnet. If not set, defaults to the `DEVNET` environment variable.

:::info
This flag requires a devnet to be running locally.  See INSERT DEVNET GUIDE HERE for more information
:::


#### `-print`
Prints the transaction to the terminal/stdout in JSON format.

#### `-broadcast`
Broadcasts the transaction to the network upon successful execution.  Without passing this flag, the transaction will just be generated locally.

#### `--save <SAVE>`
Saves the transaction to the directory located at the `<SAVE>` path.

#### `-y`
#### `--yes`
The CLI will ask for manual confirmation on several steps throughout the deployment process.  Setting this flag automatically agrees to all confirmations.

:::warning
Do not use this feature unless you know what you are doing!
:::

#### `--priority-fees <PRIORITY_FEES>`
Specifes the priority fee for the deployment transaction(s) delimited by `|` and used in order. The fees are in microcredits and must either be valid `u64` or `default`. Defaults to 0.

:::tip
1 Credit == 1,000,000 Microcreditss
:::


#### `-f <FEE_RECORDS>`
#### `--fee-records <FEE_RECORDS>`

Specifes the record(s) to pay for fees privately, delimited by `|` and used in order. The fees must either be valid plaintext, ciphertext, or `default`.  If not specified, then transaction fees will be public.


#### `--consensus-heights <CONSENSUS_HEIGHTS>`
Specifies the consensus heights to use, delimited by `,`. This should only be set if you are using a custom devnet.

The following will enable Consensus_V0 at block 0, Consensus_V1 at block 1, etc.:
```bash
--consensus-heights 0,1,2,3....
```


#### `--consensus-version <CONSENSUS_VERSION>`
Specifies the consensus version to use. If one is not provided, the CLI will attempt to determine it from the latest block.

#### `--max-wait <MAX_WAIT>`
Specifies the number of seconds to wait for a block to appear when searching for a transaction. Defaults to 8 seconds.

#### `--blocks-to-check <BLOCKS_TO_CHECK>`
Specifies the number of blocks to look at when searching for a transaction.  Defaults to 12 blocks

```
Options:
--base-fees <BASE_FEES>
  [UNUSED] Base fees in microcredits, delimited by `|`, and used in order. The fees must either be valid `u64` or `default`. Defaults to automatic calculation.
--offline
    Enables offline mode.
--enable-ast-spans
    Enable spans in AST snapshots.
--enable-dce
    Enables dead code elimination in the compiler.
--conditional-block-max-depth <CONDITIONAL_BLOCK_MAX_DEPTH>
    Max depth to type check nested conditionals. [default: 10]
--disable-conditional-branch-type-checking
    Disable type checking of nested conditional branches in finalize scope.
--enable-initial-ast-snapshot
    Write an AST snapshot immediately after parsing.
--enable-all-ast-snapshots
    Writes all AST snapshots for the different compiler phases.
--ast-snapshots <AST_SNAPSHOTS>...
    Comma separated list of passes whose AST snapshots to capture.
--build-tests
    Build tests along with the main program and dependencies.
--no-cache
    Don't use the dependency cache.
--no-local
    Don't use the local source code.
```


&nbsp;

---


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

The `leo query` command is used to get data from a network supporting the canonical `snarkOS` endpoints.

---


### `leo query block`

To fetch blocks from a given network, run the following command
```bash
leo query <ID>
```
where `<ID>` is either a specific block height or block hash.  The block will be returned in JSON format.

For example, you can fetch the Mainnet genesis block by running either of the following commands:
```bash
leo query block 0 --network mainnet --endpoint https://api.explorer.provable.com/v1
```
```bash
leo query block ab1sm6kyqle2ftg4z8gegafqrjy0jwjhzu6fmy73726dgszrtxhxvfqha0eee --network mainnet --endpoint https://api.explorer.provable.com/v1
```



### Flags:

#### `--network <NETWORK>`
Specifies the network to deploy to. Overrides any `NETWORK` environment variable set manually or in a `.env` file.  Valid network names are `testnet`, `mainnet`, and `canary`.


#### `--endpoint <ENDPOINT>`
The endpoint to deploy to. Overrides any `ENDPOINT` environment variable set manually or in a `.env` file.


#### `--latest`
#### `-l`
Queries the latest block.

```bash title="Example:"
leo query block --latest
```


#### `--latest-hash`
Queries the hash of the latest block.

```bash title="Example:"
leo query block --latest-hash
```


#### `--latest-height`
Queries the height of the latest block

```bash title="Example:"
leo query block --latest-height
```

#### `--range <START_HEIGHT> <END_HEIGHT>`
#### `-r <START_HEIGHT> <END_HEIGHT>`
Queries up to 50 consecutive blocks.

```bash title="Example:"
leo query block --range <START_HEIGHT> <END_HEIGHT>
```


#### `--transactions`
#### `-t`
Queries all transactions at the specified block height
```bash title="Example:"
leo query block <BLOCK_HEIGHT> --transactions
```


#### `--to-height`
Queries the block height corresponding to a hash value
```bash title="Example:"
leo query block <BLOCK_HASH> --to-height
```

---

### `leo query transaction`

To fetch a specific transaction from a given network, run the following command:

```bash
leo query transaction <ID>
```
where `<ID>` is the ID of the transaction.  The transaction will be returned in JSON format.

### Flags:

#### `--network <NETWORK>`
Specifies the network to deploy to. Overrides any `NETWORK` environment variable set manually or in a `.env` file.  Valid network names are `testnet`, `mainnet`, and `canary`.


#### `--endpoint <ENDPOINT>`
The endpoint to deploy to. Overrides any `ENDPOINT` environment variable set manually or in a `.env` file.


#### `--confirmed`
#### `-c`
Returns information (including details about onchain execution) about the queried transaction if it was confirmed.


#### `--unconfirmed`
#### `-u`
Queries the original (unconfirmed) transaction.


#### `--from-io <INPUT_OR_OUTPUT_ID>`
Get the ID of the transaction that an input or output ID occurred in.
```bash title="Example:"
leo query transaction --from-io <INPUT_OR_OUTPUT_ID>
```


#### `--from-transition <TRANSITION_ID>`
Get the ID of the transaction containing the specified transition.

```bash title="Example:"
leo query transaction --from-transition <TRANSITION_ID>
```


#### `--from-program <PROGRAM_NAME>`
Get the ID of the transaction that the specified program was deployed in.
```bash title="Example:"
leo query transaction --from-program <PROGRAM_NAME>
```

---

### `leo query program`

To fetch a specific program from a given network, run the following command:

```bash
leo query program <PROGRAM_NAME>
```

You can also use this to query specific mappings and mapping values from a given program.  For example, if you wanted to query your public balance of Aleo credits:

```bash
leo query program credits.aleo --mapping-value account <YOUR_ADDRESS>
```


### Flags:

#### `--network <NETWORK>`
Specifies the network to deploy to. Overrides any `NETWORK` environment variable set manually or in a `.env` file.  Valid network names are `testnet`, `mainnet`, and `canary`.


#### `--endpoint <ENDPOINT>`
The endpoint to deploy to. Overrides any `ENDPOINT` environment variable set manually or in a `.env` file.


#### `--mappings`
Lists all mappings defined in the latest deployed edition of the program.


#### `--mapping-value <MAPPING> <KEY> `
Get the value corresponding to the specified mapping and key.  Will return `null` if key is not present in mapping.

#### `--edition <EDITION>`
An optional edition to query for when fetching the program source. If not specified, the latest edition will be used.

The edition of the program is set to `0` upon initial deployment and is incremented by `1` each time the program is upgraded.  See the **[Upgrading Programs](./../guides/03_program_upgradability.md)** guide for more details.


---

### `leo query stateroot`

This command queries the latest stateroot of a given network. 

### Flags:

#### `--network <NETWORK>`
Specifies the network to deploy to. Overrides any `NETWORK` environment variable set manually or in a `.env` file.  Valid network names are `testnet`, `mainnet`, and `canary`.


#### `--endpoint <ENDPOINT>`
The endpoint to deploy to. Overrides any `ENDPOINT` environment variable set manually or in a `.env` file.

---

### `leo query committee`

This command queries the current validator committee for a given network. 

### Flags:

#### `--network <NETWORK>`
Specifies the network to deploy to. Overrides any `NETWORK` environment variable set manually or in a `.env` file.  Valid network names are `testnet`, `mainnet`, and `canary`.


#### `--endpoint <ENDPOINT>`
The endpoint to deploy to. Overrides any `ENDPOINT` environment variable set manually or in a `.env` file.

---

### `leo query mempool`

This command queries transactions and transmissions from the memory pool for a node on a given network.

:::note
This command can only be run with a custom endpoint.  Using the official Provable API endpoint will fail.
:::

To query transactions:
```bash
leo query mempool --transactions
```

To query transmissions:
```bash
leo query mempool --transmissions
```


### Flags:

#### `--transactions`
Queries the transactions in the memory pool.


#### `--transmissions`
Queries the transactions in the memory pool.



#### `--network <NETWORK>`
Specifies the network to deploy to. Overrides any `NETWORK` environment variable set manually or in a `.env` file.  Valid network names are `testnet`, `mainnet`, and `canary`.


#### `--endpoint <ENDPOINT>`
The endpoint to deploy to. Overrides any `ENDPOINT` environment variable set manually or in a `.env` file.

---

### `leo query peers`

This command queries peer information for a node on a given network.

:::note
This command can only be run with a custom endpoint. Using the official Provable API endpoint will fail.
:::

### Flags:

#### `--metrics`
#### `-m`          
Queries all peer metrics


#### `--count`
#### `-c`             
Queries the count of all participating peers

#### `--network <NETWORK>`
Specifies the network to deploy to. Overrides any `NETWORK` environment variable set manually or in a `.env` file.  Valid network names are `testnet`, `mainnet`, and `canary`.


#### `--endpoint <ENDPOINT>`
The endpoint to deploy to. Overrides any `ENDPOINT` environment variable set manually or in a `.env` file.


&nbsp;

---

## `leo remove`
[Back to Top](#commands)

To remove a dependency from your project, run the following command:

```bash
leo remove <NAME>
```
where `<NAME>` is the name of the imported program.

See the **[Dependency Management](./04_dependencies.md)** guide for more details.

### Flags:

#### `--all`
Removes all dependencies (or dev dependencies, if used with --dev).

#### `-dev`
Removes dev dependencies.


&nbsp;

## `leo run`
[Back to Top](#commands)

:::tip
Use this command to run your program before [**executing**](#leo-execute) it.
:::

To run a Leo transition function with inputs from the command line, run the following command:
```bash
leo run <TRANSITION_NAME> <INPUTS>
```

where `<TRANSITION_NAME>` is the name of the `transition` or `async transition` function to run and `<INPUTS>` is a list of inputs to the program separated by spaces.

This command does not synthesize the program circuit or generate proving and verifying keys.

```bash title="sample output:"
       Leo     ... statements before dead code elimination.
       Leo     ... statements after dead code elimination.
       Leo     The program checksum is: '[...]'.
       Leo ✅ Compiled '{PROGRAM_NAME}.aleo' into Aleo instructions.

⛓  Constraints

 •  '{PROGRAM_NAME}.aleo/{FUNCTION_NAME}' - ... constraints (called 1 time)

➡️  Outputs

 • {OUTPUT_0}
 • {OUTPUT_1}
 ...
```

If one or more of your inputs are negatives, and consequently begin with a `-`,
you may separate the inputs with a `--` so that the command line parser
won't attempt to parse them as options:
```bash
leo run <TRANSITION_NAME> -- <INPUT_0> -- <INPUT_1> ...
```

### Flags:
```
--offline
    Enables offline mode.
--enable-ast-spans
    Enable spans in AST snapshots.
--enable-dce
    Enables dead code elimination in the compiler.
--conditional-block-max-depth <CONDITIONAL_BLOCK_MAX_DEPTH>
    Max depth to type check nested conditionals. [default: 10]
--disable-conditional-branch-type-checking
    Disable type checking of nested conditional branches in finalize scope.
--enable-initial-ast-snapshot
    Write an AST snapshot immediately after parsing.
--enable-all-ast-snapshots
    Writes all AST snapshots for the different compiler phases.
--ast-snapshots <AST_SNAPSHOTS>...
    Comma separated list of passes whose AST snapshots to capture.
--build-tests
    Build tests along with the main program and dependencies.
--no-cache
    Don't use the dependency cache.
--no-local
    Don't use the local source code.
--private-key <PRIVATE_KEY>
    The private key to use for the deployment. Overrides the `PRIVATE_KEY` environment variable.
--network <NETWORK>
    The network to deploy to. Overrides the `NETWORK` environment variable.
--endpoint <ENDPOINT>
    The endpoint to deploy to. Overrides the `ENDPOINT` environment variable.
--devnet
    Whether the network is a devnet. If not set, defaults to the `DEVNET` environment variable.
--consensus-heights <CONSENSUS_HEIGHTS>
    Optional consensus heights to use. This should only be set if you are using a custom devnet.
```

&nbsp;

----


## `leo test`

This command runs all the test cases specified in the Leo file in `tests/`.  

If you want to run a specific set of tests, run the following command:
```bash
leo test <TEST_NAME>
```
where `<TEST_NAME>` is the string to match against the qualified name of each test.

Check out the [**Testing**](./guides/07_testing.md) guide for more information.


### Flags:
```
--offline
    Enables offline mode.
--enable-ast-spans
    Enable spans in AST snapshots.
--enable-dce
    Enables dead code elimination in the compiler.
--conditional-block-max-depth <CONDITIONAL_BLOCK_MAX_DEPTH>
    Max depth to type check nested conditionals. [default: 10]
--disable-conditional-branch-type-checking
    Disable type checking of nested conditional branches in finalize scope.
--enable-initial-ast-snapshot
    Write an AST snapshot immediately after parsing.
--enable-all-ast-snapshots
    Writes all AST snapshots for the different compiler phases.
--ast-snapshots <AST_SNAPSHOTS>...
    Comma separated list of passes whose AST snapshots to capture.
--build-tests
    Build tests along with the main program and dependencies.
--no-cache
    Don't use the dependency cache.
--no-local
    Don't use the local source code.
```


&nbsp;

----

## `leo update`
[Back to Top](#commands)

To download and install the latest Leo version run:

```bash
leo update
```

```bash title="console output:"
Checking target-arch... aarch64-apple-darwin
Checking current version... v3.1.0
Checking latest released version... v3.1.0
       Leo 
Leo is already on the latest version
```

### Flags:
#### `-l`
#### `--list`
Lists all available versions of Leo.


&nbsp;

## `leo upgrade`
[Back to Top](#commands)

Upgrades a program that is already deployed on the network.

See the **[Upgrading Programs](./../guides/03_program_upgradability.md)** guide for more details.


### Flags:

#### `--private-key <PRIVATE_KEY>`
Specifies the private key to use for the deployment. Overrides any `$PRIVATE_KEY` environment variable set manually or in a `.env` file.

#### `--network <NETWORK>`
Specifies the network to deploy to. Overrides any `NETWORK` environment variable set manually or in a `.env` file.  Valid network names are `testnet`, `mainnet`, and `canary`.


#### `--endpoint <ENDPOINT>`
The endpoint to deploy to. Overrides any `ENDPOINT` environment variable set manually or in a `.env` file.

**Common Endpoints:**

| Network |  Endpoint  |
|:---------:|:------:|
| Devnet (local)  | https://localhost:3030 | 
| Testnet  | https://api.explorer.provable.com/v1| 
| Mainnet  | https://api.explorer.provable.com/v1| 


#### `--devnet`
Specifies whether the network being deployed to is a devnet. If not set, defaults to the `DEVNET` environment variable.

:::info
This flag requires a devnet to be running locally.  See INSERT DEVNET GUIDE HERE for more information
:::


#### `-print`
Prints the transaction to the terminal/stdout in JSON format.

#### `-broadcast`
Broadcasts the transaction to the network upon successful execution.  Without passing this flag, the transaction will just be generated locally.

#### `--save <SAVE>`
Saves the transaction to the directory located at the `<SAVE>` path.

#### `-y`
#### `--yes`
The CLI will ask for manual confirmation on several steps throughout the deployment process.  Setting this flag automatically agrees to all confirmations.

:::warning
Do not use this feature unless you know what you are doing!
:::

#### `--priority-fees <PRIORITY_FEES>`
Specifes the priority fee for the deployment transaction(s) delimited by `|` and used in order. The fees are in microcredits and must either be valid `u64` or `default`. Defaults to 0.

:::tip
1 Credit == 1,000,000 Microcreditss
:::


#### `-f <FEE_RECORDS>`
#### `--fee-records <FEE_RECORDS>`

Specifes the record(s) to pay for fees privately, delimited by `|` and used in order. The fees must either be valid plaintext, ciphertext, or `default`.  If not specified, then transaction fees will be public.


#### `--consensus-heights <CONSENSUS_HEIGHTS>`
Specifies the consensus heights to use, delimited by `,`. This should only be set if you are using a custom devnet.

The following will enable Consensus_V0 at block 0, Consensus_V1 at block 1, etc.:
```bash
--consensus-heights 0,1,2,3....
```


#### `--consensus-version <CONSENSUS_VERSION>`
Specifies the consensus version to use. If one is not provided, the CLI will attempt to determine it from the latest block.

#### `--max-wait <MAX_WAIT>`
Specifies the number of seconds to wait for a block to appear when searching for a transaction. Defaults to 8 seconds.

#### `--blocks-to-check <BLOCKS_TO_CHECK>`
Specifies the number of blocks to look at when searching for a transaction.  Defaults to 12 blocks

```
Options:
--base-fees <BASE_FEES>
  [UNUSED] Base fees in microcredits, delimited by `|`, and used in order. The fees must either be valid `u64` or `default`. Defaults to automatic calculation.
--skip <SKIP>...
  Skips the upgrade of any program that contains one of the given substrings.
--offline
    Enables offline mode.
--enable-ast-spans
    Enable spans in AST snapshots.
--enable-dce
    Enables dead code elimination in the compiler.
--conditional-block-max-depth <CONDITIONAL_BLOCK_MAX_DEPTH>
    Max depth to type check nested conditionals. [default: 10]
--disable-conditional-branch-type-checking
    Disable type checking of nested conditional branches in finalize scope.
--enable-initial-ast-snapshot
    Write an AST snapshot immediately after parsing.
--enable-all-ast-snapshots
    Writes all AST snapshots for the different compiler phases.
--ast-snapshots <AST_SNAPSHOTS>...
    Comma separated list of passes whose AST snapshots to capture.
--build-tests
    Build tests along with the main program and dependencies.
--no-cache
    Don't use the dependency cache.
--no-local
    Don't use the local source code.
```



<!-- 
### `leo example`
[Back to Top](#commands)

:::warning
`leo example` has been deprecated and will no longer be supported.
::: -->
