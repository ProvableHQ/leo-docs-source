---
id: dependencies 
title: Dependency Management
sidebar_label: Dependency Management
---
[general tags]: # (guides, dependency, dependency_management, imports, program)

## Leo Imports
In your `main.leo` file, specify any imported dependencies using the `import` keyword before the program declaration:
```leo
import credits.aleo;

program test.aleo {
...
}
```

From the root of your Leo program directory, use the `leo add` command to update the `program.json` manifest to add dependencies.

## Deployed Programs
When adding a deployed program as a dependency to your program, such as the `credits.aleo`, use the following command::

```
leo add credits.aleo
```
or
```
leo add credits
```

If you are deploying to mainnet, you will need to specify mainnet imports using the `--network` flag as follows:

```
leo add credits --network mainnet
```

For the first imported dependency, a new `dependencies` field will be added to the 'package.json` manifest:

```json
{
  "program": "your_program.aleo",
  "version": "0.0.0",
  "description": "",
  "license": "MIT",
  "dependencies": [
    {
      "name": "credits.aleo",
      "location": "network",
      "network": "testnet",
      "path": null
    }
  ]
}
```

Dependencies can be removed using the `leo remove` command:
```bash
leo remove credits.aleo
```

## Local development
When deploying to a local devnet, specify the path for the local dependency as follows:

```
leo add program_name.aleo --local ./path_to_dependency
```
The dependencies section in the `program.json` manifest should include the path:
```json
{
  "program": "your_program.aleo",
  "version": "0.0.0",
  "description": "",
  "license": "MIT",
  "dependencies": [
    {
      "name": "local_dependency.aleo",
      "location": "local",
      "network": null,
      "path": "./path"
    }
  ]
} 
```

## Deploying to a program with local dependencies to a network.
When deploying a program that uses local dependencies, use the following command:
```bash
leo deploy --recursive
```
All local dependency will be deployed in order, followed by the main program.  Deployed dependencies will be skipped.