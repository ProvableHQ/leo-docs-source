---
id: blocks
title: Blocks
sidebar_label: Blocks
---

A **block** is a fundamental data structure for organizing Aleo [transactions](03_transactions.md) over time.

## Components of a Block

An Aleo block is serialized in the following format:


|    Parameter     |  Type  |                                        Description                                        |
|:----------------:|:------:|:-----------------------------------------------------------------------------------------:|
|   `block_hash`   | string |                                   The hash of the block                                   |
| `previous_hash`  | string |                              The hash of the previous block                               |
|     `header`     | object |                                  The header of the block                                  |
|  `transactions`  | array  |                               The transactions in the block                               |
| `ratificactions` | array  |                    The ratifications for proving rewards in the block                     |
|    `coinbase`    | string | The coinbase puzzle solution constructed by accumulating the individual prover solutions. |
|   `signature`    | string |                                The signature for the block                                |

#### Transactions

The [**transactions**](./03_transactions.md) are a list of all transactions that are included in the specified block.

#### Block Header

The **block header** contains components that summarize the state of the specified block,
as well as the state of the ledger at this point in history. See the [components of a block header](#components-of-a-block-header)
for more details.

## Components of a Block Header

An Aleo block header is serialized in the following format:

|          Parameter           |  Type  |                                   Description                                   |
|:----------------------------:|:------:|:-------------------------------------------------------------------------------:|
|    `previous_state_root`     | string | The Merkle root representing the blocks in the ledger up to the previous block. |
|     `transactions_root`      | string |           The Merkle root representing the transactions in the block.           |
|       `finalize_root`        | string | The Merkle root representing the on-chain finalize including the current block. |
|     `ratifications_root`     | string |          The Merkle root representing the ratifications in the block.           |
| `coinbase_accumulator_point` | string |                  The accumulator point of the coinbase puzzle.                  |
|          `metadata`          | object |                           The metadata of the block.                            |

#### Metadata

|           Parameter            | Type |                       Description                        |
|:------------------------------:|:----:|:--------------------------------------------------------:|
|           `network`            | u16  |               The network ID of the block                |
|            `round`             | u64  |       The round that produced this block - 8 bytes       |
|            `height`            | u32  |            The height of this block - 4 bytes            |
| `total_supply_in_microcredits` | u64  |        The total supply of microcredits - 8 bytes        |
|      `cumulative_weight`       | u128 |     The cumulative weight for this block - 16 bytes      |
|   `cumulative_proof_weight`    | u128 |  The cumulative proof target for this block - 16 bytes   |
|       `coinbase_target`        | u64  |       The coinbase target for this block - 8 bytes       |
|         `proof_target`         | u64  |        The proof target for this block - 8 bytes         |
|     `last_coinbase_target`     | u64  |   The coinbase target for the last coinbase - 8 bytes    |
|   `last_coinbase_timestamp`    | u64  | The Unix timestamp (UTC) for the last coinbase - 8 bytes |
|          `timestamp`           | i64  |    The Unix timestamp (UTC) for this block - 8 bytes     |


[//]: # (## Advanced Topics)

[//]: # ()
[//]: # (### Verifying Blocks)

[//]: # ()
[//]: # (The steps to verify a block are as follows:)

[//]: # ()
[//]: # (1. Validate the block header )

[//]: # (    - Check that the `merkle_root_hash` is derived correctly)

[//]: # (    - Check that the `pedersen_merkle_root_hash` is derived correctly)

[//]: # (    - Check that the block timestamp is not more than 2 hours into the future)

[//]: # (    - Check that the block's difficulty target matches the expected difficulty target)

[//]: # (    - Check that the difficulty hash is less than or equal to the difficulty target)

[//]: # (    - Verify the PoSW proof)

[//]: # (2. Verify that there is only 1 coinbase transaction included in the block)

[//]: # (3. Verify that the sum of all transaction value balances are equal to the expected block reward)

[//]: # (4. Verify each transaction included in the block)

[//]: # ( )