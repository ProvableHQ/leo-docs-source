---
id: get_state_path_for_commitment
title: Get State Path For Commitment
sidebar_label: Get State Path For Commitment
---

```bash title=ENDPOINT
GET /testnet/statePath/{commitment}
```

Returns the state path for the given commitment.
The state path proves existence of the transition leaf to either a global or local state root.

### Arguments

|  Parameter   |  Type  |                  Description                  |
|:------------:|:------:|:---------------------------------------------:|
| `commitment` | string | The record commitment in the best valid chain |

### Response

|       Parameter       |  Type  |                Description                |
|:---------------------:|:------:|:-----------------------------------------:|
|  `global_state_root`  | string |           The global state root           |
|     `block_path`      | string |    The Merkle path for the block hash     |
|     `block_hash`      | string |              The block hash               |
| `previous_block_hash` | string |          The previous block hash          |
|     `header_root`     | string |           The block header root           |
|     `header_path`     | string | The Merkle path for the block header leaf |
|     `header_leaf`     | string |           The block header leaf           |
|  `transactions_path`  | string |  The Merkle path for the transaction ID   |
|   `transaction_id`    | string |            The transaction ID             |
|  `transaction_path`   | string | The Merkle path for the transaction leaf  |
|  `transaction_leaf`   | string |           The transaction leaf            |
|   `transition_path`   | string |  The Merkle path for the transition leaf  |
|   `transition_leaf`   | string |            The transition leaf            |

