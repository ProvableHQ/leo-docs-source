---
id: get_block_transactions
title: Get Block Transactions
sidebar_label: Get Block Transactions
---

```bash title=ENDPOINT
GET /testnet/block/{height}/transactions
```

Returns the transactions for the given block height.

### Arguments

| Parameter | Type | Required | Description                                     |
|:----------|:----:|:--------:|:------------------------------------------------|
| `height`  | u32  |   Yes    | The block height of the requested transactions. |

### Response

| Parameter |                    Type                    |              Description               |
|:---------:|:------------------------------------------:|:--------------------------------------:|
| `result`  | [array](../../concepts/03_transactions.md) | The array of transactions in the block |
