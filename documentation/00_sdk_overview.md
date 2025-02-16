---
id: sdk
title: Aleo SDK
sidebar_label: Overview
---


The [Aleo SDK](https://github.com/ProvableHQ/sdk) provides tools for building zero knowledge applications. It consists of
several TypeScript & JavaScript libraries which provide the following functionality:
1. [Aleo account management](https://provable.tools/account)
2. [Web-based program execution and deployment](https://provable.tools/develop)
3. [Aleo credit transfers](https://provable.tools/transfer)
4. [Management of program state and data](https://provable.tools/record)
5. [Communication with the Aleo network](https://provable.tools/rest)

All of this functionality is demonstrated on [provable.tools](https://www.provable.tools/).

The Aleo SDK is divided into three Typescript/Javascript packages

## 1. [Aleo SDK](./sdk/typescript/00_sdk_overview.md) - Build Zero Knowledge Web Apps

<a href="https://www.npmjs.com/package/@provablehq/sdk"> <img alt="Aleo SDK" src="https://img.shields.io/npm/l/%40aleohq%2Fsdk?label=NPM%20-%20Aleo%20SDK&labelColor=green&color=blue" /></a>


The official Aleo SDK providing Javascript/Typescript tools for creating zero knowledge apps.

### ⚡ Build your own app

Start here with the [documentation](./sdk/typescript/00_sdk_overview.md) and follow the instructions to install the [Aleo SDK](https://github.com/ProvableHQ/sdk/tree/testnet3/sdk#readme) to get started building your
first zero knowledge web app.

#### Source: [`sdk/sdk`](https://github.com/ProvableHQ/sdk/tree/testnet3/sdk)


## 2. [Create-leo-App](./sdk/create-leo-app/00_app_installation.md) - Zero-Knowledge Web App Examples

<a href="https://www.npmjs.com/package/create-leo-app"> <img alt="Create Leo App" src="https://img.shields.io/npm/l/create-aleo-app?label=NPM%20-%20Create-Leo-App&labelColor=green&color=blue" /></a>

Create-leo-app provides zero-knowledge web app examples in common web frameworks such as React. Developers looking to
start with working examples should start here.

### ⚡ Build your own app


You can start with a template by running
```bash
npm create leo-app@latest
```


#### Source: [`sdk/create-leo-app`](https://github.com/ProvableHQ/sdk/tree/testnet3/create-leo-app)

## 3. [Aleo-Wasm](./sdk/wasm/00_wasm_installation.md) - Zero Knowledge Algorithms in JavaScript + WebAssembly

<a href="https://www.npmjs.com/package/@provablehq/wasm"> <img alt="Create Aleo App" src="https://img.shields.io/npm/l/%40aleohq%2Fwasm?label=NPM%20-%20Aleo%20Wasm&labelColor=green&color=blue" /></a>
<a href="https://www.npmjs.com/package/@aleohq/nodejs"> <img alt="Create Aleo App" src="https://img.shields.io/npm/l/%40aleohq%2Fnodejs?label=NPM%20-%20Aleo%20Nodejs&labelColor=green&color=blue" /></a>
<a href="https://crates.io/crates/aleo-wasm"> <img alt="Aleo-Wasm" src="https://img.shields.io/crates/v/aleo-wasm.svg?color=neon" /></a>

Aleo Wasm is a Rust crate which compiles Aleo code responsible for creating and executing zero knowledge programs into
WebAssembly.

When compiled with `wasm-pack` JavaScript bindings are generated for the WebAssembly allowing Aleo zero
knowledge programs to be used in the browser and NodeJS. This package is available on NPM (linked above). The
[documentation](./sdk/wasm/00_wasm_installation.md) provides instructions for compiling this [crate](https://github.com/ProvableHQ/sdk/tree/testnet3/wasm) and using it in web projects for those interested in building from
source.

❗ Currently program execution is only available in web Browsers. However, account, program and data management within
NodeJS is functional.

Source: [`sdk/wasm`](https://github.com/ProvableHQ/sdk/tree/testnet3/wasm)

## 4. Aleo Python SDK - Zero Knowledge Algorithms in Python and Zero Knowledge Machine Learning

The official Aleo Python SDK providing tools for creating zero knowledge apps. It consists of two separate libraries:

* The **aleo** library, which uses foreign function interfaces to bring snarkVM functionalities to Python.
* The **zkml** library, which transpiles scikit-learn machine learning models into Leo programs for inference.

Both libraries are in an early development stage. They can be installed through `pip install aleo` respectively `pip install zkml`. The
[documentation on the aleo library](./sdk/python/00_aleo_sdk.md) and the [documentation on the zkml library](./sdk/python/01_zkml_transpiler.md) provide more information on installation and usage.

<!-- markdown-link-check-disable -->
Source: [`python-sdk`](https://github.com/ProvableHQ/python-sdk)
<!-- markdown-link-check-enable -->

## 📚 Documentation

#### [API Documentation](https://developer.aleo.org)
API Documentation, tutorials for the Aleo SDK, and documentation on how to build Leo and Aleo Instructions programs can
be found on the [Aleo Developer Docs](https://developer.aleo.org) page.

#### [SDK Readme](https://github.com/ProvableHQ/sdk/tree/testnet3/sdk#readme)
The SDK Readme provides concepts core to executing zero knowledge programs in the web and several detailed examples of
how to use the SDK to build web apps using Aleo.

#### [Aleo Wasm Readme](https://github.com/ProvableHQ/sdk/tree/testnet3/wasm#readme)
The Aleo Wasm Readme provides instructions for compiling the Aleo Wasm crate and using it in web projects. Those who
want to build from source or create their own WebAssembly bindings should start here
