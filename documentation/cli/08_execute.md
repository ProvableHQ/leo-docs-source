---
id: cli_execute
title: leo execute
sidebar_label: Execute
---

:::note
This command requires having a funded account. 
:::

You can execute a transition function by using the `leo execute` command.  This differs from the `leo run` command in that a transaction and proof of execution are produced, and optionally can be broadcasted to the network.

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


See the **[Executing](./../guides/04_executing.md)** guide for more details.


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

