---
id: cli_add
title: ""
sidebar_label: Add 
toc_min_heading_level: 2
toc_max_heading_level: 2
---
[general tags]: # (cli, leo_add, add_dependency, dependency, dependency_management, imports)

# `leo add`

The `leo add` command is used to add a new on-chain or local dependency to the current Leo project.


To add a local dependency to your project, run the following command:
```bash
leo add --local <LOCAL> <NAME>
```
where `<NAME>` is the name of the imported program, and `<LOCAL>` is the path to the local project.

&nbsp;

To add a program already deployed onchain as a dependency to your project, run the following command:
```bash
leo add --network <NAME>
```
where `<NAME>` is the name of the imported program.

### Flags:
#### `--local <LOCAL> `
#### `-l <LOCAL>`
Specifies that the dependency to be added is a local program located at path `<LOCAL>`.  This can either be the root directory for a Leo project, or a path directly to an already compiled `.aleo` file.


#### `--network`
#### `-n `
Specifies that the dependency to be added is a remote program currently deployed onchain. The network that it will be pulled from will be the same as the one specified in by the `NETWORK` variable in `.env`



#### `--edition <EDITION> `
#### `-e <EDITION>`
Specifies the expected edition of the program being imported. Only passing this flag will assume that the program is being imported from the network. 

:::warning
Do not use this feature unless you know what you are doing!
:::



#### `--dev`
Specifies that the imported program is a development dependency and should not be used in production



#### `-c`
#### `--clear`
Clears all previous dependencies.

:::warning
This feature is currently bugged and is non-functional.
:::

