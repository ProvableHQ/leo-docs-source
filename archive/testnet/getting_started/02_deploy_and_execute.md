---
id: deploy_execute
title: Aleo Deploy and Execute
sidebar_label: Deploy and Execute Programs
---

These changes support the first iteration of deploying and executing programs on the Aleo network.

*Bugs, usability suggestions, and feedback in general would be greatly appreciated.*


## Overview

Deployment and execution of programs is done via four new `developer` CLI commands in `snarkOS` - `decrypt`, `deploy`, `execute`, `scan`, and `transfer`.

These CLI commands currently live in snarkOS, but can also be migrated to the Aleo SDK.

*Note: All the operations are done client-side and do not require sending private keys or view keys to third parties.*

## Usage guide

### 1. Install snarkOS

```
git clone https://github.com/ProvableHQ/snarkOS.git
cd snarkOS
git checkout mainnet-staging
cargo install --path .
```

### 2. Run the node in development mode

```
snarkos start --nodisplay --dev <NODE_ID>
```

### 3. Scan the node for spendable records

This will likely be the view key associated with the development beacon.

```
snarkos developer scan -v <VIEW_KEY> --start 0 --end 1 --endpoint "http://localhost:3030"
```

### 4. Transfer credits (execute the `credits.aleo` program)

Transfer credits to another account.

```
snarkos developer execute credits.aleo transfer <INPUT_RECORD> <RECIPIENT_ADDRESS> "<AMOUNT_TO_TRANSFER>u64" --private-key <PRIVATE_KEY> --query "http://localhost:3030" --broadcast "http://localhost:3030/testnet/transaction/broadcast"
```

or

```
snarkos developer transfer <AMOUNT_TO_TRANSFER> --input-record <INPUT_RECORD> --recipient <RECIPIENT_ADDRESS>  --private-key <PRIVATE_KEY> --query "<http://localhost:3030>" --broadcast "<http://localhost:3030/testnet/transaction/broadcast>"
```

This step can be replaced with a faucet.

### 5. Deploy a program

Deploy a program with an unspent record.


```
snarkos developer deploy fibonacci.aleo --private-key <PRIVATE_KEY> --query "http://localhost:3030" --path "../leo/examples/fibonacci/build/" --broadcast "http://localhost:3030/testnet/transaction/broadcast" --fee 600000 --record <INPUT_RECORD>
```


### 6. Execute a function of a deployed program

```
snarkos developer execute fibonacci.aleo fibonacci "1u8" --private-key <PRIVATE_KEY> --query "http://localhost:3030" --broadcast "http://localhost:3030/testnet/transaction/broadcast"
```

*NOTE: Fees (in microcredits) must be greater than the transaction size in bytes. Fees can be excluded from execution transactions, but if one is specified, it must follow the above rule. *

## Commands

### Decrypt

Decrypt a record ciphertext using a view key.

##### Arguments:

- `ciphertext`- The record ciphertext to decrypt
    - shortcut: `-c`
- `view_key` - The view key used to decrypt the ciphertext
    - shortcut: `-v`

##### Example:
```
snarkos developer decrypt  -v <VIEW_KEY> -c <RECORD_CIPHERTEXT>
```

### Deploy

Create an Aleo program deployment.

##### Arguments:

- `program_id` - The ID of the program to deploy
- `path` - The path to the package directory
    - optional - defaults to the current directory
- `private_key` - The private key used to generate the deployment
    - shortcut `-p`
- `query` - The endpoint to query node state from
    - shortcut - `-q`
- `fee` - The deployment fee in microcredits
    - optional - defaults to 0
- `record` - The record to spend the fee from
- `display` - Display the generated transaction
    - optional - defaults to false
- `broadcast` - Broadcast the transaction to a specified endpoint
    - optional
- `store` - Store the transaction to a specified file path.
    - optional

##### Example:

```
snarkos developer deploy fibonacci.aleo --private-key <PRIVATE_KEY> --query "http://localhost:3030" --path "./leo/examples/fibonacci/build/" --broadcast "http://localhost:3030/testnet/transaction/broadcast" --fee 550000 --record <INSERT_RECORD_HERE>
```

### Execute

Create an Aleo program execution.

##### Arguments:

- `program_id` - The ID of the program to deploy
- `function` - The name of the function
- `inputs` - The function inputs
- `private_key` - The private key used to generate the deployment
    - shortcut `-p`
- `query` - The endpoint to query node state from
    - shortcut - `-q`
- `fee` - The deployment fee in microcredits
    - optional
- `record` - The record to spend the fee from
    - optional
- `display` - Display the generated transaction
    - optional - defaults to false
- `broadcast` - Broadcast the transaction to a specified endpoint
    - optional
- `store` - Store the transaction to a specified file path.
    - optional

##### Example:

```
snarkos developer execute fibonacci.aleo fibonacci "1u8" --private-key <PRIVATE_KEY> --query "http://localhost:3030" --broadcast "http://localhost:3030/testnet/transaction/broadcast"
```

### Scan

Scan a node for owned records in a given block range.

Note: This is a naive scan and doesn't take spent records into account.

##### Argument:

- `view_key` - The view key used to decrypt found records
    - shortcut: `-v`
- `start` - The starting block height of the query
- `end` - The end block height of the query
- `endpoint` - Endpoint to fetch blocks from

##### Example:

```
snarkos developer scan -v <VIEW_KEY> --start 0 --end 1 --endpoint "http://localhost:3030"

```

### Transfer

Transfer credits with a `credits.aleo` program execution.

##### Arguments:

- `input_record` - The record used to craft the transfer
- `recipient` - The recipient address
- `amount` - The number of microcredits to transfer
- `private_key` - The private key used to generate the deployment
    - shortcut `-p`
- `query` - The endpoint to query node state from
    - shortcut - `-q`
- `fee` - The deployment fee in microcredits
    - optional
- `record` - The record to spend the fee from
    - optional
- `display` - Display the generated transaction
    - optional - defaults to false
- `broadcast` - Broadcast the transaction to a specified endpoint
    - optional
- `store` - Store the transaction to a specified file path.
    - optional

##### Example:

```
snarkos developer transfer <AMOUNT_TO_TRANSFER> --input-record <INPUT_RECORD> --recipient <RECIPIENT_ADDRESS>  --private-key <PRIVATE_KEY> --query "<http://localhost:3030>" --broadcast "<http://localhost:3030/testnet/transaction/broadcast>"

```


## Usage on Testnet Beta

To deploy and execute programs on Testnet Beta
<!-- markdown-link-check-disable -->

1. Replace [step 3](#3-scan-the-node-for-spendable-records) with the Aleo faucet to obtain spendable credits. You can request credits from [the faucet](https://faucet.aleo.org/)
2. Replace the use of `http://localhost:3030` with `https://api.explorer.aleo.org/v1`

<!-- markdown-link-check-enable -->


Deployment transactions have an additional requirement where the included fee must have at least `deployment_size_in_bytes` microcredits.

Execution transactions do not currently have any fee requirements.

*If you'd like to try out deploying an Aleo app, you can follow the demo in the next section, [Deploy and Execute Demo](./03_deploy_and_execute_demo.md).*
