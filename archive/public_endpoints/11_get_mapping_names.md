---
id: get_mapping_names
title: Get Mapping Names
sidebar_label: Get Mapping Names
---

```bash title=ENDPOINT
GET /testnet/program/{programID}/mappings
```

Returns the names of the mappings in a program for the given program ID.

### Arguments

| Parameter   |  Type  | Required | Description                                     |
|:------------|:------:|:--------:|:------------------------------------------------|
| `programID` | string |   Yes    | The program id of the requested mappings        |

### Response

| Parameter |                  Type                   |         Description         |
|:---------:|:---------------------------------------:|:---------------------------:|
|  `array`  | [object](../../concepts/01_programs.md) | The requested mapping names |