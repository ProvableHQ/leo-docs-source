---
id: get_blocks
title: Get Blocks
sidebar_label: Get Blocks
---

```javascript title=ENDPOINT
GET /testnet/blocks?start={start_height}&end={end_height}
```

Returns the blocks for the given block range.

### Arguments

| Parameter             | Type | Required | Description                                       |
|:----------------------|:----:|:--------:|:--------------------------------------------------|
| `start_height`        | u32  |   Yes    | The starting block height of the requested blocks |
| `end_height`          | u32  |   Yes    | The ending block height of the requested blocks   |


### Response

| Parameter |                 Type                 |     Description      |
|:---------:|:------------------------------------:|:--------------------:|
| `result`  | [array](../../concepts/05_blocks.md) | The requested blocks |

