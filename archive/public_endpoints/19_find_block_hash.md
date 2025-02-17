---
id: find_block_hash
title: Find Block Hash
sidebar_label: Find Block Hash
---

```bash title=ENDPOINT
GET /testnet/find/blockHash/{transactionID}
```

Returns the block hash of the block containing the given transaction ID.

### Arguments

|    Parameter    |  Type  |    Description     |
|:---------------:|:------:|:------------------:|
| `transactionID` | String | The transaction ID |

### Response

| Parameter |  Type  |                 Description                  |
|:---------:|:------:|:--------------------------------------------:|
| `result`  | String | The block hash containing the transaction ID |
