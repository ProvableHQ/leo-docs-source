---
id: get_memory_pool_transactions
title: Get Memory Pool Transactions
sidebar_label: Get Memory Pool Transactions
---

```bash title=ENDPOINT
GET /testnet/memoryPool/transactions
```

Returns the transactions in the memory pool.

### Arguments

None

### Response

| Parameter |                    Type                    |        Description        |
|:---------:|:------------------------------------------:|:-------------------------:|
| `result`  | [array](../../concepts/03_transactions.md) | The array of transactions |