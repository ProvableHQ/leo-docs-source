---
id: dependencies 
title: Dependency Management
sidebar_label: Dependency Management
---
[general tags]: # (guides, dependency, dependency_management, imports, program)

## Leo Imports
In your `main.leo` file, specify any imported dependencies using the `import` keyword before the program declaration:
```leo
import credits.aleo;

program test.aleo {
...
}
```

From the root of your Leo program directory, use the `leo add` command to update the `program.json` manifest to add dependencies.

## Deployed Programs
When adding a deployed program as a dependency to your program, such as the `credits.aleo`, use the following command::

```
leo add credits.aleo
```
or
```
leo add credits
```

If you are deploying to mainnet, you will need to specify mainnet imports using the `--network` flag as follows:

```
leo add credits --network mainnet
```

For the first imported dependency, a new `dependencies` field will be added to the 'package.json` manifest:

```json
{
  "program": "your_program.aleo",
  "version": "0.0.0",
  "description": "",
  "license": "MIT",
  "dependencies": [
    {
      "name": "credits.aleo",
      "location": "network",
      "network": "testnet",
      "path": null
    }
  ]
}
```

Dependencies can be removed using the `leo remove` command:
```bash
leo remove credits.aleo
```

## Local Development
When deploying to a local devnet, specify the path for the local dependency as follows:

```
leo add program_name.aleo --local ./path_to_dependency
```
The dependencies section in the `program.json` manifest should include the path:
```json
{
  "program": "your_program.aleo",
  "version": "0.0.0",
  "description": "",
  "license": "MIT",
  "dependencies": [
    {
      "name": "local_dependency.aleo",
      "location": "local",
      "network": null,
      "path": "./path"
    }
  ]
} 
```

## Recursive Deployment
When deploying a program that uses local dependencies, use the following command:
```bash
leo deploy --recursive
```
All local dependency will be deployed in order, followed by the main program.  Deployed dependencies will be skipped.

## How Dependencies are Resolved

**Local dependencies** (`location: local`) are compiled from Leo source code in the specified path. They never require network access.

**Network dependencies** (`location: network`) are fetched as bytecode from the Aleo network. Leo caches these in `~/.aleo/registry/{network}/{program_name}/{edition}/` to avoid repeated downloads.

### Caching Behavior

Different commands handle caching differently:

| Command | Cache Behavior |
|---------|---------------|
| `leo build` | Uses cache (fetches only if not cached) |
| `leo run` | Uses cache |
| `leo execute` | Always fetches fresh from network |
| `leo deploy` | Always fetches fresh from network |
| `leo upgrade` | Always fetches fresh from network |
| `leo synthesize` | Always fetches fresh from network |

Commands that generate proofs or interact with the blockchain (`execute`, `deploy`, `upgrade`, `synthesize`) always fetch fresh bytecode to ensure consistency with the current network state.

To force a fresh fetch during `build` or `run`, use the `--no-cache` flag:
```bash
leo build --no-cache
```

### Editions

An **edition** is the version number of a deployed program on the Aleo network. Edition 0 is the initial deployment, and the edition is incremented by 1 each time the program is upgraded.

By default, Leo fetches the **latest** edition of a network dependency. To pin a dependency to a specific edition:
```bash
leo add some_program.aleo --edition 3
```

This records the pinned edition in the manifest:
```json
{
  "name": "some_program.aleo",
  "location": "network",
  "network": "testnet",
  "path": null,
  "edition": 3
}
```

Pinning is useful when you need reproducible builds or want to avoid breaking changes if a dependency is upgraded.