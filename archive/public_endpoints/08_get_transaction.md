---
id: get_transaction
title: Get Transaction
sidebar_label: Get Transaction
---

```bash title=ENDPOINT
GET /testnet/transaction/{transactionID}
```

Returns the transaction for the given transaction ID.

### Arguments

| Parameter       |  Type  | Required | Description                                         |
|:----------------|:------:|:--------:|:----------------------------------------------------|
| `transactionID` | string |   Yes    | The transaction id of the requested transaction hex |

### Response

| Parameter |                    Type                     |        Description        |
|:---------:|:-------------------------------------------:|:-------------------------:|
| `result`  | [object](../../concepts/03_transactions.md) | The requested transaction |