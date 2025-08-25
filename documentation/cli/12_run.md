---
id: cli_run
title: leo run
sidebar_label: Run
---

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


