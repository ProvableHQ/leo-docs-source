---
id: installation
title: Installation
sidebar: Installation
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Setup

There are a number of ways to install Leo, depending on your platform and preferences. Take your pick!

<Tabs defaultValue="cargo"
values={[
  { label: 'Cargo', value: 'cargo' },
  { label: 'Pre-Built Binaries', value: 'prebuilt' },
  { label: 'Build from Source', value: 'source' },
]}>
  <TabItem value="cargo">
    <h3>1. Install Cargo</h3>
    <p>The easiest way to install Cargo is to install the latest stable release of Rust: <a href="https://bit.ly/start-rust">bit.ly/start-rust</a></p>
    <h3>2. Install Leo</h3>
    <pre><code>cargo install leo-lang</code></pre>
    <p>This will generate the executable at <code>~/.cargo/bin/leo</code>.</p>
  </TabItem>
  <TabItem value="prebuilt">
    <p>Download and install Leo with the official pre-built binaries.</p>
    <h3>For MacOS (Apple Silicon)</h3>
    <ul>
      <li><a href="https://github.com/ProvableHQ/leo/releases/latest/download/leo.zip"><strong>Download Leo for Apple Silicon (MacOS)</strong></a></li>
      <li>This will download a <code>.zip</code> file containing a <strong>Unix Executable File</strong>.</li>
    </ul>
    <h4>Installation:</h4>
    <ol>
      <li>Extract the <code>.zip</code> file.</li>
      <li>Open a terminal and navigate to the extracted directory.</li>
      <li>Run <code>chmod +x leo</code> to make the file executable.</li>
      <li>Move <code>leo</code> to <code>/usr/local/bin</code> to use it system-wide.</li>
      <pre><code>mv leo /usr/local/bin</code></pre>
      <li>Run <code>leo --version</code> to confirm installation.</li>
    </ol>
    <h3>For Other Platforms</h3>
    <ul>
      <li><a href="https://github.com/ProvableHQ/leo/releases"><strong>Browse all Leo releases</strong></a></li>
    </ul>
  </TabItem>
  <TabItem value="source">
    <h2>Install from Source</h2>
    <p>To use the latest Leo features, install the Leo source code from GitHub.</p>
    <h3>1. Install the Prerequisites</h3>
    <ul>
      <li><strong>Install Git:</strong> <a href="https://bit.ly/start-git">bit.ly/start-git</a></li>
      <li><strong>Install Rust:</strong> <a href="https://bit.ly/start-rust">bit.ly/start-rust</a></li>
    </ul>
    <h4>Verify Installation</h4>
    <pre><code>git --version</code></pre>
    <pre><code>cargo --version</code></pre>
    <h3>2. Build Leo from Source Code</h3>
    <pre><code># Download the source code
git clone https://github.com/ProvableHQ/leo
cd leo
# Build and install
cargo install --path .</code></pre>
    <p>This will generate the executable at <code>~/.cargo/bin/leo</code>.</p>
    <h4>To use Leo, run:</h4>
    <pre><code>leo</code></pre>
  </TabItem>
</Tabs>

-----

## IDE Syntax Highlighting

Aleo maintains syntax highlighting implementations across different platforms.
If you do not see your favorite editor on this list, please reach out on [GitHub](https://github.com/ProvableHQ/welcome/issues/new).

1. [Visual Studio Code](06_tooling.md#vs-code)
2. [Sublime Text](06_tooling.md#sublime-text)
3. [IntelliJ](06_tooling.md#intellij)
