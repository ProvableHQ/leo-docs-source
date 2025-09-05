---
id: layout 
title: Layout of a Leo Project
sidebar_label: Project Layout
---
[general tags]: # (project, project_layout, manifest)

### The Manifest

**program.json** is the Leo manifest file that configures our package.
```json title="program.json"
{
  "program": "hello.aleo",
  "version": "0.1.0",
  "description": "",
  "license": "MIT",
  "dependencies": null
}
```

The program ID in `program` is the official name that other developers will be able to look up after you have published your program.
```json
    "program": "hello.aleo",
```

Dependencies will be added to the field of the same name, as they are added. The dependencies are also pegged in the **leo.lock** file.