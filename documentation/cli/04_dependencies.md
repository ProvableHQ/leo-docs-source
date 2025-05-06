---
id: dependencies 
title: Dependency Management
sidebar_label: Dependency Management
---
## Leo imports
In your `main.leo` file, specify any imported dependencies using the `import` keyword before the program declaration:
```leo
import credits.aleo;

program test.aleo {
...
}
```

Use the `leo add` command to update the `program.json` manifest to include any dependencies used in your program.

## Deployed Programs
When adding a deployed program as a dependency to your program, such as the `credits.aleo`, use the following command::

```
leo add credits.aleo
```
or
```
leo add credits
```

If you are deplolying to mainnet, you will need to specify mainnet imports using the `--network` flag as follows:

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

## Local development
When deploying to a local devnet, specify the path for the local dependency as follows:

```
leo add program_name.aleo --local ./path_to_dependency
```

