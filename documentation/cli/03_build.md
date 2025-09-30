---
id: cli_build
title: leo build
sidebar_label: Build
---
[general tags]: # (cli, leo_build, build, compile, program)

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

