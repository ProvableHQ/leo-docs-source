---
id: get_env_info
title: Get Environment Info
sidebar_label: Get Environment Info
---

```bash title=ENDPOINT
GET /testnet/node/env
```

Returns the environment info of the node.
The snarkOS command line arguments to start the node as well as the GitHub branch and commit hash.

### Arguments

None

### Response

| Parameter |  Type  |     Description      |
|:---------:|:------:|:--------------------:|
| `result`  | Object | The environment info |
