---
id: transaction_broadcast
title: Transaction Broadcast
sidebar_label: Transaction Broadcast
---

```bash title=ENDPOINT
POST /testnet/transaction/broadcast
```

Broadcasts the transaction to the ledger.

### Body

|  Type  |               Description               |
|:------:|:---------------------------------------:|
| string | The serialized transaction to broadcast |

### Response

| Parameter |  Type  |                   Description                   |
|:---------:|:------:|:-----------------------------------------------:|
| `result`  | string | The status of the transaction broadcast attempt |
