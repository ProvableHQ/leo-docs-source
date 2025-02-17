---
id: find_transition_id
title: Find Transition ID
sidebar_label: Find Transition ID
---

```bash title=ENDPOINT
GET /testnet/find/transitionID/{inputOrOutputID}
```

Returns the transition ID of the transition corresponding to the ID of the input or output.

### Arguments

|     Parameter     |  Type  |      Description       |
|:-----------------:|:------:|:----------------------:|
| `inputOrOutputID` | String | The input or output ID |

### Response

| Parameter |  Type  |    Description    |
|:---------:|:------:|:-----------------:|
| `result`  | String | The transition ID |