---
id: cli_update
title: leo update
sidebar_label: Update
---
[general tags]: # (cli, leo_update, versioning)

To download and install the latest Leo version run:

```bash
leo update
```

```bash title="console output:"
Checking target-arch... aarch64-apple-darwin
Checking current version... v3.1.0
Checking latest released version... v3.1.0
       Leo 
Leo is already on the latest version
```

If you'd like to install a specific version of Leo, you can do so by passing the `--name` flag:

```bash
leo update --name v3.0.0
```

### Flags:
#### `-l`
#### `--list`
Lists all available versions of Leo.

#### `-n`
#### `--name`
An optional release name if you wish to install a specific version of Leo.  By default, the command will look for the latest release.

