---
id: transactions
title: Transactions
sidebar_label: Transactions
---

A **transaction** is a fundamental data structure for publishing a state transition on the ledger.

## Types of Transactions

### Deploy Transaction
The deployment transaction publishes an Aleo program to the network.

|  Parameter   |  Type  |                                   Description                                    |
|:------------:|:------:|:--------------------------------------------------------------------------------:|
|    `type`    | string |                         The type of transaction (deploy)                         |
|     `id`     | string | The ID of transaction, computed via the Merkle Tree Digest of the transition IDs |
|   `owner`    | object |                         The owner address and signature                          |
| `deployment` | object |                         The deployment transaction info                          |
|    `fee`     | object |                          The deployment transaction fee                          |

#### Deployment Info

|      Parameter      | Type  |                            Description                            |
|:-------------------:|:-----:|:-----------------------------------------------------------------:|
| `global_state_root` |  u16  |             The global state root of the merkle tree              |
|    `transitions`    | array |              The [transitions](./04_transitions.md)               |

### Execute Transaction
The execution transaction represents a call to an Aleo program.

|    Parameter     |  Type  |                                   Description                                    |
|:----------------:|:------:|:--------------------------------------------------------------------------------:|
|      `type`      | string |                        The type of transaction (execute)                         |
|       `id`       | string | The ID of transaction, computed via the Merkle Tree Digest of the transition IDs |
|   `execution`    | object |                          The execution transaction info                          |
| `fee` (optional) | object |                      The optional execution transaction fee                      |


#### Execution Info

|      Parameter      | Type  |                            Description                            |
|:-------------------:|:-----:|:-----------------------------------------------------------------:|
| `global_state_root` |  u16  |             The global state root of the merkle tree              |
|    `transitions`    | array |              The [transitions](./04_transitions.md)               |

### Fee Transaction
The fee transaction represents a fee paid to the network, used for rejected transactions

| Parameter |  Type  |                                   Description                                    |
|:---------:|:------:|:--------------------------------------------------------------------------------:|
|  `type`   | string |                          The type of transaction (fee)                           |
|   `id`    | string | The ID of transaction, computed via the Merkle Tree Digest of the transition IDs |
|   `fee`   | object |                           The rejected transaction fee                           |





## Transaction Structure

|    Parameter     |  Type  |                Description                |
|:----------------:|:------:|:-----------------------------------------:|
|      `type`      | string |          The type of transaction          |
|       `id`       | string |    The ID of transaction (at1 prefix)     |
|   `deployment`   | object |      The deployment transaction info      |
| `additional_fee` | object |  The additional fee for the transaction   |


[//]: # ()
[//]: # (#### Deploy Transaction)

[//]: # ()
[//]: # (|    Parameter     |  Type  |              Description               |)

[//]: # (|:----------------:|:------:|:--------------------------------------:|)

[//]: # (|      `type`      | string |        The type of transaction         |)

[//]: # (|       `id`       | string |         The ID of transaction          |)

[//]: # (|   `deployment`   | object |    The deployment transaction info     |)

[//]: # (| `additional_fee` | object | The additional fee for the transaction |)

[//]: # ()
[//]: # (#### Deployment Info)

[//]: # ()
[//]: # (|    Parameter     |  Type   |                             Description                              |)

[//]: # (|:----------------:|:-------:|:--------------------------------------------------------------------:|)

[//]: # (|    `edition`     |   u16   |                             The edition                              |)

[//]: # (|    `program`     | object  |                             The program                              |)

[//]: # (| `verifying_keys` | mapping | The mapping of function names to their verifying key and certificate |)

[//]: # ()
[//]: # (## Advanced Topics)

[//]: # ()
[//]: # (### Creating Transactions)

[//]: # ()
[//]: # (The steps to create a transaction are as follows:)

[//]: # ()
[//]: # (1. Generate the serial numbers of the records being spent)

[//]: # (2. Generate the new records)

[//]: # (3. Generate the program commitment)

[//]: # (4. Generate the local data commitment)

[//]: # (5. Generate the transaction signatures)

[//]: # (6. Generate the ledger digest and [ledger membership witnesses]&#40;06_glossary.md#ledger-membership-witness&#41; for the input record commitments )

[//]: # (7. Generate the inner SNARK proof)

[//]: # (8. Generate the program SNARK proofs)

[//]: # (9. Generate the transaction proof)

[//]: # (10. Compose the transaction with the attributes above)

[//]: # ()
[//]: # ()
[//]: # (### Verifying Transactions)

[//]: # ()
[//]: # (The steps to verify a transaction are as follows:)

[//]: # ()
[//]: # (1. Verify that each serial number in `old_serial_numbers` does not already exist in the ledger.)

[//]: # (2. Verify that each commitment in `new_commitments` does not already exist in the ledger.)

[//]: # (3. Verify that the memo `memorandum` does not already exist in the ledger.)

[//]: # (4. Verify that the transaction proof `transaction_proof` verifies.)

