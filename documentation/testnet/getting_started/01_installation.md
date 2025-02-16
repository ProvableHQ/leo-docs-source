---
id: installation
title: Installation snarkOS
sidebar_label: Installation
---

<!-- markdown-link-check-disable -->

## <a name='TableofContents'></a>Table of Contents

* [1. Overview](#1-overview)
* [2. Build Guide](#2-build-guide)
  * [2.1 Requirements](#21-requirements)
  * [2.2 Installation](#22-installation)
* [3. Run an Aleo Node](#3-run-an-aleo-node)
  * [3a. Run an Aleo Client](#3a-run-an-aleo-client)
  * [3b. Run an Aleo Prover](#3a-run-an-aleo-prover)
* [4. FAQs](#4-faqs)
* [5. Command Line Interface](#5-configuration-file)
* [6. Development Guide](#6-development-guide)
  * [6.1 Quick Start](#61-quick-start)
  * [6.2 Operations](#61-operations)
* [7. License](#7-license)


## 1. Overview

__snarkOS__ is a decentralized operating system for zero-knowledge applications.
This code forms the backbone of [Aleo](https://aleo.org/) network,
which verifies transactions and stores the encrypted state applications in a publicly-verifiable manner.

## 2. Build Guide

### 2.1 Requirements

The following are **minimum** requirements to run an Aleo node:

### Clients

- **OS**: 64-bit architectures only, latest up-to-date for security, Ubuntu 20.04, macOS Ventura or later, Windows 11 or later
- **CPU**: 64-bit architectures only 32-cores
- **RAM**: DDR4 or better, 32GB of memory
- **Storage**: PCIe Gen 3 x4, PCIe Gen 4 x2 NVME SSD, or better, 300GB of disk space
- **Network**: Symmetric, commercial, always-on, 100Mbps of upload **and** download bandwidth
- **GPU**: Not required at this time

### Provers

- **OS**: 64-bit architectures only, latest up-to-date for security, Ubuntu 20.04, macOS Ventura or later
- **CPU**: 64-bit architectures only, 32-cores (64-cores preferred)
- **RAM**: DDR4 or better, 32GB of memory (64GB or larger preferred)
- **Storage**: PCIe Gen 3 x4, PCIe Gen 4 x2 NVME SSD, or better, 32GB of disk space
- **Network**: Symmetric, commercial, always-on, 500Mbps of upload **and** download bandwidth
- **GPU**: CUDA-enabled GPU (optional)

### Validators

 - **OS**: 64-bit architectures only, latest up-to-date for security, Validators: Ubuntu 20.04
 - **CPU**: 64-bit architectures only, 32-cores (64-cores preferred)
 - **RAM**: DDR4 or better, 64GB of memory (128GB or larger preferred)
 - **Storage**: PCIe Gen 3 x4, PCIe Gen 4 x2 NVME SSD, or better, 2TB of disk space (4TB or larger preferred)
 - **Network**: Symmetric, commercial, always-on, 1000Mbps of upload **and** download bandwidth
- **GPU**: Not required at this time

Please note to run an Aleo Prover that is **competitive**, the machine will require more than these requirements.

### 2.2 Installation

Before beginning, please ensure your machine has `Rust v1.76+` installed. Instructions to [install Rust can be found here.](https://www.rust-lang.org/tools/install)

Start by cloning this Github repository:
```
git clone https://github.com/ProvableHQ/snarkOS.git --depth 1
```

Next, move into the `snarkOS` directory:
```
cd snarkOS
```

**[For Ubuntu users]** A helper script to install dependencies is available. From the `snarkOS` directory, run:
```
./build_ubuntu.sh
```

Lastly, install `snarkOS`:
```
cargo install --path .
```

Please ensure ports `4130/tcp` and `3030/tcp` are open on your router and OS firewall.

## 3. Run an Aleo Node

## 3a. Run an Aleo Client

Start by following the instructions in the [Build Guide](#2-build-guide).

Next, to start a client node, from the `snarkOS` directory, run:
```
./run-client.sh
```

## 3b. Run an Aleo Prover

Start by following the instructions in the [Build Guide](#2-build-guide).

Next, generate an Aleo account address:
```
snarkos account new
```
This will output a new Aleo account in the terminal.

**Please remember to save the account private key and view key.** The following is an example output:
```
 Attention - Remember to store this account private key and view key.

  Private Key  APrivateKey1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx  <-- Save Me And Use In The Next Step
     View Key  AViewKey1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx  <-- Save Me
      Address  aleo1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx  <-- Save Me
```

Next, to start a proving node, from the `snarkOS` directory, run:
```
./run-prover.sh
```
When prompted, enter your Aleo private key:
```
Enter the Aleo Prover account private key:
APrivateKey1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

## 4. FAQs

### 1. My node is unable to compile.

- Ensure your machine has `Rust v1.66+` installed. Instructions to [install Rust can be found here.](https://www.rust-lang.org/tools/install)
- If large errors appear during compilation, try running `cargo clean`.
- Ensure `snarkOS` is started using `./run-client.sh` or `./run-prover.sh`.

### 2. My node is unable to connect to peers on the network.

- Ensure ports `4130/tcp` and `3030/tcp` are open on your router and OS firewall.
- Ensure `snarkOS` is started using `./run-client.sh` or `./run-prover.sh`.

### 3. I can't generate a new address ###

- Before running the command above (`snarkos account new`) try `source ~/.bashrc`
- Also double-check the spelling of `snarkos`. Note the directory is `/snarkOS`, the command is `snarkos`

## 5. Command Line Interface

To run a node with custom settings, refer to the full list of options and flags available in the `snarkOS` CLI.

The full list of CLI flags and options can be viewed with `snarkos --help`:
```
snarkOS
The Aleo Team <hello@aleo.org>

USAGE:
    snarkos [OPTIONS] <SUBCOMMAND>

OPTIONS:
    -h, --help                     Print help information
    -v, --verbosity <VERBOSITY>    Specify the verbosity [options: 0, 1, 2, 3] [default: 2]

SUBCOMMANDS:
    account    Commands to manage Aleo accounts
    clean      Cleans the snarkOS node storage
    help       Print this message or the help of the given subcommand(s)
    start      Starts the snarkOS node
    update     Update snarkOS
```

The following are the options for the `snarkos start` command:
```
USAGE:
    snarkos start [OPTIONS]

OPTIONS:
        --network <NETWORK_ID>                  Specify the network ID of this node [default: 3]

        --validator                             Specify this node as a validator
        --prover                                Specify this node as a prover
        --client                                Specify this node as a client

        --private-key <PRIVATE_KEY>             Specify the node's account private key
        --private-key-file <PRIVATE_KEY_FILE>   Specify the path to a file containing the node's account private key

        --node <IP:PORT>                        Specify the IP address and port for the node server [default: 0.0.0.0:4130]
        --connect <IP:PORT>                     Specify the IP address and port of a peer to connect to

        --rest <REST>                           Specify the IP address and port for the REST server [default: 0.0.0.0:3030]
        --norest                                If the flag is set, the node will not initialize the REST server

        --nodisplay                             If the flag is set, the node will not render the display
        --verbosity <VERBOSITY_LEVEL>           Specify the verbosity of the node [options: 0, 1, 2, 3] [default: 2]
        --logfile <PATH>                        Specify the path to the file where logs will be stored [default: /tmp/snarkos.log]

        --dev <NODE_ID>                         Enables development mode, specify a unique ID for this node
        -h, --help                              Print help information
```

## 6. Development

### 6.1 Quick Start

In the first terminal, start the first validator by running:
```
cargo run --release -- start --nodisplay --dev 0 --validator
```
In the second terminal, start the second validator by running:
```
cargo run --release -- start --nodisplay --dev 1 --validator
```
In the third terminal, start the third validator by running:
```
cargo run --release -- start --nodisplay --dev 2 --validator
```
In the fourth terminal, start the fourth validator by running:
```
cargo run --release -- start --nodisplay --dev 3 --validator
```

From here, this procedure can be used to further start-up provers and clients.

### 6.2 Operations

It is important to initialize the nodes starting from `0` and incrementing by `1` for each new node.

The following is a list of options to initialize a node (replace `<NODE_ID>` with a number starting from `0`):
```
cargo run --release -- start --nodisplay --dev <NODE_ID> --validator
cargo run --release -- start --nodisplay --dev <NODE_ID> --prover
cargo run --release -- start --nodisplay --dev <NODE_ID> --client
cargo run --release -- start --nodisplay --dev <NODE_ID>
```

When no node type is specified, the node will default to `--client`.

### 6.3 Local Devnet

#### 6.3.1 Install `tmux`

To run a local devnet with the script, start by installing `tmux`.

<details><summary>macOS</summary>

To install `tmux` on macOS, you can use the `Homebrew` package manager.
If you haven't installed `Homebrew` yet, you can find instructions at [their website](https://brew.sh/).
```bash
# Once Homebrew is installed, run:
brew install tmux
```

</details>

<details><summary>Ubuntu</summary>

On Ubuntu and other Debian-based systems, you can use the `apt` package manager:
```bash
sudo apt update
sudo apt install tmux
```

</details>

<details><summary>Windows</summary>

There are a couple of ways to use `tmux` on Windows:

### Using Windows Subsystem for Linux (WSL)

1. First, install [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install).
2. Once WSL is set up and you have a Linux distribution installed (e.g., Ubuntu), open your WSL terminal and install `tmux` as you would on a native Linux system:
```bash
sudo apt update
sudo apt install tmux
```

</details>

#### 6.3.2 Start a Local Devnet

To start a local devnet, run:
```
./devnet.sh
```
Follow the instructions in the terminal to start the devnet.

#### 6.3.3 View a Local Devnet

#### Switch Nodes (forward)

To toggle to the next node in a local devnet, run:
```
Ctrl+b n
```

#### Switch Nodes (backwards)

To toggle to the previous node in a local devnet, run:
```
Ctrl+b p
```

#### Select a Node (choose-tree)

To select a node in a local devnet, run:
```
Ctrl+b w
```

#### Select a Node (manually)

To select a node manually in a local devnet, run:
```
Ctrl+b :select-window -t {NODE_ID}
```

#### 6.3.4 Stop a Local Devnet

To stop a local devnet, run:
```
Ctrl+b :kill-session
```
Then, press `Enter`.

### Clean Up

To clean up the node storage, run:
```
cargo run --release -- clean --dev <NODE_ID>
```

## 7. License

We welcome all contributions to `snarkOS`. Please refer to the [license](#7-license) for the terms of contributions.

<!-- markdown-link-check-enable -->
