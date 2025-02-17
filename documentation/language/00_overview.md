---
id: overview
title: The Leo Language Reference
sidebar_label: Overview
---

### Statically Typed

Leo is a **statically typed language**, which means we must know the type of each variable before executing a circuit.

### Explicit Types Required

There is no `undefined` or `null` value in Leo. When assigning a new variable, **the type of the value must be
explicitly stated**.

<!-- The exception to this rule is when a new variable inherits its type from a previous variable. -->

### Pass by Value

Expressions in Leo are always **passed by value**, which means their values are always copied when they are used as
function inputs or in right sides of assignments.

## Deprecated Syntax

#### Increment and Decrement

`increment()` and `decrement()` functions are deprecated as of Leo v1.7.0.
Please use the [`Mapping::set()`](#set) function instead.

#### Finalize 

`finalize` and the associated programming model is deprecated as of Leo v2.0.0.
Please use an [`async function`](#async-function) instead.