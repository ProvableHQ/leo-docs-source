---
id: find_transaction_id_from_transition_id
title: Find Transaction ID From Transition ID
sidebar_label: Find Transaction ID from Transition ID
---

```bash title=ENDPOINT
GET /testnet/find/transactionID/{transitionID}
```

Returns the transaction ID of the transaction containing the given transition ID.

### Arguments

| Parameter |  Type  |    Description    |
|:---------:|:------:|:-----------------:|
| `result`  | String | The transition ID |

### Response

| Parameter |  Type  |    Description     |
|:---------:|:------:|:------------------:|
| `result`  | String | The transaction ID |
