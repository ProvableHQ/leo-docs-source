---
id: get_program
title: Get Program
sidebar_label: Get Program
---

```bash title=ENDPOINT
GET /testnet/program/{programID}
```

Returns the program for the given program ID.

### Arguments

| Parameter   |  Type  | Required | Description                             |
|:------------|:------:|:--------:|:----------------------------------------|
| `programID` | string |   Yes    | The program id of the requested program |

### Response

| Parameter |                  Type                   |      Description      |
|:---------:|:---------------------------------------:|:---------------------:|
| `result`  | [object](../../concepts/01_programs.md) | The requested program |