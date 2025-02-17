---
id: get_block
title: Get Block
sidebar_label: Get Block
---

```bash title=ENDPOINT
GET /testnet/block/{height_or_hash}
```

Returns the block for the given block height or block hash.

### Arguments

| Parameter         |     Type      | Required | Description                                     |
|:------------------|:-------------:|:--------:|:------------------------------------------------|
| `height` or `hash` | u32 or string |   Yes    | The block height or hash of the requested block |


### Response

| Parameter |                 Type                  |     Description     |
|:---------:|:-------------------------------------:|:-------------------:|
| `result`  | [object](../../concepts/05_blocks.md) | The requested block |

