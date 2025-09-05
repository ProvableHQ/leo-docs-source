---
id: installation
title: Installation
sidebar: Installation
---
[general tags]: # (installation, install_leo)

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Setup

There are a number of ways to install Leo, depending on your platform and preferences. Take your pick!

If you'd like to try Leo without installing it locally on your machine, check out the [Leo Playground](./02_ide.md#leo-playground).

<Tabs defaultValue="cargo"
values={[
  { label: 'Cargo', value: 'cargo' },
  { label: 'Pre-Built Binaries', value: 'prebuilt' },
  { label: 'Build from Source', value: 'source' },
]}>
<TabItem value="cargo">

### 1. Install Cargo
The easiest way to install Cargo is to install the latest stable release of [Rust](https://www.rust-lang.org/tools/install).
### 2. Install Leo
```bash
cargo install leo-lang
```
This will generate the executable at `~/.cargo/bin/leo`.
</TabItem>
<TabItem value="prebuilt">
Download and install Leo with the official pre-built binaries.

### For MacOS (Apple Silicon):
  - **[Download Leo for Apple Silicon (MacOS)](https://github.com/ProvableHQ/leo/releases/latest/download/leo.zip)**
  - This will download a `.zip` file containing a **Unix Executable File**.
#### Installation
  1. Extract the `.zip` file
  2. Open a terminal land navigate to the extracted directory.
  3. Run `chmod +x leo` to make the file executable
  4. Move `leo` to `/usr/local/bin` to use it system wide.

          mv leo /usr/local/bin

  5. Run `leo --version` to confirm installation

  ### For Other Platforms:
   - **[Browse all Leo releases](https://github.com/ProvableHQ/leo/releases)**

</TabItem>
<TabItem value="source">

### 1. Install the Prerequisites
 - **Install [Git](https://git-scm.com/downloads)**
 - **Install [Rust](https://www.rust-lang.org/tools/install)**
 
#### Verify Installation
```bash
git --version
```
```bash
cargo --version
```
### 2. Build Leo from Source Code
```bash
# Download the source code 
git clone https://github.com/ProvableHQ/leo
cd leo
# Build and install
cargo install --path .
```
This will generate the executable at `~/.cargo/bin/leo`.

#### To use Leo, run:
```bash
leo
```
</TabItem>
</Tabs>

