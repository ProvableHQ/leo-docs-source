---
id: layout 
title: Layout of a Leo Project
sidebar_label: Project Layout
---
[general tags]: # (project, project_layout, manifest, module)

## The Manifest

**program.json** is the Leo manifest file that configures our package.
```json title="program.json"
{
  "program": "hello.aleo",
  "version": "0.1.0",
  "description": "",
  "license": "MIT",
  "dependencies": null,
  "dev_dependencies": null
}
```

The program ID in `program` is the official name that other developers will be able to look up after you have published your program.
```json
    "program": "hello.aleo",
```

Dependencies will be added to the field of the same name, as they are added. The dependencies are also pegged in the **leo.lock** file.

## The Code

The `src/` directory is where all of your Leo code will live.  The main entry point of your project is a file in this directory.appropriately named `main.leo`.  Calls to many of the Leo CLI commands will require you to have this file within your project in order to succeed properly.


### Modules 

In addition to your main file, Leo also supports a module system as of v3.2.0.

Leaf modules (i.e. modules without submodules) must be defined in a single file (ex. `foo.leo`) Modules with submodules must be defined by an optional top-level `.leo` file and a subdirectory directory containing the submodules:


Take the following project as an example:
```
src
├── common.leo
├── main.leo
├── outer.leo
└── outer
    └── inner.leo
```

Given the structure above, the following modules are defined:

| Filename | Type | Module Name | Access Location & Pattern |
| -------- | ---- | ----------- | ------------------------- |
| `common.leo` | Module | `common` | `main.leo` : `common::<item>`  |
| `outer.leo` | Module | `outer` | `main.leo` : `outer::<item>` |
| `outer/inner.leo` | Submodule | `outer::inner` |`main.leo` : `outer::inner::<item>` <br> `outer.leo` : `inner::<item>`|

:::info
Only relative paths are implemented so far. That means that items in `outer.leo` cannot be accessed from items in `inner.leo`, for example. This is limiting for now but will no longer be an issue when we add absolute paths.
:::


A module file may only contain `struct`, `const`, and `inline` definitions:

```leo
const X: u32 = 2u32;

struct S {
    a: field
}

inline increment(x: field) -> field {
    return 1field;
}
```


<!-- 

## The Tests

TODO

## The Build and Outputs

Only generated when the project is compiled.  Removed when `leo clean` is called. 

TODO

-->
