---
id: cli_devnode
title: ""
sidebar_label: Devnode
toc_min_heading_level: 2
toc_max_heading_level: 2
---

[general tags]: # "cli, leo_devnode, devnode local_devnode"

# `leo devnode`

Leo devnode is a lightweight-node network that bypasses the consensus algorithm and the requirement to generate proofs for deployment and execution transactions. It is a development tool intended for rapidly iterating on Aleo program design and running end-to-end tests prior to deploying Aleo programs to testnet and mainnet.

To initialize devnode, run the following command:

```bash
leo devnode start --private-key <PRIVATE_KEY>
```

<!-- markdown-link-check-disable -->

:::info
The default endpoint for a local devnode is `http://localhost:3030`.
:::

<!-- markdown-link-check-enable -->

Press `Ctrl+C` to stop the devnode. If started with persistent storage (`-l`), the ledger state is saved and can be resumed by running `leo devnode start -l` again. If started without persistent storage, the ledger is discarded when the devnode exits.

## `leo devnode start`

```bash
leo devnode start [OPTIONS]
```

| Flag | Short | Default | Description |
|------|-------|---------|-------------|
| `--private-key` | `-p` | | Private key (required, or set via environment variable) |
| `--verbosity` | `-v` | `2` | Log verbosity level (`0`–`2`) |
| `--socket-addr` | `-a` | `127.0.0.1:3030` | Address and port for the REST API |
| `--genesis-path` | `-g` | (built-in) | Path to a custom genesis block file |
| `--manual-block-creation` | `-m` | `false` | Disable automatic block creation after broadcast |
| `--storage` | `-s` | (in-memory) | Directory for persistent ledger storage. If `-l` is given without a path, defaults to `./devnode/` |
| `--clear-storage` | `-c` | `false` | Clear the ledger directory before starting. Requires `--storage` |

**Examples**

```bash
# In-memory (ephemeral, no persistence)
leo devnode start

# Persistent, default directory (./devnode/)
leo devnode start -s

# Persistent, custom directory
leo devnode start -s ./my-ledger

# Fresh start — wipe existing ledger before starting
leo devnode start -s --c
leo devnode start -s ./my-ledger -c

# Manual block creation (blocks only advance when explicitly requested)
leo devnode start -m
```

## `leo devnode advance`

Manually advances the ledger by creating new blocks. Useful when running with `--manual-block-creation`.

```bash
leo devnode advance [NUM_BLOCKS] [OPTIONS]
```

| Argument / Flag | Default | Description |
|-----------------|---------|-------------|
| `NUM_BLOCKS` | `1` | Number of blocks to create |
| `--socket-addr` | `127.0.0.1:3030` | Address of the running devnode |

**Examples**

```bash
# Advance by 1 block
leo devnode advance

# Advance by 10 blocks
leo devnode advance 10

# Advance a devnode running on a non-default address
leo devnode advance 5 --socket-addr 127.0.0.1:4040
```

## Using Leo CLI Commands with Devnode

When targeting a devnode, two proof-generation steps can be skipped for faster iteration. Proofs are not verified by the devnode, so generating them is unnecessary overhead.

**Deploying a program**

```bash
leo deploy --skip-deploy-certificate --endpoint http://localhost:3030
```

`--skip-deploy-certificate` skips generating the deployment proof. Without it, deployment will still work but will be significantly slower.

**Executing a transition**

```bash
leo execute <TRANSITION> <INPUTS> --skip-execute-proof --endpoint http://localhost:3030
```

`--skip-execute-proof` skips generating the execution proof. Without it, execution will still work but will be significantly slower.

**Upgrading a program**

```bash
leo upgrade --skip-deploy-certificate --endpoint http://localhost:3030
```

## Typical Workflow

```bash
# 1. Start a persistent devnode
leo devnode start -s

# 2. Deploy your program (in another terminal, from your Leo project directory)
leo deploy --skip-deploy-certificate --endpoint http://localhost:3030

# 3. Execute a transition
leo execute <TRANSITION> <INPUTS> --skip-execute-proof --endpoint http://localhost:3030

# 4. Reset and start fresh when needed
leo devnode start -s -c
```
