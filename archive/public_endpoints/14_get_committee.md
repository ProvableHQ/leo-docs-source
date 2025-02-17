---
id: get_committee
title: Get Committee
sidebar_label: Get Committee
---

```bash title=ENDPOINT
GET /testnet/latest/committee
```

Returns the list of current committee members and their stake.

### Arguments

None

### Response

| Parameter |                  Type                  |         Description          |
|:---------:|:--------------------------------------:|:----------------------------:|
| `result`  | [array](../../concepts/00_accounts.md) | The latest committee members |