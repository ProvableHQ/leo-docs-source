---
id: installation
title: Installation
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Downloads

Download the latest Leo release using a pre-built installer for your platform, and start developing today.

<Tabs
    defaultValue="mac"
    values={[
        {label: 'Pre-Built Installer', value: 'prebuilt'},
        {label: 'Install from Source', value: 'source'},
    ]}>
    
    <TabItem value="prebuilt">
        ## Pre-Built Installer

        <p>Download and install Leo with the official pre-built installer.</p>

        **For MacOS (Apple Silicon)**
        - <a href="https://github.com/AleoHQ/leo/releases/latest/download/leo.zip"><b>Download Leo for Apple Silicon (MacOS)</b></a>  
        - This will download a `.zip` file containing a **Unix Executable File**.  
        - **Installation:**  
          1. Extract the `.zip` file.  
          2. Open a terminal and navigate to the extracted directory.  
          3. Run `chmod +x leo` to make the file executable.  
          4. Move `leo` to `/usr/local/bin` to use it system-wide.  
          ```
          mv leo /usr/local/bin
          ```
          5. Run `leo --version` to confirm installation.

        **For Other Platforms**
        - <a href="https://github.com/AleoHQ/leo/releases"><b>Browse all Leo releases</b></a>

    </TabItem>

    <TabItem value="source">
        ## Install from Source

        <p>To use the latest Leo features, install the Leo source code from GitHub.</p>

        ### 1. Install the Prerequisites
        - **Install Git:** [bit.ly/start-git](https://bit.ly/start-git)  
        - **Install Rust:** [bit.ly/start-rust](https://bit.ly/start-rust)

        **Verify Installation**
        ```
        git --version
        cargo --version
        ```

        ### 2. Build Leo from Source Code
        ```
        # Download the source code
        git clone https://github.com/AleoHQ/leo
        cd leo

        # Build and install
        cargo install --path .
        ```

        This will generate the executable at `~/.cargo/bin/leo`.

        **To use Leo, run:**
        ```
        leo
        ```
    </TabItem>

</Tabs>

-----

## IDE Syntax Highlighting

Aleo maintains syntax highlighting implementations across different platforms.
If you do not see your favorite editor on this list, please reach out on [GitHub](https://github.com/ProvableHQ/welcome/issues/new).

1. [Visual Studio Code](06_tooling.md#vs-code)
2. [Sublime Text](06_tooling.md#sublime-text)
3. [IntelliJ](06_tooling.md#intellij)
