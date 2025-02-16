---
id: zkml-transpiler
title: zkml transpiler
sidebar_label: zkml Transpiler
---

<!-- markdown-link-check-disable -->
[![github]](https://github.com/ProvableHQ/python-sdk/tree/master/zkml)
<!-- markdown-link-check-enable -->

[github]: https://img.shields.io/badge/github-8da0cb?style=for-the-badge&labelColor=555555&logo=github

[**zkml on PyPI**](https://pypi.org/project/zkml/)

Aleo transpiler for building zero-knowledge machine learning applications.

This project provides a Python library named `zkml` that transpiles `scikit-learn` machine learning models into a `Leo` project for inference. The resulting leo project can then be run and executed from Python. The project is at an early stage.

Supported machine learning models currently include:
* Decision tree classifiers
* Multilayer perceptron neural network regressor

## Usage
The `zkml` Python library is available on [PyPI](https://pypi.org/project/zkml/) for installation.

### Prerequisites
#### Python
Ensure you have 3.9.6 or newer installed.
   - Verify with:
   ```bash
   python3 --version
   ```
   - If not installed, follow the instructions [here](https://wiki.python.org/moin/BeginnersGuide/Download).

#### Leo
Ensure you have Leo version 1.9.3 or newer installed.
   - Verify with:
   ```bash
   leo --version
   ```
   - If necessary, update:
   ```bash
   leo update
   ```
   - Installation guide: [Leo Installation](./../../leo/01_installation.md)

### Installation

You can install the `zkml` Python library from PyPI using the following command:
```bash
pip3 install zkml
```

<!-- markdown-link-check-disable -->
Note: On some systems, you may need to use `pip` instead of `pip3`.
Alternatively, you can also install through the `.whl` file or in editable mode from the [GitHub repository](https://github.com/ProvableHQ/python-sdk/tree/master/zkml).
<!-- markdown-link-check-enable -->

### Usage

<!-- markdown-link-check-disable -->
Below is a brief description of the classes and functions provided by the library. Detailed documentation is in progress and will be available soon.
We encourage you to also check out the [examples on GitHub](https://github.com/ProvableHQ/python-sdk/tree/master/zkml/examples).
<!-- markdown-link-check-enable -->

* In a first step, you can receive an object of the class `zkml.LeoTranspiler(model, validation_data)`
    * For the `model` parameter, pass the trained scikit-learn model
    * For the `validation_data` parameter, pass the training or validation dataset. While this parameter is not strictly required, we recommend using it. The dataset is used to compute a fixed-point scaling factor and the required Leo integer types. Using the parameter helps to ensure numerical stability in the inference computation. The larger the dataset, the better.
    * When the model is a scikit-learn multilayer perceptron regressor, there are additional optional parameters you can pass. First, the string `data_representation_type`, which can take the values `int` or `field`. It determines if the data is represented as integers or fields inside the circuit. By default, it is `int`, the `field` mode is experimental. The second parameter you can pass is the boolean parameter `layer_wise_fixed_point_scaling_factor`. By default, it is `true`, which leads to a growing fixed point scaling factor per layer. It can reduce the constraint usage, as there will be no divisions after multiplying two fixed-point numbers. For deep MLP networks (more than 3 layers) or large fixed point scaling factors, however, it may lead to large integers inside the circuit and setting it to `false` may be worth trying out.
* In a second step, you can start the transpilation process through `leo_transpiler.to_leo(path, project_name, model_as_input, fixed_point_scaling_factor)`
    * For the `path` parameter, pass the path where the new leo project should be stored in
    * For the `project_name` parameter, pass the desired name of the leo project.
    * The boolean `model_as_input` parameter is optional and by default `False`. If set to `False`, the model parameters (i.e., thresholds, weights, biases) are hardcoded in the Leo code. If set to `True`, these model parameters are treated as additional circuit inputs.
    * The integer `fixed_point_scaling_factor` parameter is optional and by default `None`. By default, the transpiler automatically computes a recommended fixed-point scaling factor. In certain cases, this scaling factor can be (by a wide margin) too large, which can result in constraint-rich circuits or numerical instability in inference. With the parameter, you can set the fixed point scaling factor manually. We recommend using powers of 2, and trying out smaller powers values such as 8, 16, 32. Generally, a higher fixed point scaling factor leads to more accurate computations at the cost of a higher circuit constraint count.
* After transpiling, you can run the computation of the Leo project through `leo_transpiler.run(input)`, and receive an object of class `LeoComputation`
    * For the `input` parameter, pass the inference data sample or dataset
* Similarly, after transpiling, you can execute the Leo project through `leo_transpiler.execute(input)`, and receive an object of class `ZeroKnowledgeProof`
    * For the `input` parameter, pass the inference data sample or dataset

## Building Python Apps

<!-- markdown-link-check-disable -->
Please check out the [examples on GitHub](https://github.com/ProvableHQ/python-sdk/tree/master/zkml/examples).
Further documentation and tutorials as to how to use the `zkml` Python library will follow soon.
<!-- markdown-link-check-enable -->
