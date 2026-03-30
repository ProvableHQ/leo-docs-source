---
id: aleo-sdk
title: Aleo SDK
sidebar_label: Aleo SDK
---

<!-- markdown-link-check-disable -->
[![github]](https://github.com/ProvableHQ/python-sdk/tree/master/sdk)
<!-- markdown-link-check-enable -->

[github]: https://img.shields.io/badge/github-8da0cb?style=for-the-badge&labelColor=555555&logo=github

[**aleo on PyPI**](https://pypi.org/project/aleo/)

Aleo Python bindings for building zero-knowledge applications.

This project uses `PyO3` and `Maturin` to provide a Python library named `aleo` that binds to `snarkVM` Rust functionalities. The resulting `aleo` library can be easily installed and used within Python. The project is at an early stage. At the moment, only Mac OS 11 or newer on Apple Silicon is supported - support for other operating systems will follow. If you are using another operating system, you may be able to build the library yourself following the instructions below.

Supported functionalities currently include:
* Private key generation

## Usage
The `aleo` Python library is available on [PyPI](https://pypi.org/project/aleo/) for installation.

### Installation
Ensure you have Python 3.11 or newer installed. If your operating system is supported, you can install the library from pip:

```bash
pip3 install aleo
```

<!-- markdown-link-check-disable -->
Alternatively, you can also install from a `.whl` file, either from the [GitHub repository](https://github.com/ProvableHQ/python-sdk/tree/master/sdk/target/wheels), or by building it yourself. For the installation, use a command like this:
<!-- markdown-link-check-enable -->

```bash
pip3 install aleo-0.0.3-cp311-cp311-macosx_11_0_arm64.whl
```

Note: On some systems, you may need to use `pip` instead of `pip3`.

### Example use
You can import the library, generate a private key object, and print it as a string as in the following example:
```Python
import aleo
private_key = aleo.PrivateKey()
print(private_key.to_string())
```

<!-- markdown-link-check-disable -->
### Build Instructions
To build it, first clone the [GitHub repository](https://github.com/ProvableHQ/python-sdk/tree/master/sdk). Then, run the following command:
```bash
bash build.sh
```
<!-- markdown-link-check-enable -->

After successful building, you will find a new `.whl` file in the `target/wheels` directory that you can install using the command from above.

## Building Python Apps

Further documentation and tutorials as to how to use the `aleo` Python library will follow soon.
