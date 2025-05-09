---
id: hello
title: Hello, Leo
sidebar_label: Hello, Leo
---

## Initialize the project

Use the Leo Command Line Interface (CLI) to create a new project.
In your terminal, run:
```bash
leo new hello
cd hello
```

This creates a directory with the following structure:

```bash
hello/
├── .gitignore # A default `.gitignore` file for Leo projects
├── .env # The environment, containing the `NETWORK` and `PRIVATE_KEY` variables.
├── program.json # The manifest for the Leo project
└── src/
  └── main.leo # The Leo source code
```

## Build and Run 

Now let's compile the program.
```
leo build
```
 
On invoking the build command, Leo automatically creates a `/build⁠` and `output/`⁠ folder in the project directory. The compiled code is contained in the `build` directory. `output` is used to stored intermediate artifacts from compilation. 


The `leo run` command will compile and run the program.
In your terminal, run:
```bash
leo run main 1u32 2u32
```

```bash title="console output:"
Leo Compiled 'main.leo' into Aleo instructions

⛓  Constraints
 •  'hello.aleo/main' - 33 constraints (called 1 time)

➡️  Output
 • 3u32
  
Leo ✅ Finished 'hello.aleo/main' (in "/hello/build")
```

Congratulations! You've ran your first Leo program.

## Unpacking the Project

Let's go through the project.

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

All files in the current package will be compiled with the specified Leo `version`.

```json
    "version": "0.0.0",
```

Dependencies will be added to the field of the same name, as they are added. The dependencies are also pegged in the **leo.lock** file.

### The Code
Open up **src/main.leo**.
The **main.leo** file is the entry point of a Leo project. It often contains a function named `main`.
Let's break down the structure of a Leo file.
```leo title="src/main.leo" showLineNumbers
// The 'hello' program.
program hello.aleo {
    transition main(public a: u32, b: u32) -> u32 {
        let c: u32 = a + b;
        return c;
    }
}
```

`program hello.aleo {` defines the name of the [program](./../language/02_structure.md#program-scope) inside the Leo file.
The program ID must match the `program.json` manifest file.

The keyword `transition` indicates a [transition](./../language/02_structure.md#transition-function) function definition in Leo.
The `main` transition takes an input `a` with type `u32` and `public` visibility, and an input `b` with type `u32` and `private` visibility (by default).
The transition returns one result with type `u32`.
The transition body is enclosed in curly braces `{ }`. 
```leo
transition main(public a: u32, b: u32) -> u32 {
```

Inside the `main` function we declare a variable `c` with type `u32` and set it equal to the addition of variables `a` and `b`.
Leo's compiler will check that the types of `a` and `b` are equal and that the result of the addition is type `u32`.
```leo
let c: u32 = a + b;
```

:::note
Leo is designed to detect many errors at compile time, via statically checked strong types.
Try changing the type of any variable and seeing what Leo recommends with helpful error messages.
:::

Last, we return the variable `c`.
Leo will check that `c`'s type matches the function return type `u32`.
```leo
return c;
```

Now let us use the Leo CLI and see what other commands we can run on our program.

## Step by step

### 1. Clean
First, remove all build files with:
```bash
leo clean
```

```bash title="console output:"
  Leo cleaned the build directory (in "/build/")
```

### 2. Update
The `leo update` command updates the Leo compiler to the latest version.
```bash
leo update
```

```bash title="console output:"
  Leo ✅ Updated to version 1.12.0
```

### 3. Execute

The `leo execute` command executes the Leo program and outputs a transaction object
```bash
leo execute main 1u32 2u32
```

```bash title="console output:"
 Leo ✅ Compiled 'main.leo' into Aleo instructions

⛓  Constraints
 • 'hello.aleo/main' - 33 constraints (called 1 time)

➡️  Output
 • 3u32
 
 {"type":"execute","id":"at1 ... (transaction object truncated for brevity)
 
 Leo ✅ Executed 'hello.aleo/main' (in "/hello/build")
```

The `leo execute` command will attempt to verify a proof only if all previous steps completed successfully. Under the hood, the Leo [CLI](./../cli/00_overview.md) will check for existing `.prover` file to constructing proof, the `.verifier` file to verify proof and the `.avm` file containing the program's bytecode in the **build/build** directory before running each command. This ensures that we don't run unnecessary commands.

You'll notice that running `leo execute` produces a JSON object. This is a [`Transaction`](https://developer.aleo.org/concepts/fundamentals/transactions) that can be broadcast to the Aleo network.

## Next Steps

To learn more about the Leo language and its syntax, start [here](./../language/00_overview.md).

To learn more about how to use the Leo CLI, start [here](./../cli/00_overview.md).
