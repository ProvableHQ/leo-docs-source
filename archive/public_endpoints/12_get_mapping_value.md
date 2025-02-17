---
id: get_mapping_value
title: Get Mapping Value
sidebar_label: Get Mapping Value
---

```bash title=ENDPOINT
GET /testnet/program/{programID}/mapping/{mappingName}/{mappingKey}
```

Returns the value in a key-value mapping corresponding to the supplied mappingKey.


### Arguments

| Parameter     |  Type  | Required | Description                                  |
|:--------------|:------:|:--------:|:---------------------------------------------|
| `programID`   | string |   Yes    | The program id of the requested mapping      |
| `mappingName` | string |   Yes    | The name of the mapping to access            |
| `mappingKey`  | string |   Yes    | The key of the key-value pair in the mapping |

### Response

| Parameter |                  Type                   |                  Description                   |
|:---------:|:---------------------------------------:|:----------------------------------------------:|
| `result`  | [object](../../concepts/01_programs.md) | The value of the key-value pair in the mapping |
