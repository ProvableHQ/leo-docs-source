---
id: libraries
title: Leo Libraries
sidebar_label: Libraries
---

[general tags]: # "library, reusable_code, struct, const, fn, dependency, module"

A **library** is a Leo project that contains reusable code — structs, constants, and helper functions — intended to be shared across multiple programs. Unlike a regular Leo program, a library has no on-chain footprint: it declares no program ID, no mappings, no records, and no entry functions. All library code is inlined into the programs that use it at compile time.

## Creating a Library

Use `leo new` with the `--lib` flag to create a library project:

```bash
leo new math_utils --lib
```

This produces the same project structure as a regular Leo project, with one difference: the main source file is named `lib.leo` instead of `main.leo`.

```
math_utils/
├── program.json
└── src/
    └── lib.leo
```

## Writing a Library

A library source file (`lib.leo`) may contain `struct` definitions, `const` declarations, and `fn` definitions. It does **not** contain a `program { }` block.

```leo title="src/lib.leo"
/// The maximum value representable by a u32.
const MAX_U32: u32 = 4294967295u32;

/// A 2-D point with integer coordinates.
struct Point {
    x: i32,
    y: i32,
}

/// Returns the absolute value of a signed 32-bit integer.
fn abs(x: i32) -> i32 {
    return x >= 0i32 ? x : 0i32 - x;
}

/// Returns the Manhattan distance between two points.
fn manhattan(a: Point, b: Point) -> u32 {
    let dx: i32 = abs(a.x - b.x);
    let dy: i32 = abs(a.y - b.y);
    return dx as u32 + dy as u32;
}
```

### What a library may contain

| Item | Allowed | Notes |
| ---- | ------- | ----- |
| `const` declarations | ✅ | Global compile-time constants |
| `struct` definitions | ✅ | Shared data types |
| `fn` definitions | ✅ | Helper functions, including generic `fn::[…]` |
| `program { }` block | ❌ | Libraries have no on-chain identity |
| `mapping` / `storage` | ❌ | No on-chain state |
| `record` types | ❌ | Records belong to programs |
| Entry `fn` / `final fn` | ❌ | No callable entry points |

## Declaring the Dependency

To use a library from another Leo project, add it to that project's `program.json`. Libraries can only be referenced from the local filesystem.

### Local library

```json title="program.json"
{
  "program": "my_app.aleo",
  "version": "0.1.0",
  "description": "",
  "license": "MIT",
  "leo": "4.0.0",
  "dependencies": [
    {
      "name": "math_utils",
      "location": "local",
      "path": "../math_utils"
    }
  ]
}
```

:::info
The `leo add` command can populate these entries automatically:
```bash
leo add math_utils --local ../math_utils 
```
:::

## Using a Library

Reference library items with the `{library_name}::{item}` path syntax. No `import` statement is required; the dependency entry in `program.json` is sufficient.

```leo title="src/main.leo"
program my_app.aleo {
    fn closest(
        origin: math_utils::Point,
        a: math_utils::Point,
        b: math_utils::Point,
    ) -> math_utils::Point {
        let da: u32 = math_utils::manhattan(origin, a);
        let db: u32 = math_utils::manhattan(origin, b);
        return da <= db ? a : b;
    }

    @noupgrade
    constructor() {}
}
```

Constants from a library are referenced the same way:

```leo
const CEILING: u32 = math_utils::MAX_U32;
```

## Generic Library Functions

Library functions support const generic parameters, just like regular helper functions. The concrete type argument must be a compile-time constant.

```leo title="src/lib.leo"
/// Clamps `value` to the range [0, MAX].
fn clamp::[MAX: u32](value: u32) -> u32 {
    return value > MAX ? MAX : value;
}
```

```leo title="src/main.leo"
program my_app.aleo {
    fn normalize(x: u32) -> u32 {
        return math_utils::clamp::[100u32](x);
    }

    @noupgrade
    constructor() {}
}
```

## How Libraries Work

Libraries are **inlined at compile time**. The Leo compiler resolves all library references before emitting Aleo bytecode — no library code appears as a separate program on-chain. This means:

- Calling a library function has the same cost as calling an inline helper function.
- Libraries cannot be deployed independently; they exist only as source-level abstractions.
- Circular dependencies between libraries are not allowed.

For more on how dependencies are resolved and cached, see [Dependency Management](../guides/02_dependencies.md).
