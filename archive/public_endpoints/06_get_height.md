---
id: get_height
title: Get Height
sidebar_label: Get Height
---

```bash title=ENDPOINT
GET /testnet/height/{blockHash}
```

Returns the height for the given block hash.

### Arguments

| Parameter   |     Type      | Required | Description                            |
|:------------|:-------------:|:--------:|:---------------------------------------|
| `blockHash` | string |   Yes    | The hash of the requested block height |


### Response

| Parameter | Type |        Description         |
|:---------:|:----:|:--------------------------:|
| `result`  | u32  | The requested block height |

