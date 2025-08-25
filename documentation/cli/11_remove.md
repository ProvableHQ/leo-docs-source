---
id: cli_remove
title: leo remove
sidebar_label: Remove
---
To remove a dependency from your project, run the following command:

```bash
leo remove <NAME>
```
where `<NAME>` is the name of the imported program.

See the **[Dependency Management](./../guides/02_dependencies.md)** guide for more details.

### Flags:

#### `--all`
Removes all dependencies (or dev dependencies, if used with --dev).

#### `-dev`
Removes dev dependencies.

