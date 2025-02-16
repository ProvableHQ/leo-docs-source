# How To set up SnarkVM

### 1 Install Rust

We recommend installing Rust using [rustup](https://www.rustup.rs/). You can install `rustup` as follows:

- macOS or Linux:
  ```bash
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  ```

- Windows (64-bit):

  Download the [Windows 64-bit executable](https://win.rustup.rs/x86_64) or
  [Windows 32-bit executable](https://win.rustup.rs/i686) and follow the on-screen instructions.

### 1.1 Build from Crates.io

We recommend installing `snarkvm` this way. In your terminal, run:

```bash
cargo install snarkvm
```

Now to use `snarkvm`, in your terminal, run:
```bash
snarkvm
```

### 1.2 Build from Source Code

Alternatively, you can install `snarkvm` by building from the source code as follows:

```bash
# Download the source code
git clone https://github.com/ProvableHQ/snarkvm && cd snarkvm

# Install snarkVM
$ cargo install --path .
```

Now to use `snarkvm`, in your terminal, run:
```bash
snarkvm
```

## 2. Command Line Usage Guide

>You can print the list of commands by running `snarkvm --help`


* [snarkvm new](#snarkvm-new)
* [snarkvm build](#snarkvm-build)
* [snarkvm run](#snarkvm-run)
* [snarkvm execute](#snarkvm-execute)
* [snarkvm clean](#snarkvm-clean)
* [snarkvm update](#snarkvm-update)

[//]: # (5. [aleo node]&#40;#5-aleo-node&#41;)
[//]: # (5. [aleo deploy]&#40;#6-aleo-deploy&#41;)

The Aleo private key, view key, and address will be printed to console.
See [`concepts/accounts`](../concepts/accounts.md) for more information.

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
└── main.aleo # Your program file
```

## `snarkvm run`
First make sure you have created a `.env` file with the following variables:
```bash
NETWORK={$NETWORK}
PRIVATE_KEY={${PASTE_YOUR_PRIVATE_KEY_HERE}}
```

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
